# Preprocessing comparison evidence for PR #290

Measured candidate: `86ef5ab3e2f85199f7b2d6cd2231ba9ff91a49b6`.
Legacy preparation reference: `fabd62d9c8785dd0edda35201359bcc635b7d3de`.
Both paths use the candidate's unchanged detector and the same 38 verified corpus sources. No sources are missing. RAID's recorded deterministic sample contains 230 machine and 40 human rows; its 300-machine target was not reached, and the recorded selection/hash was retained. HC3 contains 300 paired records (600 rows).

The gzip files contain no corpus text. `summary.json.gz` includes all thresholds, Wilson intervals, source/register comparisons, category counts including zeros, fingerprints, selection/skip totals, and changed examples identified by source spans. `decisions.jsonl.gz` contains every decision from both paths in both modes. `sources.sha256` and `code.sha256` record the exact input and source-code fingerprints. This evidence branch is separate from the implementation PR so its candidate SHA stays fixed.

Reproduce from the measured candidate:

```bash
git checkout 86ef5ab3e2f85199f7b2d6cd2231ba9ff91a49b6
node scripts/corpus.js fetch
node scripts/corpus.js verify
node scripts/fp-compare.js --out /tmp/fp-comparison
```

Decompress the published files with `gzip -dk summary.json.gz decisions.jsonl.gz`.

SHA256 of the uncompressed outputs:

```text
580f30d92db491665a2c48f3ce1efce92f7ca6b2b8f15c2ba8bdd36c011bf8cf  summary.json
5e32f6dbfadd82fdfa7a41401442cf3a9ac9306863a019d6175e3b27774e2fe0  decisions.jsonl
```

At score >=3:

| Mode | Human units, legacy → current | Machine units, legacy → current | FPR | TPR |
|---|---:|---:|---:|---:|
| Paragraph | 889 → 891 | 779 → 792 | 8.324% → 8.305% | 15.276% → 15.909% |
| Document | 376 → 376 | 530 → 529 | 10.106% → 9.574% | 26.792% → 32.325% |

These changes reflect restored structure and some changes in the measured population; they do not establish general authorship-detection accuracy. Seven machine recipe paragraphs regain bullet-list findings. Real paragraph boundaries remove seven human Tier-2 clusters that existed only after whole-document flattening. One fully quoted machine document is now explicitly detector-too-short after quote masking. Line preservation also exposes a historical title-case false positive and two archived-page zero-width/footer artifacts; see the discussion on #288 for diagnostics and follow-up scope.

This amended candidate also preserves detector-recognized tab-indented fences and constructs default measurement rows from the exact verified source snapshot. All metric and decision data are identical to the preceding f1dfd940 evidence; only candidate/code fingerprints changed.
