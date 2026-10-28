# A release carries the work and its update feed

The current public release has one complete HTML work, a byte-identical web alias, a small RSS projection, and an identity manifest. The HTML is sufficient to take the work elsewhere. RSS lets a subscriber follow it without repeatedly fetching the full carrier.

## Publishing a reviewed change

Use the HTML's carried canonical builder and existing `blog feeds` interface. The exporter reads accepted public writing; it does not upload anything or change the accepted workspace. Begin with `Lumen/OFFICER.md` after extracting an unchanged original into a new destination. Running software remains subject to the recipient's permissions.

Changes mode labels an initial collection without inventing publication dates. Later exports distinguish new-to-feed works from revisions. Unchanged exports preserve event identity; a revision gets a distinct event ID while its page URL remains stable. A feed reader may still choose its own presentation and notification behavior.

Compact summaries keep the feed small. Optional before/after excerpts show a bounded changed span and mark truncation. They are not a complete word-level edit history. Full-content export remains available when that reading experience is wanted. Removed or nonpublic works are excluded from the next projection; that cannot recall copies already fetched by readers.

The comparison receipt belongs in private release custody. It binds the public snapshots used to distinguish revisions and must be recovered and hash-verified before the next comparison. It is not a second writing database. If it is lost, a baseline reset must say what history is unavailable rather than inventing a chain of edits.

Publish the reviewed HTML, index alias, RSS and updated EDITION.json in one guarded Git commit. Preserve unrelated entries and modes, verify the intended tree and parent, recheck the branch, update without force, and read back exact immutable bytes. Verify the live HTML and feed separately after deployment. A successful local build, a successful branch update and a working public URL are different results.

## Recovering an edition

1. Select an immutable public commit and its EDITION.json, not a mixture of changing branch reads.
2. Retrieve and verify both the complete HTML and RSS against that manifest. Keep the originals unchanged.
3. Reopen the HTML's carried source using its documented interface. Generic custody can preserve the bytes without interpreting the carrier.
4. Recover the matching private comparison receipt through its owner's authorized records. Verify its identity and prior release association before classifying another change.
5. Reconcile any uncertain publication against the actual destination before retrying. Do not start a second publisher merely because another scheduled label arrived.

The companion's existing workshop inventory/checkpoint/reopening tools support selected-file custody. Its narrow Lumen.sh/checkpoint Git planner is not an HTML/RSS publisher. No new builder or schedule is needed for this pair.
