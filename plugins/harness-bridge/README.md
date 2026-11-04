# harness-bridge

Claude Code can only run subagents on Claude models. This plugin lets a Claude Code session hand a subagent's job to another agent CLI, [Codex](https://github.com/openai/codex) or [pi](https://github.com/badlogic/pi-mono), so the work runs on a non-Claude model.

Its main use is [pstack](https://github.com/cursor/plugins/tree/main/pstack). pstack's Claude Code adapter runs every non-Claude model slug on the session model, so arena runners and cross-judges all end up on Claude. With this plugin, entries such as `codex:gpt-6.1-sol high` or `pi:openrouter/z-ai/glm-5.3-flash low` in your pstack model config run on those models, and arena can pick a judge from a different model family.

## What's in it

| Component | Name | What it does |
|---|---|---|
| Skill | `harness-bridge:harness-adapter` | Command recipes for codex and pi: task folder layout, sandbox requirements, resume, and image generation. |
| Agent | `harness-bridge:harness-wrapper` | A Sonnet, low-effort subagent that runs one recipe command, waits, and checks the output files itself. The thinking happens in the external model, not in the wrapper. |
| Hook | SessionStart | Adds the pstack routing rule (`hooks/harness-routing.md`) to every session's context. |

The routing rule only applies to role values that start with `codex:` or `pi:` in `~/.claude/rules/pstack-models.md`. Every other value follows pstack's own adapter, so the plugin changes nothing unless you add such entries.

## Requirements

- The `codex` CLI, signed in, and/or the `pi` CLI with a provider key (for example OpenRouter).
- If you use Claude Code's sandbox, allow what the CLIs need in `~/.claude/settings.json`:

  ```json
  "sandbox": {
    "filesystem": { "allowWrite": ["~/.codex", "~/.pi/agent"] },
    "network": { "allowedDomains": ["chatgpt.com", "openrouter.ai"] }
  }
  ```

- After every `brew upgrade codex` on macOS, clear the quarantine flag, or the first headless run waits on a Gatekeeper dialog: `xattr -dr com.apple.quarantine /opt/homebrew/Caskroom/codex/<version>`.

## Configuring pstack seats

Write external seats in `~/.claude/rules/pstack-models.md` as `codex:<model> <effort>` or `pi:<provider>/<model> <effort>`:

```
arena runners: opus medium, codex:gpt-6.1-sol high, pi:openrouter/deepseek/deepseek-v4.1-flash-1017 max
arena cross-judge pool: pi:openrouter/z-ai/glm-5.3-flash low, codex:gpt-6-astra low
```

- The effort is passed to the CLI as written (codex `model_reasoning_effort`, pi `--thinking`). Use a level the model accepts; pi models differ.
- Families: `codex:` entries are `openai`; `pi:openrouter/<vendor>/...` entries are `<vendor>`. Arena never picks a judge from the session's own family, so from a Claude session any of these can judge, and no Claude entry can.
- If an entry fails to launch, arena's own fallback applies, using this skill's default model for that harness, and the final note says so.

To turn the routing off, disable the plugin. The codex and pi entries then fall back to the session model, as pstack's adapter specifies.

## Limits

- **Codex judges are not truly read-only.** Codex's own sandbox cannot run inside Claude Code's, so codex runs with `danger-full-access` and Claude Code's sandbox is its only confinement. The routing rule copies a judge's inputs into its own folder and checks their checksums afterwards. pi judges run with `--tools read`, which pi enforces.
- **Image generation needs your approval.** Codex's `$imagegen` needs network access the sandbox blocks, so it runs from the main session outside the sandbox. It cannot run unattended or from a subagent.

## Verified

Tested with Claude Code on macOS, Codex CLI 0.166.0 and pi, in pstack arena runs:

- codex and pi runners, and codex and pi judges, ran on the configured models, confirmed from codex's output and pi's session files;
- judges were blind to which model wrote which candidate;
- with the routing rule removed, codex and pi seats fell back to the session model and the judge was skipped;
- a model codex rejects fell back to the default model, with the failure reported;
- codex `$imagegen` produced the requested image outside the sandbox.
