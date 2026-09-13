# Measurement follow-up evidence

PR #294, implementation `c520ac78067a79a6cf4b99241dfb992bb43c03d2`.
Generated with `node scripts/corpus.js verify` followed by
`node scripts/fp-compare.js --out <new-directory>` on the exact commit.
All 38 cached sources matched their recorded manifest hashes. Corpus text is
not included; the artifacts contain source identities, spans, hashes, and
measurement diagnostics. Decompress the two `.gz` files and verify against
`SHA256.json` before comparing them.

Compared with the [#290 evidence](https://github.com/conorbronsdon/avoid-ai-writing/tree/39accce14131723c796ee640d88c3fe1b0223815/corpus/reports/fp-preprocessing-86ef5ab3),
both preprocessing paths retain the same scored counts, overall rates, AUCs,
category totals, and source/register/model results. This is a measurement and
diagnostic repair; it does not establish better authorship classification.

The paragraph comparison pairs 62 of 70 heading attachments with their unique
legacy bodies. Twenty paired bodies were selected previously; eight pairs
change score and three change category types. The eight ambiguous/unmatched
attachments remain additions/removals. All 62 absorbed skipped-heading
relationships are retained in `changes.absorbedHeadings`, outside capped
examples. No immutable unit IDs are rewritten to achieve the pairing.

Paragraph changes: 51 added, 277 removed, 3,528 modified. Impact counts:
266 population, 39 detector, 483 normalization, 62 segmentation, 3,006 metadata.
One additional below-minimum skipped Franklin footnote block is now visible
after repairing ordinal continuation; it changes no scored result. Current
paragraph accounting is 1,683 selected / 1,896 skipped; document accounting is
905 selected / 1 skipped. Scored counts differ from selected counts when the
detector rejects a prepared unit; both are retained in the summary.

Validation: independent GPT-5.6 Sol high implementation review, 21 preprocessing,
10 measurement, and 14 comparison tests; final commit CI checks all green.
The preceding scaling-only amendment was checked on 46,240 generated inputs
against its predecessor with identical preparation results. Its forward pass
replaces repeated backward scanning of long indented prose.

MiMo and Ling reviewed the production files at `36c5bdf5`, then the final
amendment at `c520ac78`; both final amendment reviews found no concrete bug.
These were static reviews, not independent provider-executed tests. The Sol
reviewer and coordinating Codex session reproduced and adjudicated their
earlier objections: the absorption lookup addresses skipped headings, eligible
long headings remain selected, colon inference is a documented heuristic, and
whitespace folding does not inflate token counts. Qodo's capped-mapping finding
was fixed. Fingerprint overlap is intentional and its exact two-file scope is
documented. `review-receipts.json` records provider/model/session/cost details.
An attempted Nemotron review produced only simulated tool markup and is not
counted as a completed review.
