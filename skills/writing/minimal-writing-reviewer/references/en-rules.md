# English Writing & UX Copy Reference (`en-rules.md`)

Comprehensive rulebook and replacement dictionary combining:
1. **William Strunk Jr. & E.B. White** — *The Elements of Style*
2. **Joseph M. Williams** — *Style: Lessons in Clarity and Grace*
3. **UX Writing & Technical Microcopy Standards**

Apply these rules whenever reviewing or refining English documents, technical specifications, research notes, or user-facing UI copy. When a project ruleset defines domain-specific terms or wire-format identifiers, preserve those exact identifiers.

---

## 1. Strunk & White — Core Principles (*The Elements of Style*)

1. **Use the active voice (Rule 14)**
   - Name the actor as the subject and use a transitive verb when possible.
   - Use the passive voice only when the receiver of the action is the true topic of the passage or the actor is unknown/irrelevant.
   - *Bad*: `The configuration file is read by the worker at startup.`
   - *Good*: `The worker reads the configuration file at startup.`

2. **Put statements in positive form (Rule 15)**
   - Make definite assertions. Avoid tame, colorless, hesitating, non-committal language.
   - Express even negative facts in positive form rather than double negatives (`not honest` → `dishonest`, `did not remember` → `forgot`, `did not pay any attention to` → `ignored`).
   - Reserve `not` for explicit denial or sharp contrast (`Not charity, but simple justice`).
   - *Bad*: `The token is not valid if it does not have a signature.`
   - *Good*: `The token fails validation without a signature.`

3. **Use definite, specific, concrete language (Rule 16)**
   - Prefer the specific to the general, the definite to the vague, and the concrete to the abstract.
   - Replace vague quantifiers (`several`, `a short period`, `various conditions`) with exact numbers, thresholds, or states.
   - *Bad*: `In the event of a network issue, the client retries several times after a brief delay.`
   - *Good*: `When a request times out, the client retries up to 3 times with a 2-second backoff.`

4. **Omit needless words (Rule 17)**
   - Every word must tell. Cut filler preambles, redundant modifiers, and clause-level padding.
   - Turn subordinate clauses into phrases, and phrases into single words where meaning permits.

5. **Express coordinate ideas in similar form (Rule 19)**
   - Keep parallel grammatical structure across headings, bullet items, button pairs, table columns, and compound clauses.
   - *Bad*: `1. Verify the schema, 2. Running the migration, 3. Check logs.`
   - *Good*: `1. Verify the schema, 2. Run the migration, 3. Check the logs.`

6. **Keep related words together (Rule 20)**
   - Place modifiers immediately next to the words they modify to prevent squinting or dangling modifiers.
   - *Bad*: `Users only can export reports in CSV format.`
   - *Good*: `Users can export reports only in CSV format.`

7. **Place the emphatic words of a sentence at the end (Rule 22)**
   - The end of a sentence is its most prominent position. Lead with context or conditions and land on the key outcome or action.
   - *Bad*: `The upload fails if the file exceeds 50 MB, in most cases.`
   - *Good*: `If the file exceeds 50 MB, the upload fails.`

8. **Write with nouns and verbs, prefer plain words, and do not overstate (Chapter V)**
   - Let strong nouns and verbs carry the weight rather than propping up weak words with adjectives and adverbs (`very`, `quite`, `rather`, `extremely`).
   - Prefer familiar words over ornate ones, and state facts without hype.

---

## 2. Joseph M. Williams — *Style: Lessons in Clarity and Grace*

1. **Align Characters with Subjects and Actions with Verbs**
   - Readers find prose clear when the grammatical **subject** is the main **actor/character** (a person, user, service, or concrete component) and the **verb** names the key **action** that character performs.
   - Avoid **nominalizations** ("zombie nouns"—verbs or adjectives turned into abstract nouns like *utilization*, *implementation*, *investigation*, *occurrence*) paired with weak verbs (*perform*, *conduct*, *make*, *occur*, *take place*, *be*).
   - *Bad*: `An evaluation of the audio stream is performed by the scoring engine to determine pronunciation accuracy.`
   - *Good*: `The scoring engine evaluates the audio stream to measure pronunciation accuracy.`

2. **Old-to-New Information Flow (Topic → Stress Cohesion)**
   - **Begin sentences with familiar ("old") information**—concepts already introduced in the preceding sentence or shared context.
   - **End sentences with unfamiliar ("new") or complex information** that deserves emphasis (the **stress** position).
   - *Bad*: `A new cryptographic signature is generated using the private key after the payload is canonicalized by the client.`
   - *Good*: `The client canonicalizes the payload, then signs it with the private key to produce a cryptographic signature.`

3. **Get to the Main Verb Quickly (Sprawl Control)**
   - Avoid long introductory windups before the subject, and never wedge a long interrupting clause between the subject and its verb.
   - *Bad*: `The worker, after validating the request headers and checking rate-limit quotas in Redis, forwards the payload.`
   - *Good*: `After validating the request headers and checking Redis rate limits, the worker forwards the payload.`

4. **Cut Metadiscourse and Hedging**
   - Remove throat-clearing commentary about your own writing (`It is important to note that`, `In this document, we will explain`, `As a matter of fact`).
   - Remove empty intensifiers (`clearly`, `obviously`, `basically`, `essentially`, `virtually`) and cowardly hedges (`somewhat`, `arguably`, `tends to`, `in a sense`)—either state the fact or state the exact condition/uncertainty.

---

## 3. Banned / Wordy Phrase Replacement Dictionary

### 3.1 Wordy Phrases → Concise Equivalents

| Avoid (Wordy / Padded) | Prefer (Concise) |
| :--- | :--- |
| `the question as to whether` / `whether or not` | `whether` |
| `there is no doubt that` | `no doubt` / `doubtless` |
| `used for [X] purposes` | `used for [X]` |
| `in a [adjective] manner` (e.g., `in a hasty manner`) | `[adverb]` (e.g., `hastily`) |
| `this is a subject that` | `this subject` |
| `the fact that` / `due to the fact that` / `owing to the fact that` | *(omit)* / `because` / `since` |
| `in spite of the fact that` / `regardless of the fact that` | `although` / `even though` |
| `in order to` / `for the purpose of` / `with the aim of` | `to` / `for` |
| `at this point in time` / `at the present time` / `currently` | `now` |
| `prior to` / `subsequent to` | `before` / `after` |
| `in the event that` / `under circumstances in which` | `if` / `when` |
| `is able to` / `has the ability to` / `is capable of` | `can` |
| `it is necessary that` / `there is a requirement to` | `must` / `need to` |
| `a large number of` / `a sufficient number of` | `many` / `enough` |
| `in close proximity to` | `near` |
| `on a daily basis` / `on a regular basis` | `daily` / `regularly` |
| `with regard to` / `in reference to` / `concerning the matter of` | `about` / `for` |
| `in the near future` | `soon` |
| `as a consequence of` | `because of` |

### 3.2 Nominalizations (Zombie Nouns) → Active Verbs

| Avoid (Nominalization + Weak Verb) | Prefer (Strong Verb) |
| :--- | :--- |
| `make an adjustment to` / `perform an adjustment` | `adjust` |
| `perform a validation of` / `conduct validation` | `validate` |
| `carry out an investigation into` | `investigate` |
| `make a decision` / `reach a conclusion` | `decide` / `conclude` |
| `provide a description of` | `describe` |
| `give consideration to` / `take into consideration` | `consider` |
| `make an assumption that` | `assume` |
| `perform initialization of` | `initialize` |
| `cause a reduction in` | `reduce` |
| `achieve an improvement in` | `improve` |
| `conduct a review of` | `review` |
| `provide assistance to` | `help` |

### 3.3 Pretentious / Fancy Words → Plain Words

| Avoid (Pretentious / Jargon) | Prefer (Plain) |
| :--- | :--- |
| `utilize` / `utilization` | `use` |
| `facilitate` | `help` / `ease` |
| `commence` / `initiate` | `start` / `begin` |
| `terminate` / `discontinue` | `end` / `stop` |
| `endeavor` | `try` |
| `ascertain` | `find out` / `check` |
| `demonstrate` (when meaning *show*) | `show` |
| `modification` | `change` |
| `remuneration` | `pay` |
| `leverage` (as a verb meaning *use*) | `use` |
| `optimal` | `best` |
| `approximately` | `about` |
| `subsequently` | `later` / `then` |

### 3.4 Redundant Pairs (Pleonasms) → Single Word

| Avoid (Redundant Pair) | Prefer |
| :--- | :--- |
| `each and every` | `each` or `every` |
| `first and foremost` | `first` |
| `full and complete` | `complete` |
| `basic and fundamental` | `basic` |
| `various different` | `different` or `various` |
| `future plans` | `plans` |
| `past history` / `prior history` | `history` |
| `end result` / `final outcome` | `result` / `outcome` |
| `completely eliminate` | `eliminate` |
| `unexpected surprise` | `surprise` |

---

## 4. UX Writing & Microcopy Standards

1. **Buttons & Action Controls**
   - Begin with a specific **imperative verb** (`Save changes`, `Delete account`, `Export CSV`) that matches the actual outcome.
   - Avoid vague labels (`OK`, `Yes`, `Submit`, `Process`, `Continue`) when a concrete verb clarifies the consequence.
   - In confirmation dialogs, pair the destructive verb with a clear cancel option (`Delete project` / `Keep project` or `Cancel`) rather than `Yes` / `No`.

2. **Error Messages (What happened + Why + Next Step)**
   - Never blame the user (`You entered an invalid email` → `Enter a valid email address, like name@example.com`).
   - Never expose raw error codes, stack traces, or opaque jargon (`Error 500: NullReference` → `Couldn't save your changes. Try again in a moment.`).
   - State: **(1) what failed**, **(2) the reason (if actionable)**, and **(3) how to fix or recover**.

3. **Empty, Loading, and Success States**
   - **Loading**: State the specific action in progress (`Uploading audio…`, `Generating report…`) rather than generic `Loading…` when context matters.
   - **Empty state**: State what belongs here and how to create or find it (`No submissions yet. Upload a recording to start an evaluation.`).
   - **Success state**: Confirm the completed result concisely (`Password updated`, `Report exported`).

4. **Accessibility, Variables, and Length Constraints**
   - Write `aria-label` and `alt` text for meaning and function (`Close dialog`, `Remove filter: Grade 3`), never visual implementation (`X icon`, `Red button`).
   - Do not split sentences around dynamic variables in ways that break grammar or localization plural forms.
