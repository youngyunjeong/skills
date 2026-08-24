---
name: triaging-sentry-issues
description: Triage a Sentry error alert against the code actually running in production, then resolve it, file a tracked issue, or escalate to the user. Use when handed a Sentry issue link or an error alert and asked what to do about it. Triggers - triage this Sentry error, what do we do with this alert, review this exception, 센트리 에러 확인, 에러 알림 분류.
---

# Triaging Sentry Issues

Most alerts are not bugs in the code that is currently deployed.
Establish which build produced the event before deciding anything, then take exactly one action: resolve, file, or escalate.

Run this in the main session.
Sentry access needs credentials, and delegated subagents usually do not carry them.

## Input

A Sentry issue URL.
The organization and project slugs are in the URL; read them from there rather than assuming them.
Ask the user for the link when none was given.

## Workflow

### 1. Fetch the issue

Use the Sentry MCP tools when the agent has them.
Otherwise call the Sentry Web API with a token from the environment:

```shell
curl -sS -H "Authorization: Bearer $SENTRY_AUTH_TOKEN" \
  "https://sentry.io/api/0/organizations/<org>/issues/<issue-id>/"
```

Extract the error message and the first-party stack frames, the culprit or transaction, the **release** (commit SHA), the environment, the browser, OS and device tags, and the occurrence counts with first and last seen.
Note the time spread: a burst of events within milliseconds is one page load, not many failures.

### 2. Check the event against production reality

This is the decisive step.
The question is whether the event came from the build currently in production.

```shell
git log -1 --format='%H %ci %s' <release-sha>   # when was this release built
git fetch origin <production-branch>
git log origin/<production-branch> -3            # what is production now
```

When they differ, confirm what production actually serves rather than trusting the branch:

```shell
curl -s '<the failing url>' | grep -oE '[0-9a-f]{40}' | sort -u
```

Then check whether the failing code path still exists at `HEAD`: the dependency in the manifest, the file in the stack trace, the branch that threw.
A runtime or framework migration can make an error path structurally impossible in current builds even though the issue is still open.

### 3. Classify into exactly one bucket

**A. No action - resolve.** Any of:

- The event's release is stale and the current production build no longer contains that code path.
- A single-client burst: a few events in a tiny window, zero or one users, no recurrence.
- Recoverable noise: a fallback that still renders, bot traffic, an ancient browser.

**B. Non-urgent fix - file an issue.** All of:

- The bug is real and reproducible in the current production build.
- Blast radius is limited: few users, a non-core flow, graceful degradation.

**C. Urgent - escalate.** Any of:

- The current release tag plus growing occurrences or many users affected.
- A core flow broken: authentication, the primary write path, or pages failing to render at all.
- Data integrity risk: failing writes, constraint violations.

### 4. Act

**A.** Resolve the Sentry issue with a one-paragraph rationale that includes the release comparison and why no code action follows.
The rationale lands in the activity feed.
Note in the report that Sentry reopens the issue as regressed if the same signature recurs on a new release.

**B.** File a GitHub issue using the `creating-github-issues` skill rather than a bare `gh issue create`, carrying the error summary, the Sentry link and short ID, the root-cause findings, and the affected files from step 2.
Leave the Sentry issue unresolved and record the GitHub issue link on it.
Put the Sentry short ID in the issue body so the fixing pull request closes it automatically.

**C.** Take no automatic action: no resolve, no issue, no code changes.
Report immediately with what is broken, how many users are affected, since which release, and a recommended response, then wait for the user's decision.

### 5. Report

Lead with the verdict.

- Conclusion: which bucket, and the action taken.
- Evidence: the release comparison, the occurrence scale in events, users and time spread, and whether the code path still exists.
- Follow-up: the regression reopen condition (A), the issue number created (B), or the decision awaited (C).

## Rules

- Never resolve on occurrence counts alone. A stale release is the justification; a low count is not.
- Never change code as part of triage. Triage decides; fixing is separate work.
- Prefer the issue's own occurrence statistics when event search is unavailable on the plan.
