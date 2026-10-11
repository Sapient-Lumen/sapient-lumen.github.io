# ProgRSS: six-worker result flow

Design proposal, 11 October 2026. This document defines a workflow; it does not launch tasks, execute ProgRSS, change credentials, or publish anything. Root remains the sole final integrator and publisher. Six worker slots are the expected short-term capacity, not a permanent architectural dependency.

Enqueue useful fragments immediately and flush small ready batches promptly. The ten-minute first-copy target is an initial age alert and engineering objective, not an intentional delay or a service guarantee.

## 1. The operating principle

Trust workers to own the substance. Make their results independently understandable and recoverable, then send ordinary review, repair, and combination work back through the worker pool. Root should decide scope, priorities, exceptional trade-offs, and whether an exact candidate may be integrated and published. Root should not become the person who re-derives every result.

Separate three assertions:

1. **Preserved:** these exact bytes and required recovery dependencies are recoverable from at least one verified eligible destination; two complete copies are **replicated**.
2. **Accepted:** a specified independent review found these bytes meet their bounded contract.
3. **Published:** the exact intended artifact reached a verified destination. A public provisional checkpoint is allowed before technical acceptance; an accepted product release is a separate claim.

Preserve promising work before deciding whether it is correct. Never present preservation as acceptance, acceptance as an application test, or a repository push as a deployed-site check. A result can be useful and preserved publicly and privately while remaining disputed, provisional, or untested. The user has authorized public/private preservation of ordinary ProgRSS work and their project discussion. Credentials, protected instructions, and third-party confidential material still require exclusion or their applicable handling. Both eligible destinations may progress concurrently; neither must wait for the other.

The postmortem makes this separation concrete: conversation continuity survived while the historical project pathname was absent; an existing-directory launch succeeded while a launch in that missing pathname failed; preserved source bytes were reconstructed without application execution. The new flow must survive the same boundaries rather than assume them away. See the local postmortem, sections 1, 8–9, and 12–14.

## 2. A pool with rotating responsibilities

There are six reusable worker slots, W1–W6, plus root. Slots are a capacity description, not a request to create more agents or persistent tasks. A worker may choose methods, split a result into smaller units, propose follow-on jobs, and communicate with another assigned worker inside its existing authorized scope. Starting additional agents or tasks remains a separate authorization question.

A useful starting allocation is:

- Three workers advancing bounded investigations, designs, or implementation packages.
- One worker doing independent review and author–reviewer repairs.
- One worker doing preservation preparation and evidence checking.
- One worker acting as the current intake steward and candidate assembler.

These are temporary responsibilities, not a hierarchy or six permanent specialties. Rotate stewardship and assembly after a checkpoint or coherent batch. An author may review another worker's unrelated package while waiting for its own review. Expertise should influence routing; rotation should prevent dependence on one worker's memory, not force unnecessary handoffs halfway through a difficult job.

The steward maintains the next eligible work list, removes duplicate deliveries, routes reviews, and gathers the small packet for root. The assembler prepares a candidate in an isolated namespace and checks interactions. Neither can declare a final integration, change canonical publication state, or publish remotely. One worker can hold both responsibilities when traffic is light. A package that worker substantially authored or modified still needs a different reviewer.

Within already authorized goals, only the current steward routes or replaces job claims and prepares the assignment records; root persists them through ordinary custody packets without individually re-deciding each routing choice. A handoff records the next steward and a new routing epoch; late messages from the old epoch remain evidence, not current assignments. Workers own separate job/attempt output namespaces, including when they share one filesystem. They do not concurrently edit a shared live project tree or another worker's immutable package. Root serializes canonical updates against the expected remote head; a head mismatch is a reconciliation job, never permission to overwrite intervening work.

Capacity is elastic:

- At six slots, use the starting allocation and shift a producer to review whenever that is the bottleneck.
- At three or four, combine stewardship with assembly and keep one non-author review lane available.
- At two, alternate author and reviewer; root still handles publication.
- At one, preserve work and assemble candidates, but label independent review unavailable. Root can perform a bounded review if genuinely independent and able to do it; otherwise acceptance waits. Do not manufacture a second sign-off.

Use 11–24 October 2026 as a planning window only, with capacity reduction possible sooner. Nothing here creates an automation or assumes workers, sessions, or computers stay alive until then.

## 3. The smallest useful work unit

A job should deliver one independently assessable conclusion or change, usually small enough to reach a meaningful checkpoint in roughly 15–45 minutes of active work. This is a sizing guide, not a timeout on valid research. Larger jobs produce durable partial results and split when their acceptance questions become separable.

One short job card contains:

- Stable job ID and parent goal; one-sentence question or outcome.
- Pinned inputs: immutable commit/blob or artifact hashes, contract version, and prerequisite result IDs.
- Deliverable and acceptance checks; important exclusions and allowed work boundary.
- Current owner, attempt/generation ID, touched interfaces or paths, and known dependencies.
- Next useful result, blocking condition, and where a replacement should continue.

The worker owns how to solve it. Root does not prescribe an exhaustive micro-plan. A worker can create proposed child job cards under its parent goal, but cannot broaden permissions or consume new agents merely by naming them. The steward routes those cards to already authorized available workers.

Useful units include a testable fixture family, one adapter behavior, a bounded recovery audit, or a design decision with alternatives and discriminating evidence. “Understand all ProgRSS” is not a useful unit.

## 4. Result packages: compact envelopes, unrestricted substance

Each result has a small envelope and referenced payload. Hundreds or thousands of cases, findings, traces, or examples may live in the payload. Do not force workers to discard useful volume or paste it all into root's conversation.

Required envelope fields:

- `job_id`, `attempt_id`, `result_id`, `schema_version`, author identity, and creation time.
- Pinned input/base identities and dependency result versions.
- One-paragraph conclusion; changed paths/interfaces; affected acceptance criteria.
- Manifest of payload paths, sizes, and SHA-256 digests; a digest of the canonical envelope/manifest.
- Evidence inventory with explicit `passed`, `failed`, `not_run`, or `blocked` outcomes, exact tested artifact identity, and replay instructions where applicable.
- Known limitations and proposed next disposition: review, useful partial, blocked, superseded, or no-change.
- Public/private classification and any unresolved disclosure concern.

Prefer self-contained plain text and ordinary source files. Large archives are allowed when necessary, with a simple materialization contract, safe relative names, and exact hashes. Do not recreate a long chain of opaque references or splice transformations merely to avoid storing a small ready-to-read result.

The immutable result identity binds the job, attempt, pinned inputs, manifest, and bytes. Keep timestamps or transport metadata outside a content fingerprint used for duplicate-content detection. A delivery/event ID is different from a result ID: one result can arrive more than once without becoming two results.

Workers may attach long analysis relevant to the deliverable, but the package must not rely on hidden reasoning, private assistant memory, or unrecorded chat for its claims. Preserve observable facts, rationale, decisions, and reproducible evidence.

## 5. Intake and review routing

The steward handles new packages without waiting for root to read them:

1. Check the envelope is complete, referenced bytes exist, hashes agree, and identifiers/dependencies are coherent. Missing information returns to the author with the specific omission; an accepted payload is never silently repaired in place.
2. Record the result for the next preservation packet immediately. A structurally valid, disclosure-eligible result can be saved publicly and privately before substantive review, with provisional status explicit. Uncertain disclosure eligibility blocks only the affected public content; it does not block an otherwise eligible destination.
3. Route to an available non-author reviewer, preferably one familiar with the relevant contract but not co-author of the change. Avoid permanent reciprocal pairs. Where several workers qualify, choose the least-loaded rather than awaiting a particular worker.
4. The reviewer checks the acceptance contract, relevant boundary cases, and whether evidence supports the conclusion. Reuse trustworthy evidence with exact identity; independently inspect the critical assertion. Full repetition of every low-risk check is unnecessary.
5. Return one of: `accept`, `needs_change`, `needs_evidence`, `blocked_dependency`, or `disputed`, with exact result identity and brief reasons. A small correction goes directly back to the author, not through root.

The review is a compact record, not another essay. One independent review is the normal requirement for a bounded, low-risk package. Add a second non-author check for cross-cutting invariant changes, recovery/destructive-operation semantics, privacy/publication boundaries, materially conflicting evidence, or a candidate that combines interacting changes. The second check should cover a named risk; it must not be ceremonial.

For high-volume results, review the generation/collection method, manifest completeness, representative cases, and every case that determines acceptance or a high-consequence claim. Record precisely what was sampled. Sampling cannot justify a claim that every case passed.

## 6. Disagreement, dependencies, and combinations

### Resolve substantive disagreement where the knowledge is

The reviewer first states the smallest contested proposition and the evidence that would distinguish the alternatives. The author can clarify, amend, or disagree. After one focused exchange without convergence, send the exact disputed slice to a third qualified worker as adjudicator. Give that worker both positions and the pinned artifacts, not a compressed root interpretation.

Adjudication is based on contract and evidence, not majority vote. The adjudicator can choose one interpretation, propose a discriminating check, or preserve two alternatives. A root exception is needed only if the unresolved question changes user intent, project policy, a cross-project invariant, permissions, or release risk; or if two focused repair/adjudication passes cannot resolve a consequential disagreement. Root receives a short decision card with options and consequences. Other compatible work continues.

### Dependencies are exact and small

A result depends on specific versions, never “whatever is latest.” The steward keeps a reconstructible directed graph of job/result dependencies. A blocked job records the missing input and can continue independent portions. Detect cycles when accepting dependency declarations; route the smallest cycle to its owners to split an interface contract or create one joint bounded job.

Acceptance of A does not automatically accept B, which uses A. When an input changes, only the affected dependency descendants become `stale_pending_impact_review`. A worker can establish that an unaffected descendant remains valid using a short compatibility record; otherwise it revises or reruns the relevant checks. Preserve the old evidence and state exactly what changed.

### Combine off the canonical line

The assembler groups compatible accepted results by common base, goal, and interface. A candidate contains exact result IDs, a deterministic application order where needed, resulting file digests/tree identity, integration evidence, and excluded or unresolved items. It is a proposal, not the canonical integration.

- Disjoint paths can still interact through shared contracts; check declared interfaces and affected invariants.
- Same-path textual collisions return to the authors or a combination worker. Do not let root hand-edit a conflict during publication.
- When combination changes semantics or reviewed bytes, the combined result is new and receives an independent check of the changed interactions. Existing unchanged package reviews remain useful evidence.
- A stale base is an explicit rebase/combination job. Unrelated new commits need not invalidate all evidence, but applicability must be shown against the new candidate identity.
- A small ready candidate may proceed while a disputed neighboring package stays pending. Do not make one unresolved result hold the whole checkpoint hostage.

## 7. Supersession and duplicate deliveries

Results and reviews are append-only in meaning. Corrections create a new result that names the exact result(s) it supersedes and explains the changed claim or bytes. Never silently edit a reviewed package or erase a failed run.

The current disposition is a derived view of records. Minimal v1 tracks open/active/closed jobs, preserved-copy count, technical acceptance, and per-destination publication outcome. Extended review substates can be added when useful, but are not prerequisites to the first checkpoint. A disputed package can already be durable and publicly preserved with truthful labeling.

Duplicate handling is idempotent:

- Same result ID and same digest: acknowledge once; record another transport receipt only if useful; do not repeat review, integration, or publication.
- Same job and identical content/input fingerprint under a different delivery ID: attach the receipt as an alias to the existing result.
- Same result ID with different bytes: quarantine both deliveries pending identity repair. Never choose last-write-wins or imply wrongdoing without evidence.
- Similar-looking conclusions with different inputs or evidence: keep both until a worker confirms semantic duplication. They may be independent corroboration or genuinely different findings.
- A late result from an old attempt is preserved and marked late. It cannot replace the active owner or accepted successor automatically, but its useful work may be adopted through a new result.

Before any retry after an uncertain remote operation, root checks the intended immutable identity and destination. A timeout is not evidence that the operation failed.

## 8. Backpressure without suppressing useful output

Limit simultaneous unfinished obligations, not the number of findings. Starting settings for six workers:

- A worker owns one primary production job and, if useful, one small unrelated review. It can finish a package while another is being preserved.
- Keep at most two completed, unreviewed substantive packages per producing worker before assigning that worker to review, consolidation, or missing evidence.
- If the ready-for-review queue exceeds roughly six packages, or the oldest ready item has waited longer than a typical work unit, shift capacity toward review. If candidate assembly is the oldest queue, shift toward assembly instead.
- Keep at most two globally prepared custody packets, including one in flight. A packet may target both repositories. At the cap, coalesce compatible results and pause new loss-bearing branches. Already-preserved results waiting for review are a separate queue; a delayed product release must not block custody.

These are visible, adjustable starting thresholds rather than service guarantees. Tune them using oldest queue age, accepted-result throughput, root decision time, and uncheckpointed work age. A giant result needs a manifest and chunked payload transport; it does not need a giant conversation message.

Expose a checkpointable partial as soon as useful, rather than waiting for agreement. Proposed v1 aims for the first verified eligible copy within 10 minutes of new valuable work, with a 30-minute oldest-unpreserved admission guard. These are measured targets, not guarantees. The steward offers preservation packets at coherent boundaries. If neither eligible destination can establish custody by the guard or queue cap, stop adding expensive local-only production; diagnose the blocker and review already-preserved material. If one copy is verified, its work is preserved while the second destination remains pending. Bound the replication backlog too; do not hide it or let it starve first-copy custody.

No timer or scheduling service is enabled by this design. These are conditions evaluated during the authorized active work loop. If the session is not running, nobody should claim a checkpoint or review will occur automatically.

## 9. Stalled work and lost workers

Each claimed job records a claim generation, active worker instance, last substantive progress event, expected next artifact, and an expiry appropriate to the job. A useful initial expectation is a partial result or obstacle update within one work unit, adjusted when a known long operation is still running.

On silence or missed progress:

1. Check the existing worker/task state and any known operation/result ID. Ask for the smallest progress update; do not launch duplicate expensive work blindly.
2. Distinguish a real running operation from an absent worker, blocked input, missing cwd, and failed transport. Repeated identical probes are not diagnosis.
3. When reassignment is justified, issue a new claim generation, preserve the old claim/result history, and give a replacement the durable job card plus latest usable partial. Old-generation results cannot mutate current state.
4. If independent workers fail at the same boundary, open one shared infrastructure/contract blocker rather than six redundant retries.

No assumption of a reliable wall-clock lease can override actual evidence that an external operation is still running. For operations with external effects, establish the existing operation's outcome or idempotency before repeating it. All workers remain unable to publish remotely, reducing split-brain publication risk.

After a launch failure, use a known-existing-directory control and check the requested directory from its parent, within granted access. Record command, cwd, stage, and exact error separately. Do not turn a missing path into a global runner-outage claim. An access denial is a stop at that boundary, not permission to try a different access route.

## 10. Root's narrow interface and exceptions

Ordinary root input is one short publication packet:

1. Outcome, affected goal, exact candidate digest, and pinned base.
2. Included result IDs and independent review records; explicit evidence coverage and unrun checks.
3. Manifest of the exact private/public changes, preservation/publication classification, and rollback reference.
4. The command/API-independent verification plan and the next worker-owned action.
5. Exceptions requiring a decision, if any.

Root verifies identities, scope, authorization, candidate applicability, disclosure boundary, and the integrity of the review/evidence chain. Root then makes the final integration/publication decision and performs the authorized publication. It need not reread all traces or re-derive each worker's technical reasoning. An inconsistency is routed back to the relevant worker for a focused answer.

Escalate immediately for:

- Missing or conflicting authority, access denials, credentials, privacy uncertainty, or a proposed high-impact communication.
- Artifact identity mismatch, suspected corruption, uncertain publication outcome, or an attempted canonical-state collision.
- A choice that materially changes the user's desired behavior, scope, compatibility contract, or accepted release risk.

Escalate after worker resolution attempts for:

- Two focused technical repair/adjudication passes without resolution of a consequential disagreement.
- A dependency cycle or shared blocked boundary the affected workers cannot break within the current goal.
- Durability backlog beyond the stated risk budget, or publication queue growth that capacity shifts cannot drain.

Routine test failure, small revision requests, semantic duplicates, and answerable dependency questions go to workers first. Root does not become a seventh first-line reviewer by default.

## 11. Concurrent public and private checkpoint lanes

The intended destinations are `Sapient-Lumen/lumen-workshop` under `projects/progrss` and the reviewed ProgRSS subtree of `Sapient-Lumen/sapient-lumen.github.io`. Root verifies each destination, current remote state, and applicable authority. One publisher can progress both independent operations concurrently, preserving all existing tree, parent, byte, disclosure, and approval guards. Serialize mutations to each target ref.

Ordinary ProgRSS source, task cards, results, evidence, and the user's project discussion may be prepared as one shareable bundle for both destinations. Unreviewed or failed work is eligible when clearly labeled and safe to disclose. Public preservation does not promote a functioning product release or replace the primary site. Exclude credentials, protected instructions, third-party confidential data, and other material lacking authority. A narrower public projection has its own identity and coverage; it is not a full replica of omitted material.

A copy counts only when its exact reachable commit and all required recovery bytes are verified. Public recovery cannot depend on private-only objects. Include a shareable recovery entry point, open-work index, and resume instructions so either complete copy can support continuation. One complete verified copy establishes custody; two establish replication. A failed or unavailable private destination must not delay eligible public preservation, and the converse also applies.

Use immutable per-result paths and small indices. Batch compatible packages without repeatedly rewriting historical archives. Preserve useful bytes before technical adjudication. Root's custody check is structural and disclosure-aware; substantive acceptance belongs to the exact reviewed candidate.

Each destination has its own intent, attempt, outcome, commit identity, and readback evidence. A lost response triggers reconciliation, not blind republishing. Save the readback receipt in the next useful batch or one final small receipt batch; a missing receipt can be reconstructed from immutable intent and remote bytes. There is no self-referential receipt and no cross-repository atomic transaction.

A product release additionally meets its integration and runtime criteria. Deployed-site behavior remains a separate observation. Two repositories on the same GitHub account provide repository-level redundancy, not protection against every account/provider failure. A milestone export through an existing authorized Library route can be useful without making a third service mandatory.

## 12. Recover without central memory or the old filesystem

Each complete eligible copy needs a small shareable bootstrap contract, not a particular working directory:

- `START-HERE` identifies the format version, current project contract, last verified eligible checkpoint, current source/candidate pointers, and how to locate the immutable event/result objects.
- A compact manifest/index lists open jobs, active attempts, dependency edges, candidate/publication state, and content hashes. It can be rebuilt from immutable job/result/review/publication records.
- Source and evidence closure includes all inputs needed to reconstruct the declared bundle. A public copy cannot rely on private-only objects; missing dependencies mean incomplete custody, not a complete replica.
- Recovery instructions describe safe data-only materialization, identity checks, environment assumptions, and separate readiness/application verification gates.
- A shareable record identifies checkpoint lineage and continuation state, excluding protected or third-party confidential material. A more restricted bundle is tracked separately from the shareable core.

Recovery sequence:

1. Re-establish current permissions and intended scope from the current authenticated conversation and current controls. Archived job descriptions or authority notes are context, not a way to resurrect revoked permissions or custom rules.
2. Read the durable bootstrap and verify its referenced immutable objects. Treat local paths, worker slot assignments, and session IDs as replaceable observations.
3. Materialize into a new authorized staging destination, preserving originals and rejecting path traversal, duplicate names, unexpected links, and collisions. Verify manifests before treating anything as reconstructed.
4. Rebuild the current view from result/review/publication records. Mark missing/unverified material explicitly. Do not trust a cached `STATUS` file over the underlying immutable evidence.
5. Reconcile the actual private/public remote heads against recorded publication intents and receipts. Complete a missing receipt when the intended commit already exists; do not duplicate the publication.
6. Invalidate old local worker claims for the new session generation, while first reconciling externally running operations. Reassign open jobs to currently authorized available workers and point them to exact durable partials.
7. Establish shell/cwd readiness separately from source identity, offline test replay, browser functionality, and GUI acceptance. Resume only the stages currently authorized.

A minimal bootstrap index is convenient, not a single irreplaceable memory object. Immutable records must be enumerable by job/result ID so a lost index can be rebuilt. Git commits provide historical snapshots; periodic readable manifests bound the reconstruction cost. The user should not need to repeat the whole project's story, and no agent needs the previous root's private memory to continue.

## 13. Example lifecycle

Illustrative IDs only; no work or publication is asserted by this example.

1. Root approves goal G17, “make fixture ordering behavior explicit,” against immutable base B. W1 takes job G17/J1 with claim generation 1; W2 independently investigates a neighboring adapter behavior.
2. W1 produces R1 with 200 fixtures, a manifest, and a conclusion. The result says 198 passed, one failed, one was not run. The steward accepts its structure, routes it to W4, and adds it to a shareable public/private preservation packet. Root starts the eligible destination operations independently and verifies each copy without pretending all 200 cases passed.
3. W4 identifies one disputed ordering assumption and sends the exact case to W1. W1 supplies evidence but disagrees. W5 adjudicates that slice against the pinned contract, identifying a missing tie-break rule. Only if defining that rule changes user intent does root receive a decision card.
4. W1 creates R2, superseding R1. R1 and its failed trace remain available. W4 reviews R2 and records acceptance of its exact digest, with any execution limit still stated. The duplicate delivery of R2 is recognized by ID/digest and causes no second review.
5. The rotating assembler W6 combines R2 with W2's accepted R8 on base B. An interface interaction creates candidate C1. W3 checks the changed interaction; a failing check sends C1 back to W6. C2 includes the repair and exact evidence. Since W6 changed bytes, W6 cannot supply the independent acceptance of its own repair.
6. Root receives C2's compact packet, verifies identity, authorized scope, review coverage, and disclosure boundary, then performs final integration and the permitted publication. Exact remote readback yields independently tracked private receipt P12 and public receipt U7; either complete copy establishes custody, while both establish replication. Runtime or deployed GUI behavior is reported only if actually checked.
7. The local filesystem disappears afterward. A new authorized session reconstructs from either complete readable copy and its immutable records, reconciles the other destination, and resumes open job G17/J4 from its preserved partial. It neither reruns completed publication nor assumes the vanished path means the shell service is down.

## 14. Keep the process lighter than the work

The normal path needs only a short job card, a result envelope with payload, one independent review, and a candidate/publication packet that can contain multiple jobs. Most metadata should be generated from manifests and existing records, not repeatedly rewritten by workers.

Do not add a standing meeting, root sign-off for every internal routing decision, a mandatory second review for every tiny change, a fixed permanent worker role, or a requirement that root understand every raw finding. Add detail only where it makes a result reproducible, prevents duplicate action, exposes a real risk, or lets another worker continue after state loss.

Success is simple to observe: useful worker output keeps flowing; disagreements usually resolve among workers; root decisions stay small; exact work is preserved before it becomes expensive to lose; accepted public checkpoints remain truthful; and a replacement session can continue without reconstructing the project from conversation.
