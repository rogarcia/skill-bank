# External-harness seats for pstack

This rule extends pstack's Claude Code adapter (`docs/harness/claude-code.md`, Models table). It applies only to role values that start with `codex:`, `pi:` or `agy:` in `~/.claude/rules/pstack-models.md`. All other values follow the adapter unchanged.

## Value forms

- `codex:<model> <effort>`, such as `codex:gpt-6.2-sol high`, runs the seat on the Codex CLI.
- `pi:<provider>/<model> <effort>`, such as `pi:openrouter/deepseek/deepseek-v4.1-flash-1017 max`, runs the seat on the pi CLI.
- `agy:<model> <effort>`, such as `agy:gemini-4-neon high`, runs the seat on the Antigravity CLI (`agy`). `<model>` is an `agy models` slug without its effort suffix.

For these values, do not apply the adapter row that maps a non-Claude slug to "omit `model`". The seat is a real non-Claude seat and must not fall back to the session model unless its spawn fails (see Failure).

## Families

pstack skills that compare families (arena Phase A step 3 and Phase C) use these families in addition to their own:

- `codex:` entries belong to the `openai` family.
- `pi:openrouter/<vendor>/...` entries belong to the `<vendor>` family, such as `deepseek`. Other `pi:<provider>/...` entries belong to the `<provider>` family.
- `agy:` entries belong to the family of the company that makes the model: `agy:gemini-...` is `google`, `agy:gpt-oss-...` is `openai`, and `agy:claude-...` is `claude`.

Except for `agy:claude-...`, none of these is the `claude` family, so they are valid cross-judges for a Claude parent. An `agy:claude-...` entry is a Claude model and can never judge a Claude parent.

## Spawning a seat

1. Load the `harness-bridge:harness-adapter` skill and use its recipe for the harness. Pin the model from the value. Map the effort: codex gets `-c model_reasoning_effort="<effort>"` (use `xhigh` for `max`); pi gets `--thinking <effort>` exactly as written; agy gets `--model <model> --effort <effort>` exactly as written. The configured level is the user's choice for that model, so never change it, including when a pstack budget step would.
2. Create one task directory per seat under the scratchpad, named by seat letter only (`seat-a`, `seat-b`, ...), never by model, so judges stay blind.
3. Write `prompt.md` with everything the native subagent would have received, inlined: the task, the rubric or grounding with absolute paths, and the output instructions. The worker has no access to this conversation or to pstack's skills.
4. The seat's output path is `<task dir>/result.md`. Use that path wherever the pstack skill refers to the seat's output. The recipe writes that file from the worker's final message (codex `-o`, pi stdout, agy's JSON `response` field), so `prompt.md` must tell the worker to return its complete output as its final message and not to write `result.md` itself.
5. Read-only seats (judges, reviewers): copy the inputs into the seat's own task directory as `candidate-a.md`, `candidate-b.md`, and so on, and point the prompt only at those copies. Seat directories hold logs and session files that name the models, so never point a judge at them. pi runs with `--tools read`. agy runs with the recipe's `--settings` deny rules, which agy enforces. Codex cannot use `--sandbox read-only` inside Claude Code's sandbox, so it runs with `--sandbox danger-full-access` and Claude Code's sandbox is its only confinement; for a codex read-only seat, also record the copies' checksums before the spawn and add "the inputs' checksums are unchanged" to the wrapper's verification criteria.
6. agy seats cannot run shell commands inside Claude Code's sandbox. For an agy runner, say in `prompt.md` that shell commands are unavailable, and run the tests yourself after the seat finishes. agy writes only inside its working folder unless it gets `--add-dir`, and it refuses other writes without failing. For an agy runner that must change files outside its task directory, such as a worktree, add `--add-dir <absolute path>` for each such folder.
7. Finish writing `prompt.md` and copying any inputs before spawning. Spawn through `subagent_type: harness-bridge:harness-wrapper` with the exact command, the verification criteria (`result.md` exists, is non-empty, and answers the prompt; for agy also `agy.json` has `"status":"SUCCESS"` and no `denied_actions`), and the resume command. Spawn wrapper seats in the same message as native seats, with `run_in_background: true`.

## Failure

If the wrapper reports that the spawn or verification failed, treat it as the pstack skill's "the Task tool rejects the entry" case and follow that skill's fallback, then say so. Where that fallback calls for the family's default model, use the `harness-bridge:harness-adapter` skill's default model for the same harness.

## Reporting

In the skill's final note, list each seat with its harness, model, task directory, and whether it ran or fell back.
