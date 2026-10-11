# ProgRSS resilient work design

11 October 2026. Proposed architecture incorporating the decision to preserve shareable work publicly and privately at the same time. This document specifies the next build; it does not claim the system is implemented or its acceptance tests have run.

## The decision

Make the workspace replaceable and preserve useful fragments immediately, before everyone agrees about them. Let workers investigate, challenge, repair, and combine results within the agreed goal. The coordinator should spend attention on consequential choices and final publication, rather than reconstructing every finding.

Use the existing repositories as two eligible destinations for the same shareable checkpoint. Start their preservation operations independently. One complete, verified copy means **preserved**; two mean **replicated**. An unavailable private repository must not hold up eligible public preservation, or vice versa.

Keep correctness separate. A failed experiment can be preserved. An unreviewed public checkpoint can be labeled provisional. Neither is an accepted product release or evidence that the deployed application works.

## Minimal version one

Start with five record families, ordinary files, and the existing guarded publication route:

1. **START-HERE** identifies the format, known checkpoint identities, recovery recipe, and next useful action.
2. **STATE** is a small current index of open work, active attempts, accepted candidates, and each destination's outcome. It can be rebuilt from retained records.
3. **Job cards** state one bounded goal, pinned inputs, owned paths, acceptance criteria, current attempt, and resume step.
4. **Result bundles** contain the actual useful files, a hash manifest, concise conclusions, evidence, and limitations. Reviews and combined candidates use the same bundle format with a different result kind.
5. **Publication records** retain the intended bundle, destination, attempt, exact commit, readback outcome, and unresolved work. Intent and later receipt remain distinguishable.

Use stable job, attempt, result, and operation IDs. Submitted bytes are immutable; corrections produce a new result. The same ID and digest is a duplicate acknowledgment, not new work. The same ID with different bytes is a conflict to resolve.

No database, service, scheduler, new credentials, or permanent six-worker dependency is needed. Detailed recovery utilities and transport tuning can follow the first useful end-to-end checkpoint.

## How useful work flows

A worker takes a bounded job from a pinned base, works in its owned location, and freezes a useful fragment with its manifest and next step. Intake checks identity, dependency completeness, and disclosure eligibility, then immediately offers the fragment for preservation. Technical disagreement does not delay safe custody.

A non-author reviews the result against its contract. Small repairs and ordinary disagreements stay with the workers; an uninvolved reviewer can adjudicate a disputed slice using the original evidence. Votes do not replace evidence. Workers assemble compatible results into a new candidate and check changed interactions. Different files can still depend on incompatible assumptions.

The coordinator receives one compact packet: exact candidate identity, included results, review coverage, limitations, destination manifest, and any consequential decision. It makes the final canonical integration decision and performs the authorized publication. It need not repeat every investigation. User-intent changes, permission issues, material disclosure concerns, and unresolved release risks still reach it promptly.

## Fast custody with independent copies

The shareable core can include ordinary ProgRSS source, findings, evidence, work state, and the user's project discussion. Each complete copy includes enough recovery instructions and open-work state to continue without the other repository.

Public eligibility still excludes credentials, protected instructions, third-party confidential information, and other disclosures lacking the required authority. Inspect the actual final bytes before any public object upload. If a public projection omits material, give it a separate identity and coverage. A public source file that requires private-only archive parts is not a complete replica of that archive.

One publisher can progress both destination operations concurrently while serializing each target ref. Preserve the existing complete-tree, allowed-change, parent, candidate-byte, fresh-reference, and non-force guards. Use the available connector's advertised expected-head check against the pinned baseline; do not claim unverified implementation details or cross-repository atomicity. Native GitHub REST non-force updates are documented as fast-forward checks. [GitHub reference API](https://docs.github.com/en/rest/git/refs#update-a-reference)

A commit-created response is not custody. Acknowledge only after the intended reachable commit and all required bytes are read back and verified. Record which copy succeeded and which remains pending. Save the observation receipt in the next useful batch or a final small receipt batch; immutable intent and remote content allow it to be reconstructed if the local receipt disappears.

An uncertain write triggers readback and reconciliation before retry. Never repeat a verified copy merely because its partner failed. Public checkpoint storage also remains separate from promoting a preview or replacing the primary site.

## Recovery and ownership

A replacement session reads the durable entry point, selects an explicit verified checkpoint, reconstructs into a fresh authorized directory, and validates the required bytes before using them. It restores unfinished work and resume instructions alongside accepted results. Old attempt outputs remain useful candidates but cannot silently replace current work.

There is one authorized publisher at a time, not one immortal process. After lost coordinator state, reconcile in-flight effects and establish ownership before new writes. If an older publisher may still be active, pause publication. A private epoch or check-before-push cannot fence an already in-flight write to another repository.

The incident's verified 3,547 logical files and 181 dependencies establish preservation evidence. They do not explain the missing directory or prove full application recovery. Report byte recovery, environment readiness, executed tests, and application acceptance separately. Restored instructions do not create new authority to execute code.

Both repositories share GitHub and an account, so their redundancy mainly covers repository-level trouble. A milestone bundle through the existing authorized Library/export route can add another recovery option without making a third service or new setup mandatory.

## Proposed operating defaults

Enqueue useful fragments immediately and flush small ready batches promptly. The initial ten-minute first-copy target is an age alert and engineering objective, not an intentional waiting period or availability promise. A thirty-minute oldest-unpreserved guard stops admission of more loss-bearing production while custody is blocked.

Start with at most three production jobs and two globally prepared custody packets, including one in flight. Shift available capacity toward review, assembly, or preservation when those queues grow. About 5 MiB or 50 changed files is a batching hint; required dependencies must never be omitted to fit it. Track first-copy exposure separately from missing second copies, and keep both backlogs bounded. Tune from measured latency and effort at risk.

## First build increment and acceptance

When implementation is requested, build one thin vertical slice: one useful direct-file result, its job card and resume step, one shared manifest, two independently tracked destination intents, exact readback, and fresh-directory recovery. Reuse the existing publication guards. Do not begin by rewriting the historical archive or building the full orchestration system.

Accept that slice only when these proposed checks pass:

- The same shareable bundle is reconstructible from either verified copy, including its required state and dependencies.
- Making either destination unavailable does not prevent custody through the other; one-copy and two-copy outcomes are accurate.
- A lost response after successful publication is reconciled without blindly republishing or duplicating the job.
- A missing artifact, changed submitted byte, unsafe path, or unexpected repository change blocks the affected operation.
- Duplicate delivery and a stale attempt cannot overwrite current accepted work.
- A replacement can recover into a different directory and identify the next action without the original worker's memory.
- Preservation labels do not invent technical acceptance, test results, application execution, or deployment success.

Use small synthetic fixtures for interrupted operations before claiming crash resilience. The full archive bootstrap, broader fault matrix, and application replay are subsequent work with their own acceptance boundaries.

## Use the temporary capacity well

Treat six workers as a rotating pool, initially three producers, one candidate assembler, one independent reviewer, and one preservation/evidence role. Move capacity to the bottleneck rather than maintaining fixed specialties.

Front-load the single-worker recovery path, then complete bounded product work. During the expected two-week window, demonstrate operation with two workers and one. With two, alternate author and reviewer. With one, preserve honestly labeled work; independent acceptance waits unless a genuinely independent bounded review is available.

The planning window is a forecast, not a guaranteed expiry. Finish with complete checkpoints, clear unresolved choices, and resumable work rather than dependence on the team still being present tomorrow.
