# Independent review of the ProgRSS resilient work design

Prepared 11 October 2026. This is a design review, not an implementation report or a record of fault-injection tests. No application code was executed, repositories written, credentials created, or access changed in this review.

## Recommendation

Adopt a small shareable durable work ledger, immutable result bundles, dynamically assigned worker responsibilities, and one authorized final integrator and publisher at a time. Enqueue valuable fragments immediately and preserve eligible copies publicly and privately through independent concurrent operations, as the user has now authorized. Workers may review, adjudicate, and assemble integration candidates; the root owns acceptance of the final canonical candidate and the authorized remote writes.

A result becomes recoverable only after the exact remote checkpoint and every required artifact are verified. A worker's completion message, a local commit, a submitted push, and a branch name are each insufficient. Each destination is a separate operation with its own receipt. One complete verified eligible copy establishes custody; two establish replication. Technical acceptance and product release remain separate from public provisional preservation. If writer ownership is ambiguous, publication pauses while existing effects are reconciled.

Do not build a scheduler, consensus service, leader-election system, distributed database, new credential system, dashboard, or six independent publication paths for this effort. Six-worker capacity is useful for closing bounded work and demonstrating that the same state can be resumed with one or two workers. It is not a verified two-week service guarantee.

## What is established and what remains unproved

The incident report establishes that the historical project directory is absent while a generic shell launched from an existing directory works. It records byte verification of 3,547 logical files and all 181 r018 dependencies from the durable private checkpoint. Those facts do not establish why the directory vanished, whether the complete application environment has been reconstructed, or whether the application passes its contract.

This review uses that reported baseline. It does not repeat the byte audit or claim an application replay. A recovered manifest, a recovered working tree, an executable environment, executed tests, and accepted application behavior must remain separately reported states.

## Smallest viable operating contract

1. **One logical authoritative state with eligible replicas.** A versioned manifest and short task index identify the current accepted base, each task and attempt, result bundle, dependency closure, review disposition, and pending publication. Immutable history preserves superseded results without keeping them active forever. Each complete eligible copy includes its shareable work index and resume state, and the same logical bundle inventory digest. A local checkout is a replaceable cache.
2. **One authorized writer at a time.** Workers write only owned candidate locations. The root accepts canonical integration and performs authorized preservation and public publication. Recovery can replace a lost root process, but only after reading durable state, reconciling in-flight effects, and establishing writer ownership. No immortal process is assumed.
3. **Bounded result packets.** Each packet identifies task, attempt, ownership generation, exact base and dependencies, candidate digest, changed paths, required artifacts, acceptance evidence, classification, unresolved limitations, and proposed next action. Detailed evidence stays behind the packet rather than flooding the root's conversation.
4. **Different state for custody and acceptance.** A useful result can be durably preserved while still unreviewed, rejected, or incomplete. Conversely, a locally accepted result is still exposed until remote custody is verified. Technical acceptance does not replace disclosure eligibility or product-release requirements. The user has authorized ordinary ProgRSS work and their project discussion publicly, including honestly labeled provisional work.
5. **Fenced attempts and bound evidence.** An old attempt cannot complete a superseding attempt. Reviews and test results bind to the exact candidate and relevant environment identity; changed candidate bytes invalidate affected evidence. A disjoint file diff does not prove semantic independence from a changed base.
6. **Independent public and private custody.** Eligible destinations can progress concurrently; neither depends on the other succeeding. Each copy needs complete recovery closure. Public-only recovery cannot depend on private-only objects. A narrower sanitized projection gets its own identity and coverage, rather than being called a full replica. Public content uses a reviewed allowlist and public-specific history; product release remains separately accepted.
7. **Backpressure before more production.** Limit active implementation, integration candidates, and unacknowledged preservation packets. When custody or review falls behind, repurpose worker capacity to drain it and stop adding local-only value. Checkpoint age and volume targets are measurable objectives, not guarantees against abrupt failure.
8. **Recovery is a demonstrated procedure.** A new authorized worker can identify the durable base, materialize a fresh working copy, verify required artifacts, find unfinished tasks, reject stale completion claims, and resume without the original workers' memory. Application execution remains a separate permission and verification stage.

## Concrete failure scenarios and required outcomes

The following are acceptance obligations for later implementation. None is reported as tested by this design review.

| Scenario | Required outcome | Evidence needed to accept the design |
| --- | --- | --- |
| The shared filesystem disappears after a worker finishes but before preservation | The durable task remains incomplete or resumable. The lost local artifact is never represented as preserved. A replacement attempt starts from the last verified state. | Durable task identity, last acknowledged checkpoint, exposure report, and a fresh resume using that checkpoint alone |
| The filesystem disappears after remote acceptance but before the local receipt is recorded | Recovery reads the intended remote destination and identifies the exact artifact by immutable identity. It re-verifies custody instead of blindly producing another upload. | Exact remote commit and complete artifact hashes; reconciled local status |
| A worker vanishes during a long task | The task has a durable starting contract and last preserved useful intermediate. Another worker can resume or supersede it. The absent worker is not required for ownership transfer. | Task/attempt records, explicit supersession, and successful independent interpretation of the handoff |
| An old worker returns after a new attempt was assigned | Its output is retained as a candidate for possible reuse but cannot mark the current attempt complete or overwrite canonical files. | Attempt and ownership-generation mismatch detected; explicit disposition and current-task state unchanged |
| Two candidates touch different files but depend on incompatible assumptions | Path-level compatibility is insufficient. The combined candidate is reviewed against dependencies and the shared acceptance contract. | Bound base/dependency identities, combined checks, and recorded adjudication |
| A result summary says tests passed, but the logs concern an earlier tree | The pass is not credited to the current candidate. Only applicable evidence is reused; changed surfaces are checked again. | Candidate digest and environment identity bound to evidence; honest passed/failed/not-run fields |
| The checkpoint manifest exists but one archive part or required dependency is missing | No complete-custody acknowledgment. Preserve any useful partial state distinctly and block reconstruction-dependent work. | Enumeration and hash verification of the full required closure, including externally referenced objects |
| An archive contains a path escaping the new restore directory | Safe materialization rejects that entry before it can alter unrelated files. The original archive remains unchanged. | Proposed path and link validation plus a later isolated synthetic restore test |
| Either destination succeeds while the other fails | Complete verified custody remains valid at the surviving destination. Its recovery entry point, open state, and required bytes work independently. Retry only the unresolved operation after reconciliation. | Separate destination receipts, equal bundle coverage where replicated, and recovery using each destination alone |
| Public publication succeeds but the response is lost | Read back the intended public commit/tree before retrying. Matching bytes close the operation; a mismatch is investigated without an unconditional overwrite. | Exact public commit and content-hash readback bound to the promotion |
| The public branch changes unexpectedly between preparation and write | The candidate is not force-published over unknown work. Reconcile the new base and revalidate affected review evidence. | Expected-head conflict handling and a new accepted publication base |
| Old and recovered root processes may both still write | Freeze publication until writer ownership is resolved. A private epoch check alone is not claimed to fence an already in-flight write to another repository. | Ownership handoff or confirmed old-writer quiescence, destination reconciliation, and no unresolved in-flight mutation |
| A public bundle contains credentials, protected content, or third-party confidential material in a log, nested artifact, or metadata | Public upload of that content is blocked. User-authorized project discussion is not automatically private; inspect actual eligibility. Do not copy unreviewed repository ancestry. | Reviewed path allowlist, final-tree disclosure checks, public-specific ancestry, and explicit narrower coverage if content is excluded |
| Publication metadata points to a changed artifact after review | Do not publish it under the old approval. Materialize the immutable reviewed bytes or repeat affected review. | Digest equality at the final publication boundary |
| One remote is unavailable, or both are unavailable | Continue through the eligible available destination; if neither can establish custody, stop growing local-only work at the queue/age guard. A successful copy stays preserved while its second copy is pending. | First-copy exposure and replication backlog tracked separately; exact blockers and no access-denial bypass |
| Capacity falls from six workers to two or one tomorrow | Reduce concurrency and preserve the same contracts. Self-review is labeled; independent-release gates are not silently weakened. | A smaller-capacity recovery and one-item completion demonstration without help from original authors |
| A recovered task contains an old permission statement | Treat it as provenance, not fresh authority. Apply current user scope, confirmation requirements, and access restrictions before acting. | Current authorization check tied to the specific action and destination; no replay of obsolete custom rules |

## Highest risk findings

### Split brain needs an honest availability tradeoff

A logical lease or epoch stored in the private repository can invalidate stale worker results. It cannot, by itself, atomically reject an already in-flight mutation to a separate public repository. A check followed by a push has a gap, and the two repositories are not one transaction.

The viable initial policy is one authorized publisher at a time, immutable operation identities, expected-base updates where supported, and fail-closed recovery when writer ownership or an in-flight effect is ambiguous. This permits replacing a vanished process after ownership is established. It does not claim automatic highly available leader failover. Enforced destination-side fencing would be a later requirement only if overlapping automatic takeover becomes necessary.

### Custody must precede an acknowledgment

The acknowledgment must identify the exact durable commit and the complete required bytes, not merely say that a commit was created. A successful write response is evidence to investigate, not sufficient recovery proof. Reconcile an uncertain response by reading the immutable destination before retrying.

The checkpoint does not need to contain its own final commit hash. The manifest has its own stable content identity; a receipt can refer to the resulting commit, and recovery can regenerate the verification from that commit. Avoid an infinite chain in which a receipt needs another receipt to become meaningful.

### A single coordinator must not become a full second implementation team

Root-only publication is compatible with trusting workers to decide ordinary technical questions. A candidate assembler, an independent reviewer, and a bounded adjudicator can resolve details and return one combined packet. The root verifies scope, ownership, applicable guards, final candidate identity, cross-item consistency, and remote effects. It does not redo every research step or reread every progress message.

Preservation must have priority over optional polish and public release work. Otherwise, the root's one publication queue becomes the same loss point the design is intended to remove. Unreviewed but safe-to-share useful work can be checkpointed at both destinations with an honest status. Immediate enqueue and prompt small-batch flushing matter more than elaborate scheduling.

### Independence is evidence rather than voting

The author cannot be the candidate's independent reviewer. A reviewer needs the original artifacts and applicable acceptance contract, not only the author's summary. Ordinary disagreement can be settled by one uninvolved worker using that evidence. Material dissent remains recorded; a vote count does not make contradictory claims true.

Rotating responsibilities at a completed, preserved batch boundary distributes knowledge without erasing provenance. A fixed six-person staffing chart is unnecessary. The role separation applies per candidate even when available people and queue pressure change.

### Recovery claims must not outrun the evidence

A shell launched from an existing directory establishes that command execution works there. It does not explain the missing project directory. Verified archive bytes establish preservation integrity. They do not establish safe extraction, full materialization, environment readiness, executed tests, or application correctness. The future recovery drill must record those stages separately and stop at any missing authorization.

## Unnecessary complexity to reject

- Automatic leader election or leases presented as cross-repository transaction safety.
- Six independently writable canonical queues, branches, or publication endpoints.
- A separate database, message broker, service, dashboard, or scheduler solely to preserve this two-week effort.
- Full historical repackaging and dependency rehashing for every tiny change when unchanged immutable references are already verified; the complete closure must still remain reconstructible.
- Global coordination for every worker progress note or routine technical decision.
- Requiring technical release acceptance before safe provisional work can be preserved, or requiring one repository to succeed before the other is allowed to progress.
- Secret scanning used as a substitute for an explicit public allowlist and classification review.
- Hashes without durable byte locations, or byte locations that rely on transient local paths or expiring links as the only recovery source.
- Automatic re-execution of restored project code or old instructions merely because archive integrity passed.

## Ranked decisions still needed before implementation

1. **Verify the implementation of guarded writes and readback.** The current connector schema advertises an `expected_sha` mismatch check; use it with the pinned base, fresh ref read, and `force=false`. This advertised guard is distinct from native GitHub REST fast-forward semantics, and its atomic implementation has not been established by this design review. Retain complete-tree and exact-byte guards and prove the thin custody slice before claiming reliability.
2. **Define the ownership handoff proof.** Use the existing task/thread lifecycle and explicit coordinator handoff where reliable. Specify what evidence establishes that the old publisher cannot continue, and what happens when that evidence is unavailable. Uncertainty blocks publication; it must not be solved by an invented timeout guarantee.
3. **Fix the first acceptance contract and recovery boundary.** Name the first small ProgRSS item, its dependency closure, its required checks, and which execution approvals already exist. Preserve the current incident evidence independently of restoring a runnable application.
4. **Tune initial loss-exposure and queue targets.** Enqueue useful fragments immediately and flush small ready batches promptly. A proposed ten-minute preservation age alert/engineering target and thirty-minute dirty-age guard are starting hypotheses, not intentional waiting periods or an SLA. Measure real checkpoint cost, oldest unpreserved value, accumulated worker effort at risk, and root queue time, then tune the settings.
5. **Set the minimum public release evidence when capacity falls.** If independent review is mandatory, one surviving worker may preserve shareable provisional work at both repositories while the accepted product release remains at the last independently reviewed version. Any exception must be an explicit contract decision under current permissions.

## Reconciled decisions and remaining implementation choices

The design now has one coherent recommendation:

- A rotating pool with an initial three-producer, candidate-assembler, independent-reviewer, and preservation/evidence allocation; responsibility separation applies per candidate rather than permanently per person.
- Workers route work and resolve ordinary technical disputes inside the authorized goal. The coordinator owns final canonical integration and remote publication without individually re-deciding each routing choice.
- Eligible public/private operations progress independently. One complete verified copy is preserved, two equal-coverage copies are replicated, and narrower projections are explicitly different bundles.
- Remote exact-byte readback establishes custody. Receipt observations can be saved in the next useful/final batch and reconstructed after interruption; a mandatory second commit per acknowledgment is unnecessary.
- Immediate intake and prompt flushing, with a ten-minute initial age target, thirty-minute production guard, and two global prepared custody packets. These are tuning defaults, not availability promises.
- Five minimal record families. Full archive-bootstrap hardening and richer state machines follow the first useful direct-file preservation/recovery slice.

No unresolved architectural disagreement remains. Implementation still needs to demonstrate the guarded transport behavior, establish the practical ownership-handoff evidence, and select the first bounded ProgRSS acceptance contract. Those choices are explicit; no application replay or fault proof is claimed here.

Two repositories on the same GitHub account are repository-level redundancy, not account/provider-independent backup. An optional milestone bundle through the existing authorized Library/export route can add another recovery path without a new infrastructure dependency.
