---
name: harness-adapter
description: "Orchestrate agents on other harnesses (Codex CLI, pi, Antigravity CLI `agy`). Trigger when task requires use of models that are not natively supported on Claude Code, such as GPT, Gemini, or open-weight models"
---

# Harness Adapter

Claude Code main sessions only. Never self-delegate from a worker.

**Default models** — codex: `gpt-6.2-sol`; pi: `openrouter/deepseek/deepseek-v4.1-flash-1017`; agy: `gemini-4-neon` (effort `high`). Always pin explicitly.

**Which harness** — Google Gemini models run on agy (Antigravity CLI, the user's Google AI Pro subscription), not on pi through OpenRouter. OpenAI models run on codex. Open-weight models run on pi.

## Contract

Per task dir: `prompt.md` (goal, paths, constraints, non-goals, proof expected, output shape — worker has ZERO context) → `result.md` + `status` out. Prompts via file where the recipe allows (codex stdin, pi passes `"$(cat …)"` as the arg). Spawn with Bash `run_in_background`; progress arrives via task notifications — don't poll with sleep.

Sandbox: all harnesses run INSIDE it with the recipes below. Sandbox prerequisites: write access to `~/.codex`, `~/.pi/agent` and `~/.gemini/antigravity-cli` (`sandbox.filesystem.allowWrite`); network access to `chatgpt.com` for codex with a ChatGPT login, `openrouter.ai` for pi via OpenRouter, and `*.googleapis.com` for agy (`sandbox.network.allowedDomains`, or per-command `allowed_domains` in auto mode); for agy also `sandbox.network.allowLocalBinding: true` (agy starts a language server on a localhost port and dies with `bind: operation not permitted` without it). Known exceptions (unsandboxed, case-by-case, never wholesale): codex `$imagegen` runs (image backend needs network the sandbox blocks — fails ~5x then dies); `git commit` when SSH signing is configured (signing hangs sandboxed — or pass `--no-gpg-sign` if unsigned is acceptable). Sandboxed Python: prefix `UV_CACHE_DIR="$TMPDIR/uvcache"` and always use `uv run --with pytest python -m pytest` — bare `pytest` hits ImportError anywhere in this setup, not just worktrees.

## Recipes

**codex** (long runs fine; stderr/stdout noisy from blocked MCP hosts, ~25s startup timeouts are normal, not failure; `-c mcp_servers={}` does NOT disable them):

```bash
# inside Claude Code's sandbox (default): codex's own sandbox cannot nest in it, so use danger-full-access —
#   Claude Code's sandbox is the confinement. SSL_CERT_FILE is required: codex's TLS can't reach the macOS trust store there.
cd "$TASK" && SSL_CERT_FILE=/etc/ssl/cert.pem command codex exec --skip-git-repo-check --sandbox danger-full-access -C "$TASK" \
  -m gpt-6.2-sol -c model_reasoning_effort="high" -o "$TASK/result.md" - < "$TASK/prompt.md" > "$TASK/stdout.log" 2>&1

# outside the sandbox (the exceptions above): let codex sandbox itself
cd "$TASK" && command codex exec --skip-git-repo-check --sandbox workspace-write -C "$TASK" \
  -m gpt-6.2-sol -c model_reasoning_effort="high" -o "$TASK/result.md" - < "$TASK/prompt.md" > "$TASK/stdout.log" 2>&1

# -o writes the final message to result.md — tell the worker to return its output as its final message, not to write result.md.

# resume (session id printed as "session id: <uuid>" in stdout; --last works if unambiguous; inside the sandbox use the
#   same SSL_CERT_FILE prefix and sandbox_mode="danger-full-access"):
command codex exec resume <id> --skip-git-repo-check -c sandbox_mode="workspace-write" -o "$TASK/result2.md" - < "$P2"

# resume CANNOT change cwd: -C is rejected, -c cwd= silently ignored — it keeps the original session's dir.
#   Use ABSOLUTE paths for every output file in resume prompts or files land in the old cwd.
# knobs: -c key=value (TOML paths), --sandbox read-only, -p <profile> ($CODEX_HOME/<name>.config.toml), -i <image>
```

**pi** (MUST pass --no-approve or it hangs FOREVER on the trust-project-files prompt when run inside any project; set --thinking <level> [off, minimal, low, medium, high, xhigh, max] according to reasoning effort requested):

```bash
pi -p --no-approve --thinking "max" --session-dir "$TASK/sessions" --model openrouter/deepseek/deepseek-v4.1-flash-1017 "$(cat "$TASK/prompt.md")" > "$TASK/result.md" 2> "$TASK/stderr.log"
# resume: add -c with same --session-dir; knobs: --thinking off..max, --tools read (allowlist, enforced), --system-prompt/--append-system-prompt, --no-session for ephemeral
```

**agy** (Antigravity CLI; `-p` takes the prompt as its own value, so put it LAST — `-p --model x "…"` exits 2 with "-p took --model as its prompt". Print mode needs no trust prompt and runs in agy's auto mode: file edits run without asking. Inside Claude Code's sandbox agy CANNOT run shell commands — it runs each one in a pseudo-terminal, the sandbox blocks that (`failed to create PTY: operation not permitted`), and no Claude Code setting allows it. agy seats are for judges, reviewers, and runners whose job is reading and editing files; say in `prompt.md` that shell commands are unavailable and the orchestrator runs the tests. List models with `agy models`: slugs are `<model>-<effort>`, such as `gemini-4-neon-high`; pass the part before the effort as `--model` and the effort as `--effort`. Each model accepts only its listed levels — `gemini-4-neon` has low..xhigh, `gemini-4-argon` low..high — and anything else fails fast with `invalid model selection`):

```bash
# SSL_CERT_FILE is required inside the sandbox, as for codex (else "tls: failed to verify certificate: x509: OSStatus -26276").
#   Sandbox denials for lh3.googleusercontent.com (profile picture) are harmless noise.
# runner. The workspace is the cwd: reads anywhere work, but writes outside it are SILENTLY refused in print mode
#   (empty response, "denied_actions" in the JSON). Pass --add-dir for every folder outside $TASK the worker must change.
cd "$TASK" && SSL_CERT_FILE=/etc/ssl/cert.pem command agy --model gemini-4-neon --effort high [--add-dir /abs/repo] --output-format json \
  --log-file "$TASK/agy.log" -p "$(cat "$TASK/prompt.md")" > "$TASK/agy.json" 2> "$TASK/stderr.log"; \
  jq -r '.response // empty' "$TASK/agy.json" > "$TASK/result.md"

# read-only seat (judge, reviewer): same command plus deny rules, which agy enforces (the model is told "denied by a deny rule"):
#   --settings '{"permissions":{"deny":["command(*)","write_file(*)","read_url(*)","execute_url(*)","mcp(*)"]}}'

# success = agy.json has "status":"SUCCESS" and no "denied_actions" (exit code 1 and "status":"ERROR" with an "error" text
#   on a bad model or effort). The model that ran is in agy.log:
#   grep 'Propagating selected model override' "$TASK/agy.log"   → label="Gemini 4 Neon (High)"

# resume (conversation id is "conversation_id" in agy.json); repeat --model/--effort/--add-dir/--settings:
cd "$TASK" && SSL_CERT_FILE=/etc/ssl/cert.pem command agy --conversation <id> --model gemini-4-neon --effort high --output-format json \
  --log-file "$TASK/agy2.log" -p "$(cat "$P2")" > "$TASK/agy2.json" 2>> "$TASK/stderr.log"; \
  jq -r '.response // empty' "$TASK/agy2.json" > "$TASK/result2.md"

# never: --sandbox (agy's own terminal sandbox blocks network but NOT file writes outside the workspace, so it is no
#   confinement; inside Claude Code's sandbox commands fail anyway), --dangerously-skip-permissions, --remote-agent, --mode plan for real work
#   (plan mode soft-denies commands and ends the turn early), gemini-pro-orchestrator-* models (they start their own subagents).
# knobs: --print-timeout <dur> (default 0 = wait for the turn), --json-schema <schema or file>, --agent <name>.
```

## Execution paths

- **Direct spawn**: interactive iteration, image loops. Background bash, results straight into parent context.
- **Native-subagent wrapper**: use whenever (a) the permission classifier denies a direct external spawn — the wrapper is the sanctioned path, not a workaround; (b) long babysit loops; (c) Workflow fan-outs;
  Spawn `subagent_type: harness-bridge:harness-wrapper` (pinned sonnet + low effort — brainpower lives in the worker, not the wrapper) with prompt = the exact recipe command + verification criteria + resume command. Fallback if that agent type is missing: general-purpose with `model: "sonnet"`. In Workflow scripts: `agent(..., {model: 'sonnet', effort: 'low'})`. Same recipes, same sandbox posture — keeps worker logs out of parent context. Subagents cannot disable the sandbox (classifier hard-denies); recipes don't need it.

**Image generation** → only with codex (`$imagegen` skill, built-in). Direct spawn from the main session, outside the sandbox, with the `workspace-write` recipe (codex's own sandbox does not block the image backend); needs the user's approval, so never unattended or from a subagent. Images land in `~/.codex/generated_images/<session id>/` — name an absolute output path in the prompt so codex copies the file there.
