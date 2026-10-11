# ProgRSS: fast, safe preservation across private and public repositories

Design proposal, 2026-10-11. No remote writes, account changes, credential changes, deployment changes, or implementation are performed by this document.

Enqueue useful fragments immediately and flush small ready batches promptly. The ten-minute first-copy target is an initial age alert and engineering objective, not an intentional delay or a service guarantee.

## Recommendation

Use a root-owned, bounded publication queue with immutable intake bundles. Prepare eligible ordinary ProgRSS work and user project discussion for concurrent public/private preservation, as the user authorized on 11 October. One complete verified eligible copy establishes custody; two complete copies establish replication. Technical integration and product deployment remain separate. Useful unreviewed work can be preserved publicly with a provisional label after disclosure checks.

The principal optimization is smaller units and fewer redundant bytes, not weaker publication checks. Every branch update retains the complete-tree guard, exact parent/tree checks, fresh ref check, non-force update, and immutable readback.

The root remains the sole publisher. Workers prepare immutable results and submit small descriptors; they do not write either remote. This means one authorized publisher at a time, not an assumption that one process lives forever. Recovery reads durable state, reconciles in-flight writes, and establishes exclusive ownership before writes resume. If an older publisher may still be writing, freeze mutations until it is quiescent. A private ownership epoch or logical lease does not fence a public write at the other repository. This minimal design does not claim automatic cross-repository takeover or fencing. This design creates no workers, scheduler, persistent service, webhook, or new access grant. Implementation belongs in the already authorized work loop.

## 1. Scope and existing evidence

- Private repository: `Sapient-Lumen/lumen-workshop`, under `projects/progrss/`.
- Public repository: `Sapient-Lumen/sapient-lumen.github.io`. Publication must preserve the primary site. Use an explicitly approved preview subtree such as the existing `previews/progrss/` namespace; do not replace root `index.html`, root configuration, or site-wide assets with preview content.
- Discover actual branches and deployment configuration before implementation. Do not assume a default branch name or that a nondefault branch is publicly hidden.
- The current postmortem records private checkpoint `91ac023dc71ff90d525bc714570c588e8f387ca0` and a fresh 14:14 UTC data-only recovery audit of its 38 parts, 3,547 logical files, and 181 r018 dependencies. These are recovery inputs, not proof of new application acceptance or current branch freshness.
- The same report records public checkpoint `2047c007171fee22e79a345cb990e6d8c2c1da22` and a prior public preview. These pins are historical context; a publisher must resolve fresh tips.

Local inputs reviewed for this proposal: the current postmortem text; `recovery-audit/RECOVERY-AUDIT-RESULT.json`; and the existing publication guard implementation/documentation. The reviewed guard copy has SHA-256 `07f5e1b247ef870c708fe854a6cc50e54cb703c7ea3bedee5a267dee4e87eb14`. This identifies the copy inspected, not a claim that it is the latest production guard.

## 2. State vocabulary and honest acknowledgments

Track independent facts rather than one misleading done flag:

1. **Prepared locally:** exact bundle and manifest exist here; loss remains possible.
2. **Preserved:** at least one eligible destination has a reachable immutable commit, exact byte readback, and the complete required recovery closure.
3. **Replicated:** both intended destinations independently satisfy that condition for the same bundle identity and coverage.
4. **Accepted:** the exact candidate meets its technical review contract; preservation of failed or provisional work does not imply acceptance.
5. **Released and checked:** a reviewed product release and any claimed deployment/live behavior have their own evidence.

Return a custody acknowledgment after the first complete verified copy, naming its destination, commit, manifest digest, coverage, and verification time. Keep the other destination visibly pending, unknown, verified, or blocked. A public summary depending on private-only archive objects is not a complete public replica. If a public projection differs, give it a separate identity and describe its narrower coverage.

No local-only algorithm guarantees survival before remote custody. Atomic rename, flushing, and duplicate local directories do not establish an independent durable copy. Measure unpreserved age/volume separately from preserved-but-not-yet-replicated backlog. Save verification receipts in the next useful batch or a final small batch; immutable intent and remote content permit reconciliation if a local receipt disappears.

## 3. Immutable handoff and storage format

### Worker contract

A worker finishes a bounded logical unit into a fresh staging directory, closes its files, computes a canonical manifest, and presents a short ready descriptor. The root independently verifies bytes before accepting the unit. Do not keep appending to submitted files. A correction is a new generation with a `supersedes` relationship.

The descriptor contains task/result ID, generation, manifest SHA-256, byte/file counts, dependency IDs, intended privacy class, and a concise outcome. It never needs full logs or base64 file payloads in conversational output. Treat worker classifications and success claims as inputs to verify, not authority to publish.

### Proposed logical layout

Within `projects/progrss/preservation/`:

- `objects/sha256/<prefix>/<digest>`: immutable payloads or small deterministic packed segments.
- `bundles/<bundle-id>/manifest.json`: paths, logical modes/types, byte lengths, payload hashes, dependencies, outcomes, and provenance.
- `batches/<batch-id>.json`: bundle IDs selected for one publication, pending public projection intents, and policy versions.
- `receipts/<operation-id>.json`: subsequent verification observations about earlier publications.

These paths are proposed additions for the complete shareable bundle at either destination. Destination-specific outer paths may differ while logical manifest identity and coverage stay equal. Check for existing contracts/collisions and choose compatible names before creating them. Never overwrite an existing content-addressed path with different bytes. Private storage is still unsuitable for credentials or content whose upload is not authorized.

Bundle identity is SHA-256 of canonical semantic manifest bytes. Specify one serialization (UTF-8, deterministic key order and number representation, no insignificant whitespace) and exclude volatile observations from identity. Retrying the same generation must not change IDs merely because the clock advanced. Store SHA-256 over payload bytes and the independent Git blob object ID; these are different hash domains. Root rehashes both from the bytes it will send.

Preserve file modes/types explicitly. For inert packed evidence, the outer Git file can be mode `100644` while the manifest records each logical member’s original executable or symlink metadata. Restoration must validate relative paths, collisions, size limits, symlink handling, and hashes before recreating those modes. Preservation does not execute submitted scripts.

### Avoid repeated giant archives

- Store changed source files directly when practical. Reference unchanged verified Git objects or content-addressed payloads.
- Pack many tiny logs once per bounded batch, with deterministic ordering and metadata. Keep an index with each logical member’s hash and size. Store raw binary payloads where the authorized transport permits; API base64 is a transport encoding, not a reason to store permanent base64 copies in Git.
- Existing original uploads and archived checkpoints remain unchanged. Do not rebuild the 38-part archive for each worker message.
- Each result has direct references to immutable dependencies. Avoid unlimited prefix/splice chains. A new checkpoint may compact the replay index while reusing unchanged objects; the default design does not prune earlier preserved generations.
- Never deduplicate solely by path, timestamp, file length, or a worker-supplied digest. A referenced object must be known reachable and its bytes previously verified, or verified now. Recovery from a cold environment revalidates the necessary closure.

A batch is a transport/commit optimization, not an archive format that absorbs all past work. Missing dependencies block complete-bundle acknowledgment; unrelated self-contained bundles can proceed.

## 4. Fast lane, integration lane, and public lane

### Concurrent preservation lanes

Preserve shareable source, test results, relevant evidence, user project discussion, and explicit failures in both eligible intake namespaces after bounded checks: manifest validity, byte identities, path/mode safety, complete recovery closure, disclosure eligibility for each destination, and all publication invariants below. Neither destination waits for the other. Do not make preservation wait for GUI acceptance, full application integration, consensus, or a polished report.

Use append-only checkpoint subtrees on the established authorized branches when their contracts allow it. The public checkpoint must not replace the primary site or execute a provisional app as a side effect. Separate preservation branches are optional destination choices, not prerequisites for speed. Do not alter workflows, branch protection, repository visibility, or permissions as an incidental optimization.

### Integration lane

Materialize verified bundles into a fresh candidate working copy. Resolve overlaps explicitly, run project contracts/tests, then publish a reviewed integration commit with bundle lineage. Preserve conflicting variants separately rather than selecting whichever worker finished last. Preservation state remains true if integration fails.

### Public eligibility and product release

Build public content from a strict output allowlist. The shareable core may match the private core exactly; produce a narrower projection only when something cannot be shared. Unknown disclosure eligibility fails closed for that content. Technical disagreement alone is not a reason to withhold an honestly labeled provisional checkpoint. For each public file, record its source lineage privately, allowed destination, transformed byte hash, mode, and projection/review policy version. Generate a clean manifest and inspect the actual final bytes, including embedded data, HTML comments, source maps, generated metadata, and nested artifacts. Bind review/scan evidence to the exact final projection manifest and candidate tree; any change invalidates that evidence until reviewed again.

Exclude credentials, protected instructions, third-party confidential data, and personal/sensitive information whose specific disclosure is not authorized. Ordinary ProgRSS work and the user's project discussion are authorized for public preservation. Inspect raw logs, nested archives, hidden configuration, API responses, and metadata for excluded material rather than declaring all project evidence private by default. Secret scanning is supplementary; absence of a regex match does not establish publishability. Do not copy a private manifest and merely delete a few obvious fields. For any genuinely restricted material, even hashes can disclose linkage; publish only the reviewed public payload identity. Shareable bundle hashes may match both copies. If restricted content is omitted, the projection must not claim to replicate it.

Use a public-specific projection ID. A preview release may be stored at `previews/progrss/checkpoints/<projection-id>/` if that path is compatible with the site. Never expose private Git history by pushing a private branch or history into the public repository. Construct a new commit on the public baseline with public-only bytes. Creating any blob in the public repository is already a disclosure boundary, so complete review before object upload, not just before the ref update.

For the primary site, the manifest must show exact preservation of every unrelated leaf, mode, and type. Preview additions do not authorize root redirects, navigation replacement, workflow edits, or primary-site promotion. If previews are served on the primary origin, review service-worker scope, shared browser storage, cookies, redirects, and global assets; a nested path alone is not browser isolation. A public source-only preservation path can avoid loading a preview until live behavior is approved.

Include a shareable batch intent and recovery index with each eligible copy. Both repository operations may start and progress concurrently under the same authorized publisher. Their outcomes and receipts remain independent. If private preservation is unavailable, eligible public preservation continues; if public is unavailable, private continues. Public disclosure checks must pass before creating any public object. Content that is not public-eligible cannot use public storage as a fallback.

## 5. Mandatory publication transaction, per repository

The root performs this sequence for private preservation, integration, public projection, and later receipts alike:

1. Resolve exact repository identity, intended branch/ref, baseline commit `B`, and its tree `T0` through the authorized service. Reject a ref to an unexpected object type. Record the pins and complete baseline tree.
2. Freeze selected manifests and independently hash every intended new byte stream and its Git blob framing. Review exact before/after path, mode, type, and object IDs. Existing paths and deletions need explicit intent; absence from an upload list never means deletion.
3. Upload only reviewed new blobs. Build a candidate tree `T1` using explicit `base_tree=T0` plus the exact patch. Avoid ambiguous simultaneous directory-tree replacement and nested-path edits. Never use a patch-only tree as the repository root. GitHub documents both the overlay behavior and the deletion risk if the base is omitted. [GitHub tree API](https://docs.github.com/en/rest/git/trees)
4. Retrieve the actual complete candidate tree and run the existing offline guard against independently pinned `T0`, `T1`, and the reviewed change manifest. A create-tree response or patch list is not the candidate inventory. Stop on nonzero exit. Truncated inventories must be expanded from immutable subtrees and canonically verified before use; do not relabel a truncated response as complete. [GitHub tree retrieval](https://docs.github.com/en/rest/git/trees#get-a-tree)
5. Create proposed commit `C` with exactly the intended sole parent `B` and tree `T1`, then retrieve it and verify parent/tree and applicable metadata. Pin commit metadata in the attempt record to make an uncertain creation easier to reconcile. [GitHub commit API](https://docs.github.com/en/rest/git/commits)
6. Re-read the target ref immediately before mutation. If it differs from `B`, stop this attempt, record it, and rebuild/review against the new base. Do not silently substitute a new parent while keeping an old candidate tree.
7. Advance only the intended ref with `force=false` or an ordinary non-force Git push. No force, force-with-lease, mirror push, broad refspec, or automatic history rewrite. GitHub documents its non-force update as a fast-forward check. [GitHub ref API](https://docs.github.com/en/rest/git/refs#update-a-reference)
8. Read back `C` by immutable identity, the complete tree, and all newly changed blob bytes; verify exact byte hashes, modes, types, parent, and tree. Read the branch again. If it has advanced beyond `C`, establish ancestry and report separately whether `C` remains reachable and whether current paths still match. A blob existing without a reachable accepted commit is not a complete preservation result.
9. Emit the per-repository outcome receipt. A successful API response without readback is `write_observed_verification_pending`, not verified publication.

The existing guard additionally reconstructs canonical directory hashes and rejects undeclared changes, incomplete trees, malformed modes/types, and duplicate paths. It does not prove authorization, remote freshness, reviewed blob bytes, commit parentage, secret safety, or successful deployment. Those remain explicit caller checks.

### Concurrency boundary

A fresh read followed by GitHub’s ref PATCH is not an atomic expected-old-SHA compare-and-swap: the documented update body has `sha` and `force`, not an expected-old field. With `C` built directly on `B`, an ordinary competing forward commit will make `C` fail the non-force check; unrelated history is preserved. This is not a guarantee against another actor force-rewinding the branch, deleting it, or otherwise rewriting history between calls. Root serialization covers this publisher only. [GitHub ref API](https://docs.github.com/en/rest/git/refs#update-a-reference)

Do not claim stronger exclusion than the available transport actually provides. The current connector schema advertises `expected_sha` and says a mismatch is rejected. Use that guard with the pinned baseline and `force=false`, alongside the fresh ref read. This is a connector-level advertised check, distinct from native REST fast-forward behavior; its atomic implementation is not proved by this design. A connector hiding required pins/flags or the complete candidate tree is insufficient for this path. Pause publication or use another already authorized supported path; do not weaken the guard or change permissions to make progress.

### Safe performance optimizations

Cache verified immutable objects by repository and object ID, reuse unchanged blob IDs, batch mutations into one tree/commit, and reuse complete immutable subtree inventories. Reassemble the entire candidate input for the existing guard, rather than checking only changed files. Cache reuse never substitutes for branch freshness or required readback of new bytes. After a cold restart, cache trust must be reconstructed. Full-tree validation remains a real cost and should be measured, not concealed.

## 6. Batching, rate limits, and output backpressure

Proposed v1 defaults align with the capacity plan: useful partials as soon as available, first complete verified copy within 10 minutes of new valuable work when possible, a 30-minute oldest-unpreserved admission guard, and at most two globally prepared custody packets including one in flight. A packet can target both repositories. About 5 MiB or 50 changed files is a batching hint, not a transport ceiling or permission to omit required bytes. These are tuning hypotheses, not service guarantees.

Use one authorized publisher and serialize mutations to each target ref. The two independent repository operations can overlap or interleave without waiting for each other's success; honor shared API limits. Give first-copy custody priority, then drain missing replicas and ready product releases without hiding either backlog. Stop expanding local-only output when custody cannot keep up. A failed secondary copy does not erase existing custody, but unresolved replication must remain visible and bounded.

Detailed transport choices such as five-second flush windows, descriptor limits, deterministic tiny-log packs, and service-share weights are optional later optimizations, chosen only after measuring actual ingress/drain rates, remote latency, API budget, and root queue time. Do not impose a second competing operational timing policy before the first useful checkpoint.

GitHub recommends serialized REST requests and at least one second between mutating requests when making many changes. Respect response limits, `Retry-After`, and reset timestamps; follow documented backoff for secondary limits. Logical concurrency can interleave serialized requests where appropriate. A permission denial is a stop at that boundary, not a reason to change accounts, routes, or tokens. [GitHub REST best practices](https://docs.github.com/en/rest/using-the-rest-api/best-practices-for-using-the-rest-api), [GitHub rate limits](https://docs.github.com/en/rest/using-the-rest-api/rate-limits-for-the-rest-api)

Reuse verified immutable objects and the existing authorized publication route. No new credential or access grant is needed merely to improve speed. Outages, backoff, and process loss can defeat any timing target; report observed exposure honestly.

## 7. Multi-repository reconciliation and retry discipline

Treat the operation as a small durable workflow with independent results, not a distributed atomic transaction. Git atomic push applies within one remote transport, not across these two repositories. There is no rollback that erases an already disclosed public file from all readers. [Git push documentation](https://git-scm.com/docs/git-push)

For every destination retain `not_started`, `prepared`, `attempted_unknown`, `ref_observed`, `verified`, or `blocked`, plus commit/tree and attempt evidence. Retry only the incomplete side. Never republish a verified copy merely because the other destination timed out.

After any ambiguous mutation result, inspect the intended immutable commit, exact batch manifest, target ref, and ancestry before attempting another mutation:

- Exact intended commit is the tip: verify bytes and complete the acknowledgment.
- Tip is a descendant: verify the intended commit and reachability; distinguish historical preservation from current integrated content. Do not reset the tip.
- Branch remains at old baseline, but the intended commit exists: verify it, recheck freshness, then consider the same non-force ref move. A separate create-commit retry may be unnecessary.
- New unrelated tip: rebuild on it with the same semantic bundle/projection ID and a new attempt ID, then rerun all gates. Do not stamp a stale tree onto the new parent.
- Incomplete/denied/unavailable reads: keep the outcome unknown and pause dependent writes. Timeouts are not proof of absence.

Deterministic semantic IDs plus exact manifest checks make replay idempotent at the result level. They do not make network side effects exactly once. Rebase attempts can produce multiple commits with the same semantic bundle, and abandoned immutable objects can exist. Deduplicate by exact semantic ID and content, not by counting commits or accepting any matching message.

Partial success cases:

- Private verified, public blocked: custody is complete at private; replication is pending with its reason. Resume only the unresolved public operation.
- Public verified, private unavailable or receipt missing: complete public custody stands if its entire recovery closure is available. Continue/reconcile only the private operation; do not wait for private success or remove the public copy to make statuses agree.
- Both verified, deployment pending/failed: repository preservation is complete; deployment remains separate. Diagnose/retry only within authorized deployment scope.
- Workspace lost: recover reachable manifests and pending intents from immutable remote evidence into a new staging location. Rehash the required dependency closure; never assume the original absolute directory still exists.

## 8. Receipts without self-reference or write amplification

An intent record is included with each batch. It records the semantic bundle/projection IDs, hashes and modes, policy/guard version, allowed target, source lineage, and which downstream actions remain pending. It cannot contain its own final commit hash or claim future readback succeeded.

The outcome receipt records repository/ref, baseline commit/tree, candidate commit/tree, expected parent, intent digest, exact change list or immutable manifest identity, guard result, readback hashes, branch/ancestry observation, timestamps, retries, errors, and deployment evidence if applicable. Public receipts contain only public-safe identifiers. API request metadata must exclude authorization headers and secrets.

Store shareable outcome receipts with the eligible complete copies, preferably in the next useful batch; genuinely restricted diagnostic details stay only in an authorized restricted destination. If the work is ending, make one small receipt-only commit using the same publication checks and verify it. Do not recursively produce “receipt of receipt of receipt” commits: the last receipt’s reachability and hashes are directly reconstructible from Git, while the final verification observation can be returned in chat. Durable intent plus immutable remote state must allow recovery even if the final local outcome receipt disappears.

The verified receipt must specify whether evidence was freshly read, reconstructed after restart, or copied from a historical record. Avoid presenting an old successful readback as a fresh one. If verification evidence cannot be reconstructed, keep that qualification explicit.

## 9. Failure boundaries and acceptance tests

Before operational use, exercise these cases in a local test repository or approved test destination; this proposal has not run them:

1. Kill publication after each stage: local ready, blob creation, tree creation, commit creation, ref mutation, readback, receipt write. Resume by reconciliation without losing unrelated files or falsely acknowledging a local-only result.
2. Drop the response after a successful ref update. Assert no blind duplicate publication and recovery of the exact semantic ID.
3. Move the branch between fresh read and update. Require rejection/rebuild for the ordinary forward race; separately document external rewind limitations.
4. Supply patch-only/truncated trees, wrong `base_tree`, wrong parent, mode-only changes, unexpected removals, path collisions, missing dependencies, and changed bytes under the same digest. All must fail closed.
5. Feed a burst larger than queue capacity. Assert bounded memory/disk, explicit admission backpressure, preserved unique evidence, fair public progress, and no unbounded stream of chat payloads.
6. Lose the workspace after either destination succeeds while the other is unavailable. Recover the complete shareable bundle, work index, and unresolved replication intent using only the surviving eligible copy. Reject a public copy with private-only required dependencies as incomplete.
7. Insert private material in HTML comments, nested archives, source maps, manifests, and diagnostic fields. The public projection must exclude it before any public object upload.
8. Attempt to modify primary `index.html`, global configuration/assets, or a workflow during preview publication. Require rejection unless separately included in the reviewed authorized intent.
9. Make GitHub return rate limits, permission errors, and ambiguous network failures. Assert appropriate wait/stop/reconciliation behavior without an alternate-access workaround.
10. Let repository publication succeed but deployment serve stale bytes. Report the commit as preserved and the deployment as unverified; never call the site live based only on a ref update.

Operational success is measurable: all acknowledged bundles are remotely reconstructible; no unrelated tree changes; no private-to-public leakage; bounded bytes at risk under admitted load; low measured preservation latency; and independently accurate private, public, integration, and deployment status.

## 10. Minimal implementation order

1. Add the immutable bundle/descriptor contract and bounded root-owned queue around existing work. Keep the known guard and publication path.
2. Add small eligible public/private intake commits with independent state, exact readback acknowledgments, and restart reconciliation. Verify one-copy and two-copy outcomes with small fault fixtures before calling it reliable.
3. Preserve the shareable core at both destinations; use explicit narrower projections only when needed. Keep checkpoint additions in approved subtrees and the primary site unchanged.
4. Add integration/deployment status and final receipts. Measure and tune batching only after correctness holds.

Do not begin by rewriting archives, adding distributed locks, creating new accounts, increasing permissions, or building a new always-on service. The smallest robust improvement is to preserve each bounded useful result promptly through either eligible destination, then complete its second copy. Two repositories on one GitHub account provide repository-level redundancy, not account/provider independence. An occasional milestone bundle through the existing authorized Library/export route can add a recovery option without a mandatory third service or new credentials.
