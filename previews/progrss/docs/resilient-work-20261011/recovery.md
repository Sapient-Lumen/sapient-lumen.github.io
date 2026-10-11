# ProgRSS recovery and bootstrap contract

Date: 2026-10-11. Status: proposed design for review. No recovery tool, application, replay, remote write, or historical-path restoration is implemented by this document.

Enqueue useful fragments immediately and flush small ready batches promptly. The ten-minute first-copy target is an initial age alert and engineering objective, not an intentional delay or a service guarantee.

## Decision

Treat each local workspace as a replaceable landing zone. The durable unit is an immutable repository checkpoint containing the source identities, bounded evidence package, declared inputs, work status, and verification receipts. A fresh workspace should recover that checkpoint into a new, inert, read-only generation without needing the previous directory, browser session, or worker process.

The first useful custody slice can preserve a small direct-file bundle through the existing reviewed route and recover it into a fresh directory. Build a versioned resumable bootstrap validator and the full fault-test suite as the subsequent hardening workstream; it need not block the first direct-file checkpoint. Keep the existing public and private repositories and the root integrator as the sole publisher. A scheduler, database, service account, new credentials, distributed worker coordinator, and automatic product startup are unnecessary for this first implementation.

The guarantee has a clear boundary: work becomes durable when at least one eligible destination has a verified immutable commit and the full required recovery closure. Under the user's concurrent public/private direction, both operations may progress independently; one complete verified copy establishes custody and two establish replication. Ordinary ProgRSS work and user project discussion may be shareable, while credentials, protected instructions, third-party confidential material, and other unapproved disclosure remain excluded. A public copy depending on private-only required objects is incomplete, and a narrower projection is not a full replica. Unpublished local edits can still be lost. A recovered byte-identical application is not evidence that its behavior, browser storage, or external services have recovered.

## Evidence and present baseline

The postmortem established that the historical project working directory is absent in the current filesystem view. An existing parent directory supports generic Bash execution; selecting the absent directory reproduces the process-start failure. The reason that the historical directory disappeared remains unknown. This design must work across either a replacement workspace or ordinary cache loss without asserting either as the incident cause.

The available private baseline is:

- Repository: `Sapient-Lumen/lumen-workshop`.
- Commit: `91ac023dc71ff90d525bc714570c588e8f387ca0`.
- Tree: `b6c488d5c5809c5ad597143641e75fdbed869c2e`.
- Package directory: `projects/progrss/work/offline-contracts-20261011/`.
- Manifest SHA-256: `d790d8b4096a1a9000d060d35d46aede6630089c6224a4a18c34ad7411b21d6c`.
- Archive SHA-256: `33eaf4c95ebcabbb382602001a619c52e32aef7e2a597b7fe2678b6607c3195a`; 9,736,371 bytes, transported in 38 parts.
- Inventory SHA-256: `185f26c2839a1472dc41ff7dc68913bebe176eee17625878b49e381ce0edb7fc`; 1,248,072 bytes.

The fresh data-only audit completed at 2026-10-11T14:14:00.461320+00:00 and verified 3,547 logical files totaling 313,342,850 bytes, 2,243 references, 23 splices, and 181 r018 contract dependencies. It reconstructed only these two files as inert data:

| r018 source | Bytes | SHA-256 |
| --- | ---: | --- |
| `index.html` | 532,300 | `f1b25affd69b74af6abb5a4f5d887ed0bab5b7cd22f007cab00f93e36bda80c6` |
| `ProgRSS.html` | 6,219,264 | `e805e6cffa4f4474a1b3384738e1cb07afe26619191c9de5a4957e0eba263c7c` |

Their logical directory is `workstreams-20261011/regression-replay-depth/integration/candidate-r018/`. The audit validated mode metadata but did not restore filesystem modes, execute archived scripts, run the app or tests, or recreate the historical project tree.

The public repository observation is separate: `Sapient-Lumen/sapient-lumen.github.io` commit `2047c007171fee22e79a345cb990e6d8c2c1da22`, tree `f24001825d5991602e6dc906160bd4e75ac8c19f`. Its rev0021 preview Git blob is `b18e67b554ffc8c8f733ab482e70a95fea4f5fe6`, size 516,042 bytes. That observation establishes repository identity, not a deployment or browser-behavior result. Private r018 must not silently replace it.

## Durable records and identity

### A small independent entry point

Add a private `projects/progrss/RECOVERY.json` descriptor and a separately reviewed bootstrap tool. The descriptor must not depend on knowing the old local path. Its initial fields are:

- Schema version; project identifier; privacy classification.
- Exact repository owner/name, provider, and stable repository ID when the provider supplies one.
- Immutable checkpoint commit and tree; package-relative path.
- Raw-byte manifest, inventory, source-pair, replay-contract, and validator identities: SHA-256 plus byte lengths.
- Supported package format and replay-contract versions, required features, resource limits, and read-only mode policy.
- Previous durable checkpoint reference, root integration state, and links to evidence supporting that state.

Read the descriptor at a pinned descriptor commit. It may point backward to the already completed package commit. This avoids embedding a commit's own hash inside that commit. Later remote receipts likewise identify a completed earlier commit and are preserved in a subsequent root checkpoint; they do not pretend to self-certify the commit that contains them.

A known repository and immutable commit are the bootstrap trust anchor. An arbitrary manifest plus matching files establishes internal consistency only. A mutable branch name, a familiar filename, a local receipt, or a successful HTTP response is insufficient to identify the intended checkpoint. Initial anchor selection must come from an already verified durable record or an explicit integrator choice. Record that selection and its provenance. Do not fetch and execute a verifier just because the archive contains it.

### Separate project identity from executor identity

At bootstrap, record the selected writable root, its resolved path, the available executor/session identifier if supplied, tool version/hash, and timestamp. Verify an existing allowed parent before starting commands. Generate a local recovery-attempt ID and use explicit working directories thereafter. Filesystem inode, mount metadata, hostname, and elapsed time can aid diagnosis but are not proofs of project identity or prior-session continuity.

Confirm the expected provider, repository identity, and private/public destination before reads or eventual publication. Resolve any mutable pointer once, record the returned commit, and use that immutable commit for every package read. Never combine parts from changing branch tips or substitute another repository after an access denial. Keep credential material out of descriptors, logs, commands, and receipts.

## Recovery stages and permitted claims

The bootstrap has these stages:

1. `ANCHOR_SELECTED`: intended repository, commit, tree, descriptor, and tool identity recorded.
2. `FETCHING`: exact pinned objects are arriving in a private staging directory.
3. `PACKAGE_VERIFIED`: transport, archive, inventory, every logical byte identity, and compatibility checks have passed.
4. `MATERIALIZING`: selected logical files are being written as inert data in a new generation.
5. `VERIFIED_READ_ONLY`: every selected output was re-read; the complete generation and receipt are sealed and atomically made discoverable.
6. `REPLAY_ELIGIBLE`: a separate reviewed plan confirms runtime compatibility, paths, dependencies, permissions, and execution authorization. This is not an executed replay.
7. `REPLAY_VERIFIED`: a separately authorized replay completed with its own exact-input receipt and disclosed coverage limits.
8. `DURABLE_VERIFIED`: the root publisher read back the intended remote checkpoint and required objects. Local recovery alone never reaches this state.

`PAUSED_REMOTE`, `BLOCKED_ACCESS`, `BLOCKED_COMPATIBILITY`, `BLOCKED_RESOURCE`, `QUARANTINED`, and `PUBLICATION_UNCERTAIN` are explicit side states. A missing or torn journal can reduce the claimed stage. It cannot advance one. A journal is a resumption hint; the bytes and their trust anchor decide whether a stage is actually complete.

The default command ends at `VERIFIED_READ_ONLY`. A `--verify-only` mode ends at `PACKAGE_VERIFIED` without full materialization. Full recovery and minimal r018 recovery must be separately identified in the receipt. Minimal recovery still validates the complete package and records the exact selected output set, rather than claiming that all files were written.

## Clean staging and byte validation

Use a fresh unique directory below the currently authorized writable root. Never recreate the missing historical root as a bootstrap side effect. Keep incoming chunks, verified objects, incomplete generations, complete generations, and quarantine records separate. Any optional active-generation pointer contains a relative generation name and receipt hash. It does not change application state or start a browser.

Before materialization:

- Parse bounded JSON with duplicate-key rejection and exact field types. Reject malformed digests, negative lengths, unexpected required features, and unsupported major versions. A Boolean is not a valid integer length or offset.
- Verify each part's encoded length/hash and decoded length/hash, the declared ordered part list, then the concatenated archive length/hash. Preserve raw source bytes; do not silently normalize JSON, newlines, or base64 before checking the identity at the corresponding layer.
- Validate the inventory hash before interpreting it. Verify CRCs as an additional archive check, never as a substitute for SHA-256.
- Require exactly the declared physical payload namespace. Reject duplicate members, unexpected payloads, file/directory collisions, absolute paths, traversal, noncanonical separators, special files, and symlinks. Reject destination-normalization collisions, including case or Unicode collisions where the target filesystem would merge names. Never follow an existing link during creation.
- Validate every direct payload. Resolve splices only against verified declared bases, check offset bounds and replacement identity, and verify each reconstructed result. Resolve references only through supported acyclic relationships. Reject missing bases, reference cycles, ambiguous entry types, and out-of-scope references.
- Verify every logical file's byte length/hash and the aggregate counts/lengths. Validate all declared contract dependencies independently of whether those dependencies are selected for materialization.
- Enforce local hard ceilings before allocation and during streaming. Initial r018-compatible defaults can be 16 MiB per metadata document, 64 MiB per member, 1 GiB total logical bytes, 20,000 logical files, and a bounded reference depth. These are implementation defaults to test, not claims of measured requirements. A package exceeding a limit pauses for review; it cannot increase its own ceiling.

Use only regular-file, exclusive-create writes within private staging. Write to a temporary sibling; flush file contents; validate the new bytes; rename within the same filesystem; then flush the containing directory where supported. Do not merge into an existing working tree. For unsupported durability primitives or filesystems, report the reduced guarantee and retain restart revalidation rather than claiming power-loss atomicity.

## Mode validation and quarantine

Record each source mode exactly and validate its syntax and supported semantic range. Keep original mode, effective recovery mode, and the reason for any difference separate in the receipt. Do not inherit ownership, ACLs, privileged bits, or executable behavior from the archive.

Read-only materialization uses owner-readable regular files and owner-readable/traversable directories, with no executable file permissions or group/world access. On a POSIX implementation, the sealed defaults are `0400` files and `0500` directories; temporary writing uses owner-only writable staging. Preserve original mode metadata in the inventory. A future working-copy promotion can apply individually approved necessary executable permissions; it does not change the preserved evidence generation.

This distinction is needed by the real r018 package: it includes a recorded `0o777` negative-mode fixture at `workstreams-20261011/offline-preservation-audit/outputs/negative-mode/a`, plus ordinary executable fixtures. Those bytes are valid evidence. Restoring their broad or executable permissions is neither needed nor appropriate for inert recovery. Accept them as data when otherwise valid, record the sanitized effective mode, and do not mislabel mode restoration as complete.

If any integrity or safety check fails, leave the last complete generation and pointer untouched. Preserve a small failure receipt and the suspect object identities in an owner-only quarantine area. Do not display recovered HTML in a live browser, import modules, run package hooks, or execute any archived script during inspection. Read-only permissions reduce accidental edits; they are not a security sandbox and do not make code safe to execute.

## Manifest compatibility and portable replay

The initial reader supports the observed package `format: 2`, inventory `format: 2`, and replay contract `version: 1`. Encode required features explicitly: direct payloads, exact-byte references, checked prefix/replacement/suffix splices, and source mode metadata. Unknown required features or major versions block recovery. Unknown optional metadata may be retained without interpretation. Any conversion is an explicit derived record with input/output hashes and a converter version; it must not rewrite the original archive or claim identical semantics.

The r018 replay contract embeds the old absolute project root in `original_root`, job arguments, and output paths. Recovery therefore cannot simply run its command arrays. Generate a separate replay plan by mapping only declared path-valued arguments from the old root to the new verified generation, and mapping declared outputs to a new writable run directory. Log every mapping. Reject undeclared absolute paths or reads/writes escaping the designated roots. Never use blind text replacement inside application source, fixtures, arbitrary arguments, or scripts.

A script may still contain an old path internally. Such a dependency blocks portability until a reviewed adaptation or explicit supported parameterization exists. Do not recreate or alias the old root to hide the incompatibility. Any source/test adaptation receives a new identity and is described as adapted replay, rather than byte-identical execution of the archived contract.

Preserve the declared runtime tuple, currently Node `v24.19.0`, Python `3.12.14`, and lxml `6.1.1`. Inert reconstruction does not require these runtimes. A replay plan records observed versions and deviations; it cannot call a different runtime an exact replay. Do not install dependencies or obtain credentials automatically. Execution, installation, security-sensitive changes, and network behavior remain subject to their applicable authorization gates.

## Resume and partial-write rules

Derive a recovery key from the pinned repository/commit, raw descriptor or manifest identity, validator version, selected output set, and effective mode policy. Attempts for different keys never reuse an in-progress generation. A new attempt may reuse content-addressed verified objects only after rechecking their hashes.

Keep a compact journal of completed objects and stages. Update the journal by a temporary-file, flush, atomic-replace sequence. The journal never marks a part or output verified before the corresponding verified write is durable under the supported filesystem contract. On restart, ignore temporary files as proof of completion and revalidate every object reused. If the journal is absent or corrupt, reconstruct progress from verified objects; do not execute a repair script from the package.

Seal a completed generation with its selected-output inventory and final local receipt, then atomically publish the generation name and receipt identity as the optional active pointer. A crash before pointer publication leaves the old complete generation active. A crash after publication is safe only if the pointed generation and receipt verify; otherwise retain/report the prior verified generation and block activation. Preserve enough previous-pointer information to resolve that condition. Readers never consume a generation marked incomplete.

Allow one writer per recovery key using an exclusive local lock with attempt ownership. A stale PID alone is not permission to steal a lock; a restarted process has to establish that the previous owner is gone or select a new isolated attempt. Six workers must not share a mutable recovery target. They can read one sealed generation, then use isolated working copies and separate output directories. Identical retries reuse the same publication intent ID through the root integrator.

## Durable states and publication receipts

Track these independent facts per worker deliverable and root integration checkpoint:

- `LOCAL_ONLY`: exact local content exists but no remote durability is established.
- `REMOTE_COMMITTED`: an intended commit is known, with readback pending.
- `DURABLE_VERIFIED`: remote commit/tree and every required artifact identity were read back successfully.
- `INTEGRATION_REVIEWED`: identified worker outputs were reviewed against a specific parent/base and integration result.
- `REPLICATED_VERIFIED`: both intended destinations contain complete verified copies of the same logical bundle. A differing projection has separate coverage.
- `PUBLIC_CHECKPOINT_VERIFIED`: a public provisional or accepted checkpoint has independently verified bytes. Product release acceptance, deployment, and browser behavior remain additional evidence.

Store task boundaries, base checkpoint, coordinator generation, worker attempt identities, artifact identities, parent/input references, outstanding blockers, test receipts, and next permissible action in the complete shareable queue/status bundle at each eligible destination. Keep genuinely restricted metadata separately. Public recovery must work without private-only records. Local paths are optional location hints. A worker's durable handoff names repository, immutable commit, path, and digest. The root records which worker results it accepted and publishes a consistent integration commit; no worker advances a shared mutable "latest" pointer independently.

There is one authorized publisher at a time, not a requirement for one immortal root process. A replacement coordinator first reads durable status, reconciles unresolved publication intents by exact remote identities, and establishes exclusive ownership before writes resume. If the old writer may still be active or an in-flight write cannot be reconciled, publication freezes. Recovery and static verification may continue in isolated inert staging. A new durable coordinator generation invalidates old worker attempts; late results remain reviewable candidates and cannot automatically advance accepted state. Expected-head checks guard repository updates against observed conflicts. These checks are not leader election and do not fence a delayed write to another repository. Do not claim automatic split-brain-safe failover or a transaction spanning the private and public repositories.

Each receipt records schema, recovery/publication intent ID, repository/commit/tree, validator identity, exact input/output identities and counts, original/effective mode policy, selected scope, stage, timestamps, runtime evidence, deviations, failures, and explicit execution flags. Keep transport error details and private paths out of public receipts. A final local receipt should include `application_executed: false` and `contract_replay_executed: false` unless separate evidence establishes otherwise.

The last-known-durable state advances only after successful readback. If a remote write might have succeeded but its response was lost, mark the intent `PUBLICATION_UNCERTAIN`, inspect the target commit/tree and intended identities, and reconcile before retrying. Do not create duplicate commits merely because the local acknowledgment disappeared. Publish the package and status transition together when possible. If the transport cannot do that, publish immutable package objects first and advance the root pointer only after complete verification.

If either destination succeeds while the other operation or receipt write fails, its complete verified copy remains valid custody. Reconcile exact commit, coverage, and hashes; retry only the unresolved destination after its state is known. Do not repeat successful publication or roll it back merely to make both records agree. Persist shareable receipts through whichever eligible copy is available, then replicate when possible. Public/private heads and verification times remain separate fields. No private-first prerequisite applies.

Two repositories on the same GitHub account provide repository-level redundancy, not protection against all provider/account failures. An optional milestone bundle through the existing authorized Library/export route can provide another recovery route without requiring a new service or credential.

Remote receipts are historical evidence. A new recovery records whether each remote identity was freshly read or only preserved in a prior receipt. Exact hashes do not create an independent backup, prevent provider loss, or prove ongoing remote availability.

## Failure classification and bounded response

| Observation | Classification | Response | Claim that is prohibited |
| --- | --- | --- | --- |
| Old directory missing; verified remote checkpoint readable | Local cache unavailable | Recover into a fresh directory from the pinned checkpoint | Claiming why the old directory vanished |
| Existing permitted cwd works; historical cwd fails | Working-directory precondition failure | Use an existing permitted parent for bootstrap; keep diagnostics | Claiming all shell execution is unavailable |
| Network timeout or provider service error | Remote availability failure | Preserve verified local generation, checkpoint partial fetch, pause and retry under the active task | Claiming remote data was deleted or corrupt |
| Authentication or authorization denial | Access blocked | Stop that route and ask for the needed authorized action | Alternate routes or credentials to evade denial |
| Pinned object absent | Missing checkpoint object | Record exact object and provider result; verify known durable predecessor without silently substituting it | Claiming all repository history is lost |
| Manifest or payload digest mismatch | Integrity failure | Quarantine suspect bytes; preserve known-good state; re-fetch the same pinned object when permitted | Treating a changed manifest as an automatic repair |
| Valid bytes but unsupported schema/runtime/path mapping | Compatibility block | Keep verified data inert and report the exact unsupported feature | Claiming product corruption or successful replay |
| Disk full, interrupted write, or process killed | Incomplete local attempt | Revalidate and resume from completed objects; leave last complete generation active | Advancing completion from a partial journal |
| No remote and no complete verified cache | Recovery unavailable now | Report the last independently known durable anchor and exact missing inputs | Inventing a successful restore |

A known durable predecessor is an explicit recovery option, not an automatic downgrade. Name the lost/newer work interval and obtain the integrator's selection before promoting it. No amount of retrying an unavailable remote establishes a newer durable state.

## Acceptance criteria and fault proofs

The hardened archive bootstrap is accepted only when automated, data-only tests demonstrate all of the following. A first small direct-file custody slice has the narrower end-to-end acceptance criteria in the integrated brief and need not implement this entire utility first. Consuming the archived package still requires all applicable integrity and safety checks. These are proposed tests, not results of the present design exercise.

1. **Fresh landing zone:** recover the pinned r018 package in an unrelated empty allowed path. Validate all 3,547 logical identities and all 181 dependencies; materialize the selected r018 pair with the exact hashes above. The old path is never created or required.
2. **No execution:** instrument child-process, module/import, browser, and external network execution surfaces so recovery cannot trigger archived code. Fetching uses only the authorized transport. Recovered HTML remains inert.
3. **Restart at each write boundary:** terminate before and after chunk writes, object promotion, journal replacement, generation seal, and pointer replacement. Restart yields either the previous complete generation or a fully verified new one, never a mixed tree or false completion.
4. **Transport faults:** interrupt each of the 38 parts, repeat a part, change its order, truncate it, and alter encoded or decoded bytes. Only declared valid parts are reusable; failures do not change the active generation.
5. **Manifest and archive faults:** exercise duplicate JSON keys, unsupported versions/features, wrong counts, extra/duplicate members, path escape/collision, invalid mode metadata, absent bases, reference cycles, invalid splice bounds, and resource-cap breaches. Each produces the correct bounded block/quarantine result.
6. **Mode evidence:** the real `0o777` negative fixture remains recoverable as bytes with mode divergence recorded. No special bits, world-writable mode, symlink, or executable file permission is restored by the data-only path.
7. **Local corruption after success:** change one cached object, one selected source, and the receipt/pointer in separate cases. A resume/read verifies identities, refuses invalid state, and never treats the old success label as authority.
8. **Concurrency and duplication:** launch two bootstrap attempts for the same key and six readers of a sealed generation. No shared mutable output or cross-generation mixing occurs. Restarting a publication reconciliation does not produce duplicate publication intents.
9. **Remote unavailable:** verified local read-only evidence stays usable with a stale-remote observation label. Without a verified cache, recovery reports the missing remote dependency and does not fabricate a state transition.
10. **Portable replay plan:** detect every declared old-root path, generate and hash an explicit mapping, isolate outputs, and block unknown absolute paths/runtime deviations. Do not execute the 82 replay jobs in this bootstrap acceptance suite.
11. **Receipt fidelity:** verify that the original historical report remains described as 81/82 broad-run jobs plus one isolated successful retry, yielding 82 supported latest outcomes. Recovery cannot relabel it as a fresh flawless broad run or fresh GUI/native-storage acceptance.
12. **Durable checkpoint reconciliation:** with a small synthetic repository transport fixture, simulate a successful commit followed by lost acknowledgment, a partial package, and a rejected pointer update. Only verified complete content advances the durable pointer, and uncertainty is reconciled before writes repeat.
13. **Coordinator replacement:** lose the root's local journal, supply a late result from an older worker attempt, and leave one simulated public write unresolved. A new coordinator can recover data and read durable status, but cannot publish until ownership and the in-flight result are reconciled. An epoch change alone never counts as cross-repository fencing.

Fault tests establish the stated process-interruption guarantees under the tested filesystem semantics. Actual host power-loss testing, native browser-storage durability, multi-tab correctness, live feeds, and application recovery are separate acceptance work.

## Estimated implementation tasks

These are engineering effort estimates for the subsequent hardened archive bootstrap, excluding approvals, outages, product changes, and full application replay; they are not a prerequisite estimate for the first small direct-file checkpoint. They are not calendar commitments.

| Task | Deliverable | Estimate |
| --- | --- | ---: |
| Define the descriptor and receipt schemas | Versioned schema, r018 descriptor, state/error vocabulary, trust-anchor rules | 0.5 day |
| Implement the inert bootstrap core | Pinned reads, strict parsing, hash graph validation, bounded read-only materialization | 1 to 1.5 days |
| Add resumability and generation switching | Verified object reuse, lock/journal, partial-write handling, sealed generation pointer | 0.5 to 1 day |
| Add portable replay planning | Explicit old-root mappings, isolated output plan, runtime/permission blockers; no automatic execution | 0.5 day |
| Prove the failure matrix and document recovery | Fault injection, r018 data-only acceptance, concise operator runbook | 1 day |

Total: approximately 3.5 to 4.5 engineer-days. Reuse the independent data-only audit's validated logic as a reviewed starting point, but do not rerun it in place or turn its archived outputs into executable authority. The current audit deliberately uses exclusive output creation and is not a resumable bootstrap utility.

First direct-file milestone: obtain one complete pinned eligible checkpoint in a fresh directory, verify its bytes and resume state, emit a truthful read-only receipt, and stop. The later archive-bootstrap milestone proves the full r018 recovery matrix. Later product replay or publication is an explicit next stage, owned by the root integrator.

## Evidence references

- `../ProgRSS-postmortem-evidence-appendix-2026-10-11.txt`: missing-cwd differential control, current repository observations, and historical receipt qualification.
- `../recovery-audit/RECOVERY-AUDIT-RESULT.json`: fresh data-only audit, SHA-256 `829caf0a3eefe5dc577e013e4c82ab8fae3a9b2b7652a60179b3b18b27189138`.
- `../recovery-audit/MANIFEST.json` and `PACKAGE-CONTENTS.json`: package format, part transport, complete logical inventory, and recorded modes.
- `../recovery-audit/CONTRACT.r18-adapted.json`: exact dependency identities, runtime tuple, and historical absolute-path arguments.
- `../recovery-audit/FINAL-RESULT.json`: historical outcome interpretation and explicit product acceptance limits.
- `../recovery-audit/AUDIT-STEPS.txt` and `audit_archive.py`: scope of the freshly run independent audit and its non-resumable exclusive output behavior.
- `../evidence/recovered-private89-receipt.json`: historical publication readback record; its recovery does not constitute repeating all original readbacks.
