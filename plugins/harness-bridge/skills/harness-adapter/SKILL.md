---
name: harness-adapter
description: "Orchestrate agents on other harnesses. Trigger when task requires use of models that are not natively supported on Claude Code"
---

# Harness Adapter

Claude Code main sessions only. Never self-delegate from a worker.

**Default models** — codex: `gpt-6.1-sol`; pi: `openrouter/deepseek/deepseek-v4.1-flash-1017`. Always pin explicitly.

## Contract

Per task dir: `prompt.md` (goal, paths, constraints, non-goals, proof expected, output shape — worker has ZERO context) → `result.md` + `status` out. Prompts via file where the recipe allows (codex stdin, pi passes `"$(cat …)"` as the arg). Spawn with Bash `run_in_background`; progress arrives via task notifications — don't poll with sleep.

Sandbox: all harnesses run INSIDE it with the recipes below. Sandbox prerequisites: write access to `~/.codex` and `~/.pi/agent` (`sandbox.filesystem.allowWrite`); network access to `chatgpt.com` for codex with a ChatGPT login and `openrouter.ai` for pi via OpenRouter (`sandbox.network.allowedDomains`, or per-command `allowed_domains` in auto mode). Known exceptions (unsandboxed, case-by-case, never wholesale): codex `$imagegen` runs (image backend needs network the sandbox blocks — fails ~5x then dies); `git commit` when SSH signing is configured (signing hangs sandboxed — or pass `--no-gpg-sign` if unsigned is acceptable). Sandboxed Python: prefix `UV_CACHE_DIR="$TMPDIR/uvcache"` and always use `uv run --with pytest python -m pytest` — bare `pytest` hits ImportError anywhere in this setup, not just worktrees.

## Recipes

**codex** (long runs fine; stderr/stdout noisy from blocked MCP hosts, ~25s startup timeouts are normal, not failure; `-c mcp_servers={}` does NOT disable them):

```bash
# inside Claude Code's sandbox (default): codex's own sandbox cannot nest in it, so use danger-full-access —
#   Claude Code's sandbox is the confinement. SSL_CERT_FILE is required: codex's TLS can't reach the macOS trust store there.
cd "$TASK" && SSL_CERT_FILE=/etc/ssl/cert.pem command codex exec --skip-git-repo-check --sandbox danger-full-access -C "$TASK" \
  -m gpt-6.1-sol -c model_reasoning_effort="high" -o "$TASK/result.md" - < "$TASK/prompt.md" > "$TASK/stdout.log" 2>&1

# outside the sandbox (the exceptions above): let codex sandbox itself
cd "$TASK" && command codex exec --skip-git-repo-check --sandbox workspace-write -C "$TASK" \
  -m gpt-6.1-sol -c model_reasoning_effort="high" -o "$TASK/result.md" - < "$TASK/prompt.md" > "$TASK/stdout.log" 2>&1

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

## Execution paths

- **Direct spawn**: interactive iteration, image loops. Background bash, results straight into parent context.
- **Native-subagent wrapper**: use whenever (a) the permission classifier denies a direct external spawn — the wrapper is the sanctioned path, not a workaround; (b) long babysit loops; (c) Workflow fan-outs;
  Spawn `subagent_type: harness-bridge:harness-wrapper` (pinned sonnet + low effort — brainpower lives in the worker, not the wrapper) with prompt = the exact recipe command + verification criteria + resume command. Fallback if that agent type is missing: general-purpose with `model: "sonnet"`. In Workflow scripts: `agent(..., {model: 'sonnet', effort: 'low'})`. Same recipes, same sandbox posture — keeps worker logs out of parent context. Subagents cannot disable the sandbox (classifier hard-denies); recipes don't need it.

**Image generation** → only with codex (`$imagegen` skill, built-in). Direct spawn from the main session, outside the sandbox, with the `workspace-write` recipe (codex's own sandbox does not block the image backend); needs the user's approval, so never unattended or from a subagent. Images land in `~/.codex/generated_images/<session id>/` — name an absolute output path in the prompt so codex copies the file there.
