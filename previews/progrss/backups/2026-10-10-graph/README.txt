ProgRSS archived graph-edition browser backup
Graph/default-corrected edition · metadata refreshed 2026-10-10 UTC

STATUS: Archived edition with completed bounded Chrome acceptance.
This is older than the live storage-status edition as of 2026-10-10.
Archived graph commit: 3e02234970edc85873524bfc37a9e2c709e40373
Later live commit: 12050b663d7f3a4e7bcff6b1f4328d0a3b312399
The newer runtime and its fixes are not included in this ZIP.

Open index.html in a browser to use this browser-only edition. Try synthetic
data first. Browser storage and cross-origin fetching depend on the browser
and origin. Moving the file does not carry its saved browser data with it.

This ZIP backs up the program, not your observations. To preserve your own
library, use Field notes → Private backup in ProgRSS, and keep that separate
backup private.

Contents
- index.html: exact reviewed graph-edition bytes, unchanged by packaging.
- LICENSE-NOTICE.txt: license status and limits; no new license is assigned.
- notices/HISTORICAL-LICENSE-NOTICE.md: one exact carried historical notice.
- PROVENANCE.json: source identity, lineage and acceptance status.
- SHA256SUMS: hashes of the five payload files, excluding the manifest itself.

Provenance
Derived from the supplied ProgRSS rev0021 carrier. This is an unnumbered
browser-only edition, not a new full HTML/Bash carrier release. Its graph
corrections cover local collection capacity, refreshed reading decisions,
preservation of page baselines on renaming, and the existing-Lens threshold
default.

Exact index.html SHA-256:
43d7a88edbdd50568f0f22fbd6b881048e8d91e0756bd39c0eb0c339aaa060d2

Verify after extraction with “sha256sum -c SHA256SUMS” where available, or
compare these files with any SHA-256 utility. The ZIP has a separate outer
checksum beside it. Hashes detect changed bytes; they do not establish
ownership, authorship, safety or licensing rights.

Acceptance and limits
Local checks reported 69 supplied tests passing on the full carrier, plus
158 focused scenarios passing on each full/preview artifact, with 21
executable script blocks checked in each. Synthetic handlers use mocks.
Bounded Chrome checks used synthetic libraries and confirmed:
- Renaming a 200-card Lens preserved its page snapshot and observations;
  changing its selector removed only that Lens's snapshot.
- A 200-source/200-collection library allowed a collection rename that
  persisted after reload, while exported source definitions stayed exact.
- A missing existing threshold opened as 1, explicit 0 stayed 0, and a new
  Lens defaulted to 24; a changed page region produced one observation.
- A selected reading decision refreshed after an exclusion edit.
- Restoring, reloading and exporting the original synthetic library
  returned an exact baseline state.

This was one Chrome environment, not complete compatibility, accessibility,
storage-durability or security acceptance. A reverse-filter save timed out;
the unsaved edit was cancelled and its prior state checked. Horizontal
trace-panel overflow was observed at a roughly 1165 × 750 viewport.
Packaging rechecks archive integrity and exact bytes only.

Three reference-deletion policy findings remain unresolved. The optional
feed-ID export guard, newer storage changes and specialized restore-cache
work are not included. Long feed IDs can still collapse on ProgRSS reimport.
A file read begun in a Paste dialog can finish after restoring a different
library and reach replacement state; wait for any Paste file read to finish
before restoring. An earlier browser feed-download byte-readback question
remains unresolved; neither these Chrome trials nor ZIP verification
resolve it.

Excluded
The original carrier's Bash/Python companion, embedded tests, historical
instructions/documents and dormant donor archive are omitted. No browser
vault, captured private observations, credentials, private backup, internal
review logs or unrelated project material is included. Existing runtime
code can still create local browser data when you use the app.
WebRTC development and live peer use are deferred; connection setup is
hidden, while some legacy send controls remain in this archived edition.
Peer behavior is not covered by this package's acceptance.
