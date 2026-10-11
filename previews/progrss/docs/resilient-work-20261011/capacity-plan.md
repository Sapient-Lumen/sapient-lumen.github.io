# ProgRSS two week capacity and migration plan

Design prepared 11 October 2026. This document proposes how to use six workers while making their output usable after capacity falls to one or two. It does not create a schedule, start additional tasks, change credentials, operate the application GUI or authorize publication.

Enqueue useful fragments immediately and flush small ready batches promptly. The ten-minute first-copy target is an initial age alert and engineering objective, not an intentional delay or a service guarantee.

## Decision

Use the extra capacity to finish bounded ProgRSS work and remove dependence on live workers early. Establish a recoverable single-worker baseline first, then run up to three builders behind a shared candidate assembly, review and preservation pipeline. Workers settle ordinary technical questions and deliver one combined candidate to the root. The root handles user intent, exceptions, final canonical integration and authorized publication.

Treat two weeks as the user's planning forecast, not a verified platform expiration or a guarantee that six workers remain available. For planning, day 1 is 11 October and day 14 is 24 October 2026. Capacity can fall sooner; the recovery path must work from the first accepted baseline.

## What the incident establishes

The postmortem reports that the historical project directory is absent, while a shell launched from an existing directory works. The private checkpoint at commit `91ac023dc71ff90d525bc714570c588e8f387ca0` has verified preservation bytes. A fresh audit checked 38 archive parts, 3,547 logical files and all 181 r018 dependencies. Only the r018 source pair was materialized as inert data; the full working tree and application were not restored or application-tested.

Therefore, archive integrity is the starting point, not proof of a runnable baseline. Repeated local copies, a live conversation and worker memory do not remove the common risk of an ephemeral filesystem. Preservation is counted only when the authorized remote destination and its exact content have been verified.

## Six worker operating shape

| Capacity | Main job | Handoff |
| --- | --- | --- |
| Three builders | Complete up to three independent items from the existing ProgRSS queue | Small candidate, exact base identity, owned paths, relevant tests and remaining limitations |
| One candidate integrator | Resolve dependencies, assemble compatible changes in owned candidate paths and run combined checks | One candidate with the constituent task and artifact IDs |
| One reviewer | Independently challenge claims, reproduce important checks and adjudicate ordinary disagreements with the integrator | Accept, reject or a narrow unresolved question with evidence |
| One preservation custodian | Package useful work, maintain the recovery manifest and prepare publication packets | Immutable shareable checkpoint candidate and per-destination verified status |

These are dynamically assigned responsibilities in a rotating pool, not permanent worker identities or new task requests. The table shows a useful loaded-pipeline shape, not six jobs that must always run. Activate candidate assembly and review as candidates arrive; reserve prompt preservation attention whenever useful work is exposed. When there are fewer than three independent useful items, a builder helps review or recoverability, or is idle. Do not manufacture parallel work.

Each candidate records its owner, base revision and path ownership. One writer owns any shared candidate assembly path. Builders work in separate owned paths or workspaces supported by the existing session, then hand off immutable candidates. Worker integration touches only owned candidate paths; root alone performs final canonical integration and publication using the reviewed packet, without redoing routine technical adjudication. Concurrent workers must not edit the same canonical files. An integration conflict returns to the relevant owners; it is not resolved by silently overwriting another worker's work.

Rotate integrator, reviewer and custodian at a completed, preserved batch boundary, approximately daily or after several accepted batches when useful. Do not rotate just to satisfy a clock. The incoming owner reads the durable handoff and acknowledges its base before the prior owner leaves the role. A reviewer must not independently approve their own implementation. Rotation transfers responsibility; it does not invalidate existing provenance or require rerunning unchanged work.

## Keep the root out of the detail stream

Workers can decide implementation details, test selection within the contract, merge compatibility and whether an ordinary candidate meets acceptance criteria. The integrator and reviewer produce a joint resolution. When they disagree, ask one uninvolved available worker for a bounded evidence review, then choose the result supported by the contract. Preserve material dissent. If the contract cannot decide, reduce or defer the disputed change rather than send the root a long debate.

The root sees one publication packet containing:

1. Intended result and exact base and candidate identities.
2. Disclosure eligibility and destinations: concurrent public/private preservation of shareable ProgRSS work, with technical acceptance and product release reported separately.
3. Changed-path allowlist, dependency closure, byte hashes and the evidence needed to reproduce the claim.
4. Review result, combined-check result and failures, retries or untested surfaces stated separately.
5. One suggested action, any approval-sensitive change, and the next recoverable stopping point.

The full detail remains available behind the packet. Routine worker progress does not require root acknowledgment. Notify the root immediately for a required permission, disputed user intent, potential sensitive-data exposure, a broken preservation route, an unrecoverable conflict, a materially false claim or a loss-budget breach. Root remains the sole publisher. Worker consensus never replaces publication guards or mandatory confirmation.

## Work and queue limits

- Start with at most three implementation items, one integration candidate, and two unpublished preservation candidates, counting an in-flight candidate. An item waiting for review remains work in progress.
- At most two expensive test or reconstruction jobs run concurrently until actual resource use supports more. Lightweight independent checks can proceed without filling artificial quotas.
- Size a normal item so its next useful result and evidence can be handed off within roughly one focused hour. Split at a coherent dependency boundary if it cannot. This is a planning target, not a reason to interrupt a necessary atomic test.
- When integration or preservation backs up, finish and drain existing work before admitting another implementation item. Borrow builder capacity for the bottleneck; do not expand the queue.
- A test already running may finish under its existing deadline. Capture an interim resumable state before a long run when possible. A long-running test is not justification to leave all other valuable edits unpreserved.
- Every item names an acceptance criterion, evidence needed, owner and stopping condition. Work without those fields stays unstarted.

## Checkpoint settings and loss targets

Use a small configurable policy, not hard-coded promises. Initial values below are hypotheses to tune after the first few real batches. They are targets and admission guards, not service-level guarantees.

| Setting | Initial proposal | Meaning and tradeoff |
| --- | --- | --- |
| Local handoff interval | At each coherent result; normally within 5 minutes of new valuable work | Makes packaging and role transfer cheap; it does not survive loss of the shared filesystem |
| Remote preservation target | Within 10 minutes of new valuable work, plus before risky transitions | Limits ordinary exposure when root publication and the remote destination are available; smaller intervals increase fixed publication overhead |
| Dirty-work age guard | 30 minutes since the oldest unpreserved valuable change | Stop starting or extending loss-bearing work; prioritize preservation or declare the exact blocker |
| Delta packaging target | About 5 MiB or 50 changed payload files, whichever comes first | A batching hint, not a transport limit or permission to omit dependencies; tune from measured verification cost |
| Prepared custody packet cap | 2 globally, including one in flight | Each packet can target both repositories; bounds root demand and local-only value |
| Independent restore check | At initial baseline, after a recovery-format change, and at the final handoff | Validates the chain rather than repeatedly re-verifying unchanged archives without a reason |

Close a batch on the first useful trigger: a coherent result, time or size threshold, role/capacity transition, risky operation, or session interruption notice. Do not create empty checkpoints merely because time passed. A required dependency or single large source file may exceed the packaging target; include it intact or use the already reviewed preservation format, declare the exception, and verify the full reconstruction. There is no newly assumed upload-size limit.

Track exposure as both wall-clock age and accumulated worker-minutes of valuable work not covered by an acknowledged checkpoint. At six simultaneously productive workers, a 10-minute interval represents roughly 60 worker-minutes at risk; a 30-minute dirty-age guard can still represent roughly 180. These are planning illustrations, not measured loss or upper bounds on every failure. Lost local bookkeeping, unavailable publishing, source dependencies and abrupt failure can defeat a nominal target. Record actual coverage and breaches rather than reporting the target as achieved.

When the age guard or queue cap is reached, workers may continue bounded read-only research, review already preserved material or diagnose the preservation failure. They do not generate an ever-growing queue of local-only deliverables. If neither eligible destination can establish custody, say what remains exposed and stop dependent production. If one copy is verified, preserve that success and track a bounded backlog for the second copy. Retry only where the observed failure and authorization justify it; avoid unchanged polling or duplicate uploads.

The user has authorized concurrent public/private preservation of ordinary ProgRSS work and their project discussion. Prepare the eligible shareable bundle for `Sapient-Lumen/lumen-workshop` under `projects/progrss` and the reviewed ProgRSS checkpoint subtree of `Sapient-Lumen/sapient-lumen.github.io`. Neither waits for the other. One complete verified copy establishes custody; both establish replication. Public checkpoints may be provisional or unreviewed technically, but must pass disclosure and publication guards. Credentials, protected instructions, and third-party confidential material remain excluded. Product release acceptance remains separate. A narrowed projection is not a full replica; the public recovery closure cannot depend on private-only objects.

The root verifies the immutable remote commit and relevant file bytes/hashes before recording acknowledgment. A local commit, successful request submission or guessed branch head is insufficient. The receipt records the exact recovery base and dependencies. It need not recursively embed its own final hash; the next checkpoint or an independently authorized durable receipt can preserve the verification record without an infinite receipt chain.

Only one authorized publisher operates at a time; it may progress the two independent repository operations concurrently while serializing each target ref. After loss of root state, read the durable state, reconcile any in-flight remote effects by exact identity, and establish publication ownership before any new write. If the prior publisher might still be active and ownership cannot be established safely, freeze publication and report the ambiguity. A private-repository epoch is coordination data; it does not by itself fence writes to the separate public repository. Ownership and retry checks must cover each actual destination.

## Incremental implementation sequence

### First authorized implementation session

1. Preserve the incident evidence and identify the exact immutable recovery inputs. Record a known-existing-directory shell control and current workspace identity. Keep uploaded originals unchanged.
2. Reconstruct a full working copy into a fresh destination in the existing authorized writable session. Verify manifests, modes and dependency closure before executing project material. Do not infer runnable recovery from the already verified inert r018 pair.
3. Establish the smallest meaningful application check supported by the recovered project contract. Label byte recovery, environment readiness, test execution and application acceptance separately. No GUI acceptance is implied.
4. Have a different worker use only the proposed handoff to reconstruct into another fresh directory. The existing copy stays intact. Correct any missing prerequisites while six workers are available.

Exit gate: a known baseline, a self-contained recovery entry point and a verified authorized remote checkpoint. If recovery is blocked, use available workers on specific missing evidence or dependencies; do not launch three unrelated build streams.

### Days 1 and 2

Implement only the thin operating path: a machine-readable current state, per-item outcome/evidence records, and a checkpoint/publication packet generated from existing manifests. Reuse the established archive and reconstruction format before inventing another. Run one complete builder-to-integrator-to-reviewer-to-custodian-to-root cycle on an already useful small change or recovery correction.

Measure elapsed preservation lag, packet size, root review time, reconstruction time and avoidable retries for the first few batches. Tune thresholds once there is evidence. If bookkeeping costs more than it saves, simplify the packet and batch fewer items. Do not build a generic orchestration platform, dashboard or scheduler for this two-week effort.

Exit gate: a non-author can resume the item and explain its exact state without asking its original worker. The root can decide publication from one compact packet and inspect deeper only when necessary.

### Days 3 through 10

Use the pipeline for the highest-value existing ProgRSS queue items, limited by preservation and integration throughput. Favor completed, tested increments over a larger simultaneous backlog. Keep source, test contracts, relevant evidence and current decisions recoverable together. An abandoned experiment records its negative result and exit reason without becoming a permanent active task.

At meaningful dependency or recovery-format changes, repeat the restore check. Otherwise, verify changed content and the referenced immutable baseline instead of rebuilding every unchanged historical artifact at each checkpoint. A batch that changes the combined application runs the required combined checks; individual passing checks do not establish an integrated pass.

Exit gate for each batch: outcome accepted or explicitly rejected, useful evidence preserved, and queue state reconciled. Public promotion is separately gated.

### Days 11 and 12

Exercise a two-worker operating cycle and a single-worker recovery/resumption cycle using fresh directories and preserved handoffs. This is a capacity drill, not a real executor reset or deletion. Other workers can review the drill or finish already-owned work, but must not secretly supply missing knowledge to make the drill pass. Record where a human or root decision is genuinely needed.

Prune stale queue entries, consolidate duplicate evidence references and close incomplete handoffs. Preserve originals and required evidence; do not delete recoverable or permanent data without the applicable authorization.

Exit gate: one worker can identify the next item, reconstruct its base, run the stated checks, produce a checkpoint candidate and report a blocker accurately without the original team.

### Days 13 and 14 or any earlier capacity reduction

Stop admitting changes whose dependencies cannot be closed with current capacity. Drain integration and preservation queues, publish authorized checkpoints, and retain the last reviewed public release if new release review is incomplete. Produce one final state record: verified remote base, open items, exact blockers, next action per item and known untested surfaces.

The plan concludes when that handoff is verified and all begun work is either complete, preserved at a resumable stopping point or explicitly blocked. Record any incomplete second copy separately from complete custody. Two repositories on the same GitHub account do not protect against every provider/account failure; a milestone export through the existing authorized Library route is an optional additional recovery path, not a new infrastructure dependency. Continuing six-worker capacity is optional after that. No new capacity, expiry or automation is assumed.

## Graceful operation with fewer workers

**Two workers plus root:** one builder/integrator and one reviewer/custodian. Swap roles between completed items. Allow one implementation item and one unpublished candidate. Keep independent review for substantive changes and use the same packet format. Root remains publisher.

**One worker plus root:** perform intake, build, checks, packaging and resumption sequentially, with one item in progress. Mark self-review honestly. Preserve shareable unfinished or self-reviewed work at either or both eligible destinations with honest status; retain the last independently reviewed public release when the next release requires independent review. Do not fabricate a second reviewer or silently weaken a contract. Root may address a specific review exception, but the architecture must not require it to reconstruct every technical detail.

**No surviving workers or missing local state:** use the durable state record and any complete verified eligible checkpoint to resume through whatever already authorized capacity is available. Agent IDs, shell process IDs and the old cwd are evidence, not prerequisites. If no executable environment or required permission is available, report the concrete blocker and leave a recoverable state rather than claim continuation.

## Stopping rules

- Stop an item when its acceptance criterion is met. Do not add speculative features, polish or extra test permutations without a concrete remaining risk.
- Stop repeating a diagnostic after an unchanged result unless the next observation can discriminate between hypotheses. In particular, compare a failed project-cwd launch with an existing-directory control before treating the runner as unavailable.
- Stop a disputed change at the smallest safe boundary when its premise requires a user decision or broader authority. Continue independent items only within the queue and exposure limits.
- Stop new production when preservation cannot keep up. More workers are not progress if their output is about to disappear.
- Stop the two-week migration effort once the one-worker recovery path, durable handoff and current work closure are demonstrated. Do not continue infrastructure work to keep the workers occupied.

## Evidence basis and verification limits

This is a proposed operating design, not a deployed system or proof that its targets can be met. The factual recovery starting point comes from the ProgRSS working-session postmortem, evidence cutoff 11 October 2026 at 14:14 UTC, especially sections 8, 9, 12 and 13. Its immutable private checkpoint is https://github.com/Sapient-Lumen/lumen-workshop/commit/91ac023dc71ff90d525bc714570c588e8f387ca0. The recorded public checkpoint is https://github.com/Sapient-Lumen/sapient-lumen.github.io/commit/2047c007171fee22e79a345cb990e6d8c2c1da22. Those links reproduce the inspected report's verified identities; this design pass did not refresh repository state or perform publication.
