# Session Notes

Keep concise. Update before finishing when context should survive the next prompt.

## Goal

Add project-local skills as an alternative to task layers without deleting existing tasks.

## Source

User request on 2026-06-18: create the necessary skill files and update README instructions for using skills as an alternative to tasks.

## Decisions

- Kept all existing `tasks/*.md` files unchanged.
- Added project-local Codex skills under `.agents/skills/`.
- Added `skills.sh.json` to group skills for the Skills.sh repository page.
- Updated `AGENTS.md` so skills are preferred when supported and task layers remain the fallback.
- Updated `README.md` with skills-mode usage and CLI examples.

## Changed

- `.agents/skills/*/SKILL.md`
- `.agents/skills/*/agents/openai.yaml`
- `AGENTS.md`
- `README.md`
- `skills.sh.json`
- `runtime/session-notes.md`

## Validation

- Ran `quick_validate.py` for all nine `.agents/skills/*` folders; all are valid.
- Ran `python3 -m json.tool skills.sh.json`; JSON is valid.
- Ran a placeholder scan across the new skills and docs; no scaffold placeholders remain.
- Ran `git diff --check`; no whitespace errors.

## Open Items

- Decide later whether to keep tasks and skills permanently in parallel or eventually deprecate task-layer files.

## Risks

- Skills and tasks can drift if both are maintained separately; future changes should update both or declare one source of truth.

## Next

- Use skills mode for new workflows, or keep task layers as the fallback compatibility path.
