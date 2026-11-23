# harness-bridge

Claude Code can only run subagents on Claude models. This plugin lets a Claude Code session hand a subagent's job to another agent CLI, [Codex](https://github.com/openai/codex), [pi](https://github.com/earendil-works/pi/tree/main/packages/coding-agent) or the Antigravity CLI (`agy`), so the work runs on a non-Claude model.

Its main use is [pstack](https://github.com/cursor/plugins/tree/main/pstack). pstack's Claude Code adapter runs every non-Claude model slug on the session model, so arena runners and cross-judges all end up on Claude. With this plugin, entries such as `codex:gpt-6.2-sol high`, `pi:openrouter/deepseek/deepseek-v4.1-flash-1017 max` or `agy:gemini-4-neon xhigh` in your pstack model config run on those models, and arena can pick a judge from a different model family.

## What's in it

| Component | Name | What it does |
|---|---|---|
| Skill | `harness-bridge:harness-adapter` | Command recipes for codex, pi and agy: task folder layout, sandbox requirements, resume, and image generation. |
| Agent | `harness-bridge:harness-wrapper` | A Sonnet, low-effort subagent that runs one recipe command, waits, and checks the output files itself. The thinking happens in the external model, not in the wrapper. |
| Hook | SessionStart | Adds the pstack routing rule (`hooks/harness-routing.md`) to every session's context. |

The routing rule only applies to role values that start with `codex:`, `pi:` or `agy:` in `~/.claude/rules/pstack-models.md`. Every other value follows pstack's own adapter, so the plugin changes nothing unless you add such entries.

## Requirements

- At least one of: the `codex` CLI, signed in; the `pi` CLI with a provider key (for example OpenRouter); the `agy` CLI, signed in with a Google account that has a Gemini plan such as Google AI Pro.
- If you use Claude Code's sandbox, allow what the CLIs need in `~/.claude/settings.json`:

  ```json
  "sandbox": {
    "filesystem": { "allowWrite": ["~/.codex", "~/.pi/agent", "~/.gemini/antigravity-cli"] },
    "network": {
      "allowedDomains": ["chatgpt.com", "openrouter.ai", "*.googleapis.com"],
      "allowLocalBinding": true
    }
  }
  ```

  `agy` starts a local server on a localhost port and keeps its logs and conversations in `~/.gemini/antigravity-cli`, so it needs `allowLocalBinding` and that folder. Leave out the entries for CLIs you don't use. `allowLocalBinding` lets every sandboxed command listen on local ports, not only `agy`. The recipes set `SSL_CERT_FILE=/etc/ssl/cert.pem` for codex and agy, because neither can reach the macOS certificate store inside the sandbox.

- After every `brew upgrade codex` on macOS, clear the quarantine flag, or the first headless run waits on a Gatekeeper dialog: `xattr -dr com.apple.quarantine /opt/homebrew/Caskroom/codex/<version>`.

## Configuring pstack seats

Write external seats in `~/.claude/rules/pstack-models.md` as `codex:<model> <effort>`, `pi:<provider>/<model> <effort>` or `agy:<model> <effort>`:

```
arena runners: opus high, codex:gpt-6.2-sol high, pi:openrouter/deepseek/deepseek-v4.1-flash-1017 max
arena cross-judge pool: fable high, codex:gpt-6.2-astra high, agy:gemini-4-neon xhigh
```

- The effort is passed to the CLI as written (codex `model_reasoning_effort`, pi `--thinking`, agy `--effort`). Use a level the model accepts; models differ. For agy, `agy models` lists each model once per level, as `<model>-<effort>` (for example `gemini-4-neon-high`), and the `agy:` entry takes the two parts separately.
- Run Gemini models through `agy`, not through pi and OpenRouter: `agy` uses your Google subscription instead of paid OpenRouter credits.
- Families: `codex:` entries are `openai`; `pi:openrouter/<vendor>/...` entries are `<vendor>`; `agy:` entries are the model maker's family (`gemini-...` is `google`, `gpt-oss-...` is `openai`, `claude-...` is `claude`). Arena never picks a judge from the session's own family, so from a Claude session any of these can judge except `agy:claude-...`, and no Claude entry can.
- If an entry fails to launch, arena's own fallback applies, using this skill's default model for that harness, and the final note says so.

To turn the routing off, disable the plugin. The codex, pi and agy entries then fall back to the session model, as pstack's adapter specifies.

## Limits

- **Codex judges are not truly read-only.** Codex's own sandbox cannot run inside Claude Code's, so codex runs with `danger-full-access` and Claude Code's sandbox is its only confinement. The routing rule copies a judge's inputs into its own folder and checks their checksums afterwards. pi judges run with `--tools read`, which pi enforces, and agy judges run with deny rules for shell commands, file writes, web access and MCP tools, which agy enforces.
- **agy cannot run shell commands inside the sandbox.** agy runs every command in a pseudo-terminal, and Claude Code's sandbox blocks pseudo-terminals with no setting to allow them. agy seats can read and edit files, so they work as judges, reviewers, and runners that edit files, but they cannot run tests; the orchestrator runs them. agy's own `--sandbox` flag is no substitute: it blocks network access but not file writes.
- **agy writes only inside its working folder.** An agy runner that must change files elsewhere, such as a git worktree, needs `--add-dir` for that folder. Without it agy refuses the write and still reports success, so the wrapper checks `denied_actions` in agy's JSON output.
- **Image generation needs your approval.** Codex's `$imagegen` needs network access the sandbox blocks, so it runs from the main session outside the sandbox. It cannot run unattended or from a subagent.

## Verified

Tested with Claude Code on macOS, Codex CLI 0.166.0 and pi, in pstack arena runs:

- codex and pi runners, and codex and pi judges, ran on the configured models, confirmed from codex's output and pi's session files;
- judges were blind to which model wrote which candidate;
- with the routing rule removed, codex and pi seats fell back to the session model and the judge was skipped;
- a model codex rejects fell back to the default model, with the failure reported;
- codex `$imagegen` produced the requested image outside the sandbox.

Tested with agy (Antigravity CLI) 1.5.15 inside Claude Code's sandbox, signed in with Google AI Pro:

- a judge seat ran through the wrapper subagent on Gemini 4 Neon (High), confirmed from agy's log, and returned its verdict;
- the judge's deny rules blocked its file write and shell command, and the inputs' checksums were unchanged;
- a runner wrote files in an `--add-dir` folder and resumed with `--conversation`; its shell commands failed with the pseudo-terminal error above;
- a model or effort level agy does not offer failed at once with an `invalid model selection` error.
