---
name: creating-github-issues
description: Turn a free-form request, bug report, or finding into a GitHub issue with a consistent template, an executable plan, and correct labels. Use when asked to file, register, open, or write an issue, or to convert a review comment or error report into tracked work. Triggers - create issue, file issue, open a ticket, 이슈 등록, 이슈 작성, 이슈 생성, 티켓 만들기.
---

# Creating GitHub Issues

Investigate the codebase before writing.
An issue that names the affected files and a concrete plan is executable; one that restates the request is not.

## Workflow

1. Classify the request into exactly one type: `Feat`, `Bug`, `Perf`, `Chore`, `Refactor`, `UI`, `UX`, or `Docs`. Ask the user when neither the type nor the scope can be settled from the request.
2. Locate the affected code. For a bug, find the root cause and cite `file:line`. For a feature, find the anchor points where the change lands. For performance, name the measured or suspected bottleneck.
3. Check for duplicates before creating anything: `gh issue list --state open --search "<keywords>" --json number,title --limit 10`. Report a near match and ask whether to proceed.
4. Read `references/issue-template.md` and compose the body from it. Include an Execution Plan in every issue.
5. Apply labels: one primary label derived from the type, one scope label when the repository defines a scope taxonomy, and the agent-authored marker label when the repository uses one.
6. Create the issue with `gh issue create --title ... --body ... --label ...`, passing the body through a quoted heredoc so Markdown survives intact.
7. Report the issue URL, applied labels, key files identified, and the branch name from the plan.

## Rules

- Every issue ships with an Execution Plan. A plan a reader cannot act on is not a plan.
- Every acceptance criterion is testable. "Works correctly" is not a criterion; "returns 404 for an expired token" is.
- Cite paths and line numbers read from the actual repository. Never invent a file path.
- State constraints as formulas or specifications when the number was not measured. Hard-coded magic values from guesswork make the issue unverifiable.
- Omit an optional section entirely rather than leaving it blank.
- Match the repository's human-facing documentation language for the narrative sections, and keep the agent-facing plan sections in English.
- Do not create the issue until type, scope, and affected files are settled.

## Repository Conventions

Discover these once per repository instead of assuming them.

- Labels: `gh label list --limit 100`. Map the type to the closest existing label; do not create labels unless asked.
- Scope taxonomy: derive it from the top-level source directories the repository actually has.
- Marker label: use whatever the repository already applies to agent-authored issues. Skip the marker when none exists.
