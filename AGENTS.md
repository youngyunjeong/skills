# Repository Guidelines

Follow global defaults; this file contains only repository-specific additions.

## Purpose

This repository holds skills that are reusable across projects.
A skill belongs here only when it works in a repository the author has never seen.

## Portability

- No project names, service names, domain rules, or credentials.
- No hard-coded build commands. Tell the agent to discover the repository's own gates instead of asserting `make test` or a specific package manager.
- No agent-specific tool names as requirements. Name a capability ("the agent's structured question tool", "the Sentry MCP tools when available") and give a fallback that works without it.
- No hard-coded branch names. Say "the integration branch" or "the production branch" and let the repository supply the value.
- When a skill needs a repository-specific value, add a short discovery step that finds it at runtime.

## Writing Style

- Write documentation and skill bodies in English. Korean belongs only in a `description` trigger phrase, so Korean prompts still match.
- Lead with the most important information and drop anything repeated.
- Put each prose sentence on its own source line.
- Make every rule specific and verifiable. "Be careful with migrations" is not a rule; "sequence the migration against the deploy deliberately" is.
- Explain the main idea before the detail.

## Skill Format

- Use lowercase kebab-case directory names and name the entry file exactly `SKILL.md`.
- Frontmatter carries `name` and `description` only.
- The `description` states what the skill does and when to reach for it, and ends with trigger phrases in both English and Korean.
- Keep `SKILL.md` loadable in one read. Move templates, tables, and syntax detail into `references/` and reference them from the step that needs them.
- Keep supporting material inside the skill directory under `references/`, `scripts/`, or `assets/`.
- Prefer `<verb-ing>-<object>` names, but keep an established practice name when the gerund would distort the meaning.

## Repository Structure

- Active skills live at `skills/<category>/<skill-name>/SKILL.md`. Category directories are organizational only.
- `.agents/skills` is a discovery symlink, not a second copy. Edit under `skills/`.
- Update the README catalog whenever a skill is added, renamed, recategorized, or changes its trigger.

## Boundaries

- For review, explanation, or diagnosis requests, inspect and report without changing files.
- For change requests, make the in-scope edits and run the relevant safe checks without asking first.
- Ask before writing to external systems, taking destructive actions, or widening scope.

## Testing

This repository has no automated test suite; the skills are prose.
Verification is running the skill against a real repository and confirming each step is executable there.
Run `git diff --check` and read the final diff for unintended scope before opening a pull request.
