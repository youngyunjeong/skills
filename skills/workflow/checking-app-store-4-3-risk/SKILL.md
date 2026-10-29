---
name: checking-app-store-4-3-risk
description: Run a five-item pre-submission check that judges whether an iOS app could be rejected under App Store Review Guideline 4.3 (Spam) as similar to the same developer's other apps, and block submission when any item fails. Use before submitting or resubmitting an app to App Store review when the developer ships more than one app, reuses code or content from an earlier app, or has received a 4.3 rejection. Triggers - 4.3 precheck, spam rejection check, similar binary metadata concept, pre-submission similarity check, 4.3 사전 점검, 스팸 리젝 점검, 제출 전 유사성 점검, 앱스토어 제출 전 점검.
---

# Checking App Store 4.3 Risk

Apple rejects an app under Guideline 4.3 when a reviewer reads it as a copy of something already on the store, including the developer's own apps.
The rejection notice names a shared "similar binary, metadata, and/or concept" and does not say which app was the match.
This skill compares the candidate build and listing against every app the developer has shipped or had rejected, item by item, and stops the submission when any item fails.

## Basis

Only the public guideline wording is used as the basis:

- 4.3(a): "Don't create multiple Bundle IDs of the same app."
- 4.3(b): "Don't submit apps that are indistinguishable from what's already widely available."
- 4.3(b) also warns that "Repeated submissions of this kind may lead to removal from the Apple Developer Program."

Re-read the current text at https://developer.apple.com/app-store/review/guidelines/#spam before each run, because Apple revises it.

## Input

Ask the user for anything missing; never guess a comparison app.

- The candidate: app name, version, build, the shipped binary or archive, the in-app screens, the content set (for example a text corpus), and the full listing (name, subtitle, description, keywords, category, screenshots, in-app purchase setup).
- The comparison set: every other app by the same developer, each marked live, rejected, or rejected-then-withdrawn, with the same material for each where available.

Keep withdrawn submissions in the comparison set.
Assume the rejection record survives a withdrawal; Apple has not published otherwise.

## Rules

- Judge only what the reviewer receives: the binary, the in-app screens, the content, and the listing. A shared repository, commit history, or internal document is not evidence either way.
- Record exactly one verdict per item: pass or fail. An item that cannot be settled is a fail, and the report states why.
- Cite evidence as a file, a screen, or listing text. Mark any judgment that was not measured as an assumption.
- After any fix, rerun all five items. A fix in one place can create an overlap in another.
- Any failing item means no submission. A passing concept item never offsets a failure elsewhere.
- Submission still needs the human owner's explicit approval after all five items pass.

## Workflow

1. Collect the input and confirm the comparison set with the user.
2. Run the five checks below against every app in the comparison set, one pair at a time.
3. Fill the report from `references/report-template.md`.
4. When any item fails, list what to change for each failure, apply the fixes as separate work, and return to step 2.
5. When all five pass, hand the report to the owner and ask for submission approval.

## The Five Checks

### 1. Lineage

Fail when traces of a rejected app reach the reviewer.

- Binary names: types, targets, modules, widget kinds, or bundle names contain the rejected app's name. Search the built binary's strings for the rejected name, not only the source.
- Content: the candidate reuses the rejected app's content set. Renaming or regrouping categories does not count as change when the underlying items are the same.
- Listing: rejected listing text, category or theme names, featured examples, or the first screenshot's layout remain.

Reusing a repository or project file is not a failure by itself; only what ships counts.

### 2. Skeleton

Fail when the candidate's screen flows follow another of the developer's apps as a template.
Compare these four flows against each comparison app:

- Onboarding: the order and purpose of its steps.
- Paywall: its structure, meaning layout, plan presentation, and call to action.
- Theme or content picker: how the user chooses what they see.
- Widget guide: how the app teaches widget setup.

Fail when two or more of the four match a single comparison app.
A flow matches when, with app names hidden, a reviewer could not tell the two apart.
Do not count matching screen file names or code-line similarity: approved sibling apps can share file names, and a rejected app can share none.

### 3. First Impression

Fail when either holds:

- The core action is not visible in screenshots 1 and 2 and on the app's first screen.
- Two or more of onboarding, paywall, widget, and the last screenshot share the same template copy or layout with another of the developer's apps.

Judge by placing the two apps' screens side by side.

### 4. Concept

Write one line per app: who it is for, what they want, and what they do in the app.
Fail when the candidate shares both the core action and the target audience with another app.
A shared feature such as handwriting input, widgets, or a subscription is not a failure.

### 5. Metadata

Fail when any holds:

- The listing reuses a rejected app's theme names, people or work names, or text. Treat three or more consecutive matching words as reuse, excluding legal-link lines.
- The description structure matches another of the developer's apps, for example the same section headers, bullet pattern, and closing line.
- The last screenshot carries the same message as another of the developer's apps.
- The primary category is the same as another app and two or more keywords overlap. This threshold is an assumption; record any category or keyword overlap even when the item passes.

Also record the in-app purchase setup (product mix, product identifier pattern, subscription group naming).
Approved sibling apps often share it, so treat it as a weak signal: it fails the item only alongside another overlap in this item.

## Report

Lead with the decision: submission request allowed, or blocked with the failing items listed.
Then give the five-row table from `references/report-template.md` with evidence and the fix for each failure, followed by open questions and assumptions.
