# Issue Template Reference

## Title Format

```
Type: Concise description
```

Types: `Feat`, `Bug`, `Perf`, `Chore`, `Refactor`, `UI`, `UX`, `Docs`.

Examples:

- `Perf: Add connection pool timeout`
- `Bug: Vote count wrong on expired topic`
- `Feat: Auto-refresh stores at local midnight`

## Label Mapping

| Type | Primary label |
| --- | --- |
| Bug | `bug` |
| Feat | `enhancement` |
| Perf | `perf` |
| Chore | `chore` |
| Refactor | `chore` |
| UI / UX | `enhancement` |
| Docs | `documentation` |

Map to the labels the repository already has; run `gh label list` first.
When the repository defines scope labels, derive the scope from the affected paths, for example a frontend-only change, a backend-only change, a change spanning both, an infrastructure or CI change, or a shared-package change.

## Language Convention

| Section | Language | Reason |
| --- | --- | --- |
| Background | Repository language | Read by humans for context |
| Current State | Repository language | Read by humans as analysis |
| Task | Repository language | Read by humans as description |
| Acceptance Criteria | Repository language | Read by humans as a checklist |
| Dependencies | English | Bare references |
| Execution Plan | English | Consumed by agents |
| Tasks, Test Strategy, AC Verification, Risks | English | Consumed by agents |
| Key Files | English | Paths are English |

## Body Template

```markdown
## Background

(2-4 sentences: what problem exists and why it matters)

## Current State

(Describe current behavior, cite file:line, include a code snippet when it illuminates the problem)

## Task

(What needs to happen, with the intended approach)

### Key Files

- `path/to/file` - description of the change

## Acceptance Criteria

- [ ] (testable condition)

## Dependencies

- #N - (description; omit the whole section when there are none)

---
## Execution Plan

**Date:** YYYY-MM-DD
**Type:** feat | fix | chore | docs | refactor
**Branch:** <type>/<kebab-case-description>

### Tasks

1. [ ] (specific task + affected file paths)
2. [ ] (specific task + affected file paths)

### Test Strategy

- Test files: (files to create or modify)
- Test cases: (the assertions to write)

### AC Verification

- (mirror each acceptance criterion + how it gets verified)

### Risks

- (at most 3; omit the section when there are none)
```

## Optional Footer

Repositories that distinguish agent-authored issues append a marker line at the end of the body, for example:

```markdown
---
> Generated with an AI coding agent
```

Use the repository's existing marker wording when one is already in use.

## Branch Naming

`<type>/<kebab-case-description>`, for example `fix/vote-count-expired`.
Keep the Execution Plan's branch name identical to the branch the implementer will create.
