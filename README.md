# Skills

Reusable agent skills for everyday development workflow.
Each skill is a plain Markdown file with frontmatter, so it works with any agent that reads a skills directory: Claude Code, Codex, and others.

Skills here are project-agnostic on purpose.
They carry no environment variables, no service names, and no domain rules from the project they were extracted from.
Anything repository-specific is written as something the agent discovers at runtime rather than something baked into the skill.

## Install

### Codex

```shell
npx skills add youngyunjeong/skills
```

Run `/skills` to inspect the collection, then invoke one explicitly with `$creating-github-issues`, or just describe the task and let the agent match.

### Claude Code

Clone once and symlink the skills into the user scope:

```shell
git clone git@github.com:youngyunjeong/skills.git ~/workspace/skills
for d in ~/workspace/skills/skills/*/*/; do
  ln -sfn "$d" ~/.claude/skills/"$(basename "$d")"
done
```

Symlinking rather than copying keeps every project on the same version, so a fix to a skill lands everywhere at once.
Use `.claude/skills/` in a single repository instead when a skill should be scoped to that project.

### Any other agent

Point the agent at `skills/<category>/<skill-name>/SKILL.md`.
The frontmatter carries `name` and `description`; the body is the instruction set.

## Catalog

### Workflow

Repository path: `skills/workflow/`

| Skill | Use it for |
| --- | --- |
| `creating-github-issues` | Turning a request, bug report, or finding into a GitHub issue with an executable plan and correct labels. |
| `checking-app-store-4-3-risk` | Checking an iOS app against the developer's other live and rejected apps on five items before App Store submission, and blocking submission when any item could read as 4.3 spam. |
| `pair-programming` | Step-by-step collaborative implementation in an isolated worktree, with the user reviewing every step and no commit until they say so. |
| `triaging-sentry-issues` | Deciding whether a Sentry alert is stale noise, a tracked bug, or an incident, by checking the event against the build actually in production. |

### Writing

Repository path: `skills/writing/`

| Skill | Use it for |
| --- | --- |
| `minimal-writing-reviewer` | Reviewing, proofreading, and refining Korean and English documents, prose, and UI copy against a project ruleset or a minimal, zero-redundancy baseline. |

## Structure

```
skills/<category>/<skill-name>/SKILL.md     # the skill itself
skills/<category>/<skill-name>/references/  # detail read on demand
skills/<category>/<skill-name>/evals/       # golden test cases and assertions
```

`SKILL.md` stays short enough to load in full.
Anything long - templates, tables, syntax detail - moves to `references/` and gets read only when the workflow reaches it.

## Writing Skills Here

See `AGENTS.md` for the maintenance rules.
