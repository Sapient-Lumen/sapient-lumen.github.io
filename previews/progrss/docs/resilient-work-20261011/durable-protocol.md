# ProgRSS durable work protocol, version 0.1

Design proposal, 11 October 2026. No implementation, remote write, new credential, scheduled task, application execution, or peer operation is performed by this document.

Enqueue useful fragments immediately and flush small ready batches promptly. The ten-minute first-copy target is an initial age alert and engineering objective, not an intentional delay or a service guarantee.

## Recommendation

Treat the workspace as a disposable cache. Use the existing private and public repositories as independent destinations for the same shareable ProgRSS work journal and recovery bundles. The user's 11 October direction authorizes uploading such work to both at the same time; private success is not a prerequisite for public preservation. Preserve small, immutable worker bundles before substantive adjudication; integrate and release product changes only afterward. The coordinator is the sole committer to the canonical journal and the sole remote publisher. The current worker steward owns routine routing and claims inside the already-authorized goal; workers own implementation, assembly, and technical review within explicit contracts. The coordinator need not approve every assignment or personally repeat their reasoning or every test.

The protocol needs ordinary files, JSON, SHA-256, the existing GitHub access route, and a few small validation/packing scripts. It does not need a service, queue server, database, new authentication, or permanent six-worker availability.

Three statements must remain distinct:

1. **Preserved:** at least one complete eligible recoverable copy was committed remotely and read back by immutable reference. **Replicated** additionally means both intended repositories have verified complete copies.
2. **Accepted:** a specific result or combined candidate met a declared review contract.
3. **Published:** the intended approved files reached their intended destination, and the exact remote bytes were verified. Deployment or browser acceptance is a separate observation.

A broken or unreviewed experiment can be preserved publicly and privately when disclosure-safe, with its actual provisional status. Preservation does not make it an accepted application release. Anything ineligible for public disclosure remains excluded from the public bundle; two replicas of an eligible subset are not two complete replicas of a larger restricted bundle.

## 1. Baseline and limits of present evidence

The current incident establishes that the historical project working directory is absent from the current filesystem view, while commands work in a known-existing parent. The cause and timing of the disappearance remain unresolved. The protocol must survive loss or replacement of the local workspace without needing to assume a particular cause.

The starting durable baseline is `Sapient-Lumen/lumen-workshop`, commit `91ac023dc71ff90d525bc714570c588e8f387ca0`. The fresh recovery audit at `2026-10-11T14:14:00.461320+00:00` validated 38 archive parts, 3,547 logical files, and 181 r018 contract dependencies. It recovered the r018 source pair as inert data:

- `index.html`: 532,300 bytes; SHA-256 `f1b25affd69b74af6abb5a4f5d887ed0bab5b7cd22f007cab00f93e36bda80c6`
- `ProgRSS.html`: 6,219,264 bytes; SHA-256 `e805e6cffa4f4474a1b3384738e1cb07afe26619191c9de5a4957e0eba263c7c`

This is recovery/integrity evidence, not a fresh execution of the application, contract replay, or GUI acceptance. The r018 test account must retain its actual shape: 81/82 in the broad run, then 1/1 on the isolated retry, supporting 82 distinct latest passing outcomes. It must not become a fictional flawless 82/82 broad run.

Sources for this proposal's factual baseline are the existing local `recovery-audit/RECOVERY-AUDIT-RESULT.json`, `recovery-audit/FINAL-RESULT.json`, `evidence/recovered-private89-receipt.json`, and `report_text_for_qa.txt`. The receipt describes earlier publication readbacks; the recovery audit records a fresh check. These source paths identify this design's inputs, not the future recovery authority.

## 2. Storage and identity

Add the protocol beside the existing project structure, without relocating or rewriting history:

```text
projects/progrss/
  control/
    PROTOCOL.md
    jobs/<job-id>.json
    assignments/<attempt-id>.json
    results/<result-id>.json
    decisions/<decision-id>.json
    checkpoints/<checkpoint-id>.json
    receipts/<checkpoint-id>.json
    releases/<release-id>.json
    STATE.json                         # rebuildable current view
  work/<job-id>/<attempt-id>/...        # preserved deliverable files
  objects/sha256/<full-sha256>/...       # only when dedup/chunking is useful
  QUEUE.json                           # compatibility view, if still needed
  STATUS.json                          # compatibility view, if still needed
```

Keep this small. The directory layout above is normalized for clarity, not a requirement to implement a separate subsystem for each record. Version 1 needs only five record families: a current state view; jobs with assignment/claim history; results with review/decision records; checkpoint manifests; and verification receipts. Release information can be a checkpoint/receipt subtype. Separate assignment, decision, and release directories are optional organization of those fields.

Ordinary source and JSON can remain ordinary Git files. Use a content-addressed archive or the already-verified part format for large evidence only when transport needs it. Do not repack the entire 313 MB logical history for each change. Prefer direct full-file storage for the two current release artifacts and the small control records; inherited large evidence may continue to reference the verified existing archive.

### Stable IDs

- `job_id`: `progrss-j-<UUID>`; one bounded outcome, minted once by the current steward inside the authorized goal.
- `attempt_id`: `<job_id>-a<integer>`; a particular assignment of that job. A new worker or a restart from a checkpoint gets a new attempt.
- `result_id`: `<attempt_id>-r<integer>`; an immutable partial or final submission. Amendments produce a new result linked with `supersedes`, never replacement bytes under the old ID.
- `checkpoint_id`, `decision_id`, and `release_id`: type-prefixed UUIDs, minted once before the corresponding operation.
- `worker_id`: a recorded runtime identity for attribution, not the identity of the job. Slot numbers, local directory names, and product revision labels such as r018 are not globally unique work IDs.

UUIDs avoid requiring a durable counter for new jobs. Per-attempt result sequence numbers are local to one owner; after a lost owner or uncertain restart, allocate a new attempt instead of guessing the next number. Timestamps are useful metadata, not ordering or uniqueness guarantees.

Each record has `schema_version`, its stable ID, and a UTC creation timestamp. Hash exact UTF-8 bytes as written; do not reserialize an already-hashed record. A content reference contains a relative logical path, byte length, and full SHA-256. A durable external reference additionally contains the repository and full commit ID. References to `main`, `latest`, a local path, or a floating web URL cannot satisfy a recovery dependency.

For data inside the same checkpoint, use its relative path plus hash, resolved against the owning checkpoint's payload commit. Do not try to put a commit's own hash into a file inside that commit. For external dependencies, store the complete repository/commit/path/hash tuple.

The tuple protects identity, not availability or honesty: a hash cannot prove a test was actually run, that a reviewer is independent, or that a remote object will be retained forever.

## 3. Minimal records

The following are schema shapes, not live jobs or executable configuration. Angle-bracket values are placeholders. Reject unknown schema versions until a compatible reader is available. Permit explicitly namespaced extensions so the base contract stays small.

### Job

```json
{
  "schema_version": 1,
  "job_id": "progrss-j-<uuid>",
  "created_at": "<UTC>",
  "kind": "implement|review|assemble|recover",
  "objective": "<one bounded outcome>",
  "inputs": [{"repository": "<repo>", "commit": "<full commit>", "path": "<relative path>", "bytes": 0, "sha256": "<digest>"}],
  "depends_on": ["<job or accepted decision ID>"],
  "write_scope": ["<owned relative path or component>"],
  "acceptance": ["<observable criterion and required evidence>"],
  "limits": ["<task-specific non-goal or execution boundary>"],
  "deliverables": ["<expected artifact>"],
  "authority_ref": "<reference to the current user request, if available>"
}
```

`inputs` fixes the baseline. Each assignment is a tiny immutable record containing `attempt_id`, `job_id`, job-record digest, `worker_id`, the authoritative job checkpoint reference, and any preceding partial-result reference. Assignment does not alter the job. A materially changed objective or acceptance contract receives a new job ID with `supersedes`.

`authority_ref` is provenance, not an authorization token. Neither this file nor an old saved rule grants permission to future workers; the coordinator applies the current instructions and mandatory approval requirements at the time of action. Do not copy private policy notes, credentials, or standing-rule text into the journal.

### Result

```json
{
  "schema_version": 1,
  "result_id": "<attempt-id>-r0001",
  "attempt_id": "<attempt-id>",
  "job_id": "<job-id>",
  "job_sha256": "<digest>",
  "created_at": "<UTC>",
  "state": "partial|complete|blocked|failed",
  "supersedes": null,
  "base_inputs": [{"repository": "<repo>", "commit": "<full commit>", "path": "<path>", "bytes": 0, "sha256": "<digest>"}],
  "summary": "<short result or blocker>",
  "outputs": [{"path": "work/<job>/<attempt>/<file>", "bytes": 0, "sha256": "<digest>", "role": "source|patch|test|evidence|resume"}],
  "checks": [{"check_id": "<stable check ID>", "outcome": "pass|fail|blocked|not_run", "run_ref": {"path": "<run record>", "bytes": 0, "sha256": "<digest>"}}],
  "claims": [{"statement": "<bounded claim>", "evidence_paths": ["<path>"], "limitations": ["<scope limit>"]}],
  "resume": "<next concrete step, or none>",
  "conflicts": ["<known overlap or contradiction>"],
  "classification": "shareable|restricted|blocked"
}
```

A run record carries source/fixture/tool hashes; runtime versions; command and working-directory mapping with secrets removed; start/end times; exit status; test/case identities; raw-log references; and `observation_class` (`fresh`, `historical`, or `inference`). If a value is unavailable, say so rather than fabricate it. Record the actual candidate tested, not merely the job's base commit.

A result does not need tests to be preserved. It does need honest check states. `complete` means the worker's assigned submission is complete, not that the coordinator accepted it or it is durably stored. Changed source after testing invalidates the old test claim for that new source. A partial result should include the smallest restartable source/patch, known findings, and next step.

### Checkpoint

```json
{
  "schema_version": 1,
  "checkpoint_id": "progrss-c-<uuid>",
  "created_at": "<UTC>",
  "kind": "work|candidate|release|migration",
  "previous_checkpoint": {"repository": "<repo>", "commit": "<full payload commit>", "path": "<manifest path>", "bytes": 0, "sha256": "<digest>"},
  "record_ids": ["<job/result/decision IDs>"],
  "files": [{"path": "<relative repository path>", "bytes": 0, "sha256": "<digest>"}],
  "dependencies": [{"repository": "<repo>", "commit": "<full commit>", "path": "<path>", "bytes": 0, "sha256": "<digest>"}],
  "restore_instructions": "<versioned inert materialization instructions>",
  "classification": "shareable|restricted|blocked"
}
```

`previous_checkpoint` may be null only for the migration anchor. The checkpoint manifest lists all new payload files except itself. Its own bytes/digest are returned in the acknowledgment and recorded in the later receipt, avoiding a self-hash cycle. Dependencies form an acyclic recovery graph. A worker may use patches for review, but a release checkpoint must include the complete release source pair, so basic recovery does not require replaying an unbounded patch chain.

The **receipt** contains the checkpoint ID, destination/repository, exact payload commit and parent, checkpoint-manifest path/hash, verification time, verification method, checked-file count, dependency-completeness verdict, and any unresolved exceptions. It is written only after remote verification. A successful destination receipt requires no exceptions affecting recovery. Partial success at a destination uses a failure/pending record and cannot count as a complete copy, but it does not invalidate a complete verified copy at the other destination.

Keep one logical bundle ID and content digest across replicas. Define the digest over the same sorted logical file inventory of path, byte length, and SHA-256, including job/result/open-work records; exclude destination-specific transport locators. Transport manifests may differ in repository-local immutable dependency locations; each binds to this identical inventory. A sanitized projection with different content receives its own bundle identity and coverage statement, not the original's replicated status.

A complete replica must be reconstructible without reading the other repository. Public files that still require a private-only base archive do not provide independent public custody. During migration, materialize or mirror every disclosure-safe required dependency into the public lane before counting it complete. Provenance may cite an original private commit, but recovery bytes must also exist through the surviving destination's verified closure. Each destination also retains a short `START-HERE` entry point plus current open-job/attempt/resume records, pointing to exact checkpoint identities, so loss of the other repository does not leave recovered source without recoverable orchestration state.

The checkpoint's derived destination map records independent `pending`, `in_flight`, `uncertain`, `verified`, `failed`, or `ineligible` states, with the operation identity and exact receipt where known. One `verified` complete eligible destination earns `PRESERVED`; two earn `REPLICATED`. Never derive both destinations' success from one response. A timeout becomes `uncertain` until reconciled, not automatic failure or success.

The custody acknowledgment is issued after full payload/dependency verification; it does not wait for a second commit containing the observation receipt. Preserve the receipt in the next useful batch or one final small commit at handoff. Receipt persistence preserves the observation history, while the already-verified payload commit preserves the bytes. If an uncommitted receipt is lost, recover the committed checkpoint manifest, reverify the remote bytes, and create a fresh receipt with its actual new verification time. Do not relabel the new observation as the lost historical one. This avoids a mandatory second commit for every custody packet and any infinite receipt chain.

A **decision** is similarly small: decision ID, exact input result IDs/hashes, reviewer result references, accepted/rejected/needs-work verdict, rationale, candidate manifest/hash if any, remaining limits, and replacement decision if applicable. A **release record** names the accepted candidate, reviewed public allowlist, intended destination and parent, eventual public commit, verified output hashes, and deployment observation separately. Records may be created in multiple stages with linked immutable IDs; never pretend a pre-write intent is a post-write receipt.

## 4. Submission, acknowledgment, and integration

1. **Record the job first.** The steward prepares the job and assignment within the existing goal and routes them through a routine custody packet before expensive work. The coordinator verifies and publishes the packet without micromanaging each claim. Only changed scope, authority, or consequential trade-offs need a new root decision. If a tiny exploratory task runs before preservation acknowledgment, it is explicitly provisional and disposable.
2. **Work in isolation.** Give each attempt its own writable directory or checkout derived from immutable inputs. No two workers edit the same working files. Component ownership is a planning aid, not evidence that merges cannot conflict.
3. **Freeze a submission.** Write files to a staging directory, close writers, create the manifest, hash all bytes, and rename the complete bundle into a ready directory on the same filesystem. Atomic rename reduces partial local reads; it does not make the disk durable. A rejected/incomplete staging folder is not a result.
4. **Send a short notification.** Send result ID, outcome, manifest digest, bundle location, and the next decision needed. The coordinator or packaging worker reads files; neither chat nor a long report substitutes for the bytes.
5. **Preserve before deep review.** A packaging/review worker checks structure, safe paths, hash identities, dependencies, scope, and disclosure classification, then returns publication-ready manifests. The coordinator need not understand all technical details to preserve a well-formed disclosure-safe bundle. Mandatory data-sharing checks still apply before any upload.
6. **Dispatch eligible destinations independently.** For shareable work, the coordinator may start private and public preservation operations concurrently; either may proceed while the other is unavailable. Each destination gets its own expected remote parent, allowlisted paths, operation state, and non-forced conflict-detecting ref update. Preserve unrelated tree entries. If a destination's available route cannot prevent a stale writer from silently replacing its current state, stop that write until a supported safe route exists. A read followed by an unconditional overwrite is insufficient. Failure of one route is not permission to bypass its denial, but does not block the separately authorized other destination.
7. **Read back and acknowledge.** At each destination independently, verify its payload commit is reachable, read each newly uploaded payload by that exact commit, and compare length/hash with the manifest. Verify complete destination-independent recovery dependencies, reusing recorded verification only for unchanged immutable objects with its age disclosed. Create that destination's verification receipt and queue it for the next useful durable batch. On the first complete copy, send `PRESERVED <result-id> <payload-repo> <payload-commit> <manifest-path> <manifest-sha256>`, identifying the other destination's actual state. Once both complete copies are verified, record `REPLICATED` with both references. The committed manifests and exact payload identities permit fresh reconciliation even if an unsaved receipt or notification is lost.
8. **Adjudicate asynchronously.** A reviewer evaluates the result's contract; an assembly worker combines compatible results into a new candidate. Routine corrections and evidence disagreements stay with the author/reviewer or a third qualified worker. Their adjudication records enter routine custody packets. The coordinator consumes their compact verdict, evidence, conflicts, and limits and remains the final canonical integration authority; it resolves only consequential exceptions or unresolved scope/authority questions. Preserve rejected results and their reasons.
9. **Review the combined candidate.** Validate the actual combination, not just its ingredients. Rebase or conflict resolution creates a new candidate identity and requires affected checks. Results computed on an old baseline can still be preserved immediately; they cannot be silently relabeled as tested on a newer one.
10. **Release application changes only from an accepted manifest.** Public preservation under an explicit archive/work prefix may include shareable source, work records, logs, fixtures, and relevant project discussion before acceptance. It must not silently replace the live application's release files. Product release is a separate allowlisted action against `Sapient-Lumen/sapient-lumen.github.io`: root verifies accepted candidate, destination, complete bytes, parent boundary, and unchanged unrelated content, then records the release commit. A site deployment or GUI claim needs its own evidence. Repository preservation alone never authorizes peer/network operation.

Use distinct acknowledgments: `RECEIVED` means readable locally, `PRESERVED` means one complete eligible copy passed the above custody boundary, `REPLICATED` means both did, and `ACCEPTED` identifies a saved decision. `PRESERVED` allows discarding the last local copy of a submitted bundle; the surviving verified remote copy can supply the missing replica later. The worker can proceed to the next bounded slice after freezing it, but must retain unacknowledged bytes and cannot accumulate unlimited vulnerable work.

Notifications and submissions are **at least once**. Receiving the same ID with the same digest is a no-op plus acknowledgment replay. Receiving the same ID with different bytes is an identity conflict: preserve separately, quarantine from integration, and reconcile. Neither a timeout nor an absent message proves a remote write failed.

### Publisher ownership

Require one authorized publisher at a time, not one immortal coordinator process. After a coordinator restart, first reconstruct durable state, reconcile in-flight writes, and establish that the old publisher is quiescent before resuming remote writes. Record an ownership-generation ID in assignments and publication intents; attempts from an older generation may be preserved but need explicit current adjudication. This record is a logical safeguard, not a server-enforced lease.

Do not implement automatic leader election in version 1. If exclusive publisher ownership is ambiguous, freeze publication. A check-before-push or private ownership-generation record cannot atomically prevent an old in-flight writer from changing a separate public repository. Non-forced expected-parent updates catch conflicting branch histories, but do not solve every stale-writer interleaving. The private and public repository updates are separate transactions, each with reconciliation and its own receipt. No cross-repository atomicity or distributed fencing is claimed.

## 5. Six-worker throughput and coordinator load

Use the six available workers as a changing pool, not six permanent infrastructure dependencies. A useful steady-state split is three implementation/research workers, one assembly worker, one independent verifier, and one packaging/recovery worker. Rotate roles and lend spare capacity to ready work; these are functions, not compulsory fixed slots. The coordinator remains outside the six for user intent, priorities, consequential exceptions, final integration, and publication; the steward handles ordinary work routing.

Workers should return high-volume evidence to their assigned assembler/reviewer as immutable references plus short claim summaries. The assembler prepares a coherent candidate and identifies incompatible results; the verifier challenges its important claims. Both can recommend a decision. The coordinator reads a concise decision packet containing candidate identity, contract verdict, risks, conflicts, and the exact preservation/publication manifest. It digs into details when contradictions, high consequence, or weak provenance justify it, rather than redoing all specialist work.

Start with these tunable operating limits, not guaranteed service levels:

- Freeze a useful partial as soon as available, normally within about five minutes of new valuable work, and before a long test, execution-surface change, or worker handoff. Target verified remote custody within 10 minutes of the oldest new valuable work, including packaging and publication time. These are different boundaries; local freezing is not durability.
- Keep at most two globally prepared custody packets, including an in-flight packet. Coalesce compatible workers' partials into them. A worker's local cap of one frozen unacknowledged bundle plus one active bounded slice is subordinate to this global cap, not permission to build a six-packet root queue.
- At a 30-minute oldest-dirty-work age or a full global packet queue, stop admitting or extending loss-bearing production. Prioritize custody, the actual blocker, or review of already-preserved inputs. Record any already-running operation and its exposure; do not pretend the guard can undo past lost work or guarantee a wall-clock bound.
- Service the oldest unpreserved result first, with priority for irreplaceable observations or work about to lose its executor. Small batches may include several workers in one logical checkpoint mirrored to both eligible destinations. A slow worker or failing destination must not delay the first complete copy. Track replication debt separately and repair it without blocking already-durable production unnecessarily.
- Keep a short recoverable decision packet, not a giant coordinator-context summary. Raw evidence remains in referenced files, available on demand.
- At worker-capacity change, finish or checkpoint attempts, preserve their resume instructions, and reassign the same jobs with new attempt IDs. The protocol runs sequentially with one worker, more slowly but with the same records and safeguards. It does not assume six workers beyond the anticipated two-week window or create future workers automatically.

Track only four operational signals initially: oldest unpreserved-result age, unpreserved byte/count backlog, preservation latency, and unresolved candidate conflicts. These expose whether the serial publisher is becoming a bottleneck. They do not require a monitoring service; derive them from the journal when the work loop runs.

## 6. Crash boundaries and reconciliation

| Failure point | Durable truth and response |
|---|---|
| Worker dies before a frozen partial | Resume from its last preserved partial. Work since that point may be lost. |
| Bundle ready only on local disk | It is not durable. Preserve if still present; otherwise mark the result lost and restart. A chat summary cannot recreate missing bytes. |
| Some remote blobs/parts exist, no payload commit | No successful checkpoint. Reconcile known hashes and upload only missing objects. Unreachable remote objects are not retention guarantees. |
| Payload commit exists, acknowledgment absent | Locate its stable checkpoint ID and manifest, verify bytes and dependencies, then finish or recover the receipt. Do not duplicate the job or republish under a new ID merely because the call timed out. |
| Receipt exists, notification absent | Read and verify the receipt and its target; replay the same acknowledgment. |
| Concurrent root activity or remote parent changed | The non-forced conditional update must reject the stale publication. Re-read, preserve unrelated work, and rebuild the pending commit against the current parent. Never force-push to make the transaction fit. |
| Older worker returns after replacement | Preserve its attempt independently. Acceptance chooses explicitly among results; late output cannot overwrite the newer attempt. |
| Assembly modifies tested sources | The old test evidence remains historical evidence for the old candidate. Run the required changed-candidate checks before accepting the combination. |
| Public commit succeeds, private release receipt fails | Reconcile the public commit by the stable release identity and exact hashes, then save the receipt. Do not repeat public side effects blindly. |
| One preservation destination verifies, the other fails or times out | Acknowledge custody from the complete copy; retain the other's precise failed/uncertain state. Reconcile before retrying it. Report incomplete replication without claiming all work is still local-only. |
| A mirror lacks dependencies available only in the other repository | That mirror is incomplete and cannot count as independent custody. Supply the missing eligible dependency closure or disclose that replication remains incomplete. |
| Remote unavailable, rate-limited, or access denied | Mark the affected destination accurately and continue the separately authorized eligible destination. If no complete copy verifies, retain local bundles and stop claiming custody; apply the unpreserved-work limit. Retry transient faults appropriately; do not bypass a denied action or change credentials/permissions to continue. |
| Workspace path disappears or marker differs | Stop mutations in that attempt. Probe a known-existing parent once, inspect the path read-only, and recover into a fresh authorized directory. Do not loop forever launching in the missing directory or diagnose a global outage from that error alone. |
| Remote state is older, contradictory, or corrupted | Preserve the observations. Compare immutable receipts and referenced payloads; do not infer “latest” from wall-clock labels. Escalate ambiguity rather than silently reverting accepted work. |

After any uncertain write, reconcile before retrying. A new root must reconstruct active assignments and remote receipts before issuing replacement work. A lightweight attempt marker containing attempt ID, job digest, and baseline identity catches accidental use of an unrelated directory; it is not a backend container identity or a proof of continuity.

## 7. Recovery procedure

1. Start in a confirmed-existing authorized directory. Read the remote project state; treat `STATE.json`, `QUEUE.json`, and `STATUS.json` as convenient indices, not the only evidence.
2. Find the newest verified receipt and its payload manifest. Also scan later checkpoint IDs for committed payloads whose receipts were interrupted. Reconcile them before declaring work lost.
3. Fetch exact commits and verify manifests, files, and dependency closure. Reject unsafe relative paths, duplicate conflicting paths, absolute paths, parent traversal, unexpected symlinks, and archive expansion beyond declared limits. Materialize into a new directory as inert data first.
4. Recover the latest accepted candidate and separately the preserved unaccepted results. They are different sets. Restore unfinished job/attempt history and next steps from preserved records; do not reclassify them as completed.
5. Confirm tooling/runtime compatibility before executing any recovered script. Preserve original archives. Do not execute arbitrary historical commands simply because a resume note contains them.
6. Run a bounded resume check for the intended next job. Integrity success is not application correctness. Record new observations as fresh checks, keeping previous evidence and its limits intact.
7. Save a recovery result naming the input receipt, recovered file hashes, missing items, checks actually performed, and new local-to-logical path mapping. Continue with new attempt IDs when the old execution context is gone.

No browser state, localStorage, IndexedDB, live session, or credential is silently reconstructed by this source/evidence protocol. Browser persistence and multi-tab behavior require their own product-level contracts and tests.

## 8. Retention, disclosure, and evidence quality

Retain all job contracts, submitted-result manifests, decisions, release manifests, receipts, accepted source snapshots, referenced unique fixtures, and the evidence required to support claims. Preserve superseded and failed results while they explain later decisions. Keep original uploads immutable; work on copies.

Disposable build caches, dependencies reproducible from pinned manifests, and duplicate byte-identical artifacts can be excluded. Large repeated logs may be summarized only after unique failure evidence and the retained source hashes are captured; never silently trim evidence still supporting an unresolved claim. Reuse hash-identical objects and inherited manifests rather than dropping historical failures to save space.

For the initial two-week effort, use append-only retention and avoid cleanup of durable records. Afterward, any pruning needs a specific retention decision, a verified replacement snapshot, and respect for deletion approvals. Do not assume a branch will remain reachable after history rewriting; this design prohibits force-push and destructive cleanup as ordinary maintenance. It does not change repository protection settings or create tokens.

The user's explicit direction permits ordinary ProgRSS work and their relevant project discussion to be preserved in public and private concurrently. There is no blanket private-first rule. Keep a lightweight disclosure screen rather than re-asking for ordinary authorized project material. Still exclude credentials, authentication material, third-party private data, protected assistant instructions/internal notes, and unrelated files; mandatory sensitive-data and consequential-communication requirements remain applicable. Restricted material gets only its authorized destination, with its narrower replica count stated honestly. Content-safety/disclosure review is separate from technical acceptance; provisional technical work can be publicly preserved when eligible.

Trust workers' technical judgments through explicit claim/evidence/limitation records. Independence means a separate review of the actual candidate, not simply a second name on the same summary. Mechanical hashing checks identity; independent review checks meaning; targeted reruns check reproducibility. These are different assurances. Neither all-pass counts nor multiple reviewers establish exhaustive coverage.

Two GitHub repositories provide useful repository-level redundancy, but public and private repositories on the same account share provider/account/access failure domains. They are not independent disaster backups. An optional milestone export through the existing Library route can reduce that shared risk within applicable authorization; it requires no new infrastructure or credential here and is not silently created by this design. Preserve a short recovery pointer in the delivered progress report, but a pointer without retained bytes is not a backup.

## 9. Incremental migration and acceptance tests

### Migration

1. Anchor the migration record to private commit `91ac023dc71ff90d525bc714570c588e8f387ca0` and the fresh data-only audit. Retain the archive, manifest, reconstruction instructions, and their verified hashes unchanged.
2. Add protocol records beside `projects/progrss/work/`. Map existing queue entries to stable job IDs; record the migration mapping. Mark inferred or stale statuses as requiring reconciliation, not as newly verified facts.
3. Preserve one direct full-source r018 recovery pair with complete input/evidence references and explicit historical validation limits. This simplifies the next cold start without rewriting the older compressed archive.
4. Pilot one small shareable result through freeze, independent concurrent public/private payload commits, immutable readbacks, first-copy acknowledgment, two-copy replication completion, and receipt persistence in the next batch or final handoff commit. Recover it from each destination separately into a different directory, with the other destination treated as unavailable. Include a trial where the local receipt disappears before persistence and must be rederived from remote bytes.
5. Pilot one assembly decision from two immutable result inputs and verify the actual combined source. Only after the preservation loop works, expand concurrent production toward six workers.
6. Continue using existing public/private publication routes under current authority. Do not invent a new infrastructure project before the first end-to-end preservation test passes.

### Required protocol tests before calling it reliable

- Duplicate notification and duplicate submission produce one logical result and acknowledgment replay.
- Same ID with different bytes is rejected from integration and preserved as a conflict.
- Stop after each publication stage; recovery detects partial upload, committed-but-unreceipted payload, and receipted-but-unnotified success correctly.
- A stale parent update fails without overwriting unrelated tree entries.
- Missing, corrupt, or truncated parts and missing dependencies prevent that destination from counting as a complete preserved copy.
- A complete verified public copy earns custody while private upload fails; a complete private copy does the converse. Neither earns two-copy replication until the other independently verifies.
- Public recovery succeeds without private-repository reads, and private recovery succeeds without public-repository reads; provenance links alone do not satisfy missing bytes.
- A frozen bundle changed after hashing fails validation; publishers consume only the checked snapshot.
- Unsafe archive paths or undeclared output paths cannot escape the recovery directory or enter the published tree.
- An old attempt's late result cannot replace an accepted newer result implicitly.
- A passing result tied to different source hashes cannot validate a new candidate.
- The worker pool shrinks from six to one without losing job ownership, retained outputs, or the ability to resume.
- Cold recovery of the latest accepted source and unfinished work succeeds with the original local directory unavailable.

These are future acceptance checks for the protocol, not claims that they have been executed during this design task.

## 10. Exact guarantee and remaining risk

If an acknowledged checkpoint's repository, referenced commits, and dependencies remain readable and hash-correct, the protocol can reconstruct its recorded bytes and resume instructions without the original local directory or worker conversations. It can safely reconcile repeated notifications and interrupted publication using immutable IDs.

It cannot promise zero lost work. Work created after the last preserved partial may disappear, and a ten-minute checkpoint target is not a ten-minute worst-case loss guarantee during a publication backlog or remote outage. It cannot reconstruct unrecorded browser state, secrets, mental context, or observations whose bytes were never stored. It cannot by itself establish application correctness, guarantee backend/provider retention, prevent account compromise, or maintain work when every available execution surface is unavailable.

The useful operational standard is therefore: **small restartable units, prompt durable acknowledgment, explicit uncertainty, and verified recovery from immutable evidence**. Preserve first, adjudicate on exact candidates, publish only the approved boundary, and never make local continuity a prerequisite for remembering what the project has actually achieved.
