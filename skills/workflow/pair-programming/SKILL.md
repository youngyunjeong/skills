---
name: pair-programming
description: Collaborative implementation mode where the agent writes code in an isolated worktree and the user reviews every step, with all ambiguity resolved up front and no commit until the user says so. Use when the user wants to work through a change together, step by step, rather than delegating it autonomously. Triggers - pair program with me, let's work on this together, step by step with review, 페어 프로그래밍, 같이 코딩, 함께 작업.
---

# Pair Programming

The user is the reviewer.
Resolve every ambiguous decision before writing code, report scope before changing it, and stop before committing.
Do not delegate to subagents in this mode; the point is that the user sees each step.

## Core Rules

1. Keep asking until every ambiguous decision is resolved. No code changes before that.
2. Re-confirm the user's latest intent at the start of each task.
3. Report the work scope and wait for an explicit OK.
4. Stop before commit and wait for an explicit instruction to commit.

## Workflow

### 1. Load the issue (only when an issue number is given)

Run `gh issue view <number> --comments` and read the body, the acceptance criteria, any existing plan, and every comment.
Decisions recorded in comments override the body.
Summarize the goal, the acceptance criteria, and a proposed `feat/<slug>` back to the user.
This stays collaborative: do not auto-close on feasibility doubts, raise them in step 2 and let the user decide.

### 2. Resolve ambiguity

Identify the decisions that need user input and compress them into two or three core questions.
Use the agent's structured question tool when one is available; otherwise ask in plain text and wait.
Do not slice the questions too finely, and stop to re-ask when a new constraint surfaces mid-discussion.

### 3. Report the work scope

List the files to change, the behavioral changes, and the explicit out-of-scope items in one summary.
Get the user's OK before continuing.

### 4. Track the steps

For multi-step work, keep a visible task list with one step per task and mark each one done as it lands.
Skip the list for a trivial one- or two-file change.

### 5. Prepare an isolated worktree

Create the worktree just before the first edit, not at session start.

```shell
git status --short
```

Stop and notify the user when the main worktree already has stray changes; never stash or discard them.
Otherwise ask for a short kebab-case slug, then branch from the integration branch:

```shell
git worktree add -b feat/<slug> ../<repo>-feat-<slug> <integration-branch>
```

Copy gitignored environment files into the new worktree.
`git worktree add` does not materialize ignored untracked files, so builds, migrations, and end-to-end scripts inside the worktree fail with missing variables until this runs.

```shell
for f in $(git ls-files --others --ignored --exclude-standard | grep -E '(^|/)\.env'); do
  mkdir -p "../<repo>-feat-<slug>/$(dirname "$f")"
  cp "$f" "../<repo>-feat-<slug>/$f"
done
```

All later edits target paths under the worktree.
Use `git -C ../<repo>-feat-<slug> <command>` instead of changing directories.

### 6. Change the code

Prefer red-green-refactor: a failing test first, the minimal implementation second, cleanup third.
For markup or styling work where a unit test carries no signal, substitute a build plus the user's visual verification.
Make small surgical edits; do not reorganize unrelated code.

### 7. Verify

Run the repository's own gates inside the worktree, in this order: lint, type check, tests, build.
Discover the commands from the repository's task runner or package manifest rather than assuming them.
A skipped gate is reported, not silently dropped.

### 8. Summarize before commit

```markdown
## Work complete (before commit)

### Changes ({file count}, +added/-removed)

| File | Description |
| --- | --- |
| ... | ... |

### Behavior

{What changes from the user's perspective}

### Verification

- tests {N}/{N} passed
- build OK

### Please verify

1. ...

OK to commit?
```

### 9. Commit

Wait for an explicit instruction. Never auto-commit.

- Subject: `type(scope): short description`.
- Body: the why, not the what. Use a quoted heredoc to preserve formatting.
- Add `Closes #<number>` when step 1 loaded an issue.
- Follow the repository's own attribution and trailer rules.

### 10. Merge fast-forward only

```shell
git checkout <integration-branch>
git merge --ff-only feat/<slug>
```

When the fast-forward fails because the integration branch advanced, rebase the feature branch in the worktree and retry.
Stop and ask the user when the rebase conflicts; never auto-resolve silently.
Do not fall back to a merge commit.

### 11. Hold the push

Push only on explicit instruction.

### 12. Clean up

```shell
git worktree remove ../<repo>-feat-<slug>
git branch -d feat/<slug>
```

Issue each git command as its own invocation rather than chaining them, so a failure is visible at the step that caused it.
