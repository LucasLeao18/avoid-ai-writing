# Preprocessing comparison evidence for PR #290

Measured candidate: `f1dfd940559ab8cf3041ec6465ac79a0ad0b9e72`.
Legacy preparation reference: `fabd62d9c8785dd0edda35201359bcc635b7d3de`.
Both paths use the candidate's unchanged detector and the same 38 verified corpus sources. No sources are missing. RAID's recorded deterministic sample contains 230 machine and 40 human rows; its 300-machine target was not reached, and the recorded selection/hash was retained. HC3 contains 300 paired records (600 rows).

The gzip files contain no corpus text. `summary.json.gz` includes all thresholds, Wilson intervals, source/register comparisons, category counts including zeros, fingerprints, selection/skip totals, and changed examples identified by source spans. `decisions.jsonl.gz` contains every decision from both paths in both modes. `sources.sha256` and `code.sha256` record the exact input and source-code fingerprints. This evidence branch is separate from the implementation PR so its candidate SHA stays fixed.

Reproduce from the measured candidate:

```bash
git checkout f1dfd940559ab8cf3041ec6465ac79a0ad0b9e72
node scripts/corpus.js fetch
node scripts/corpus.js verify
node scripts/fp-compare.js --out /tmp/fp-comparison
```

Decompress the published files with `gzip -dk summary.json.gz decisions.jsonl.gz`.

SHA256 of the uncompressed outputs:

```text
b9e700ac6fd169b9e50a224ac1e8bf79bff4c9e6a62441171482e6c3caaf8483  summary.json
5e32f6dbfadd82fdfa7a41401442cf3a9ac9306863a019d6175e3b27774e2fe0  decisions.jsonl
```

At score >=3:

| Mode | Human units, legacy → current | Machine units, legacy → current | FPR | TPR |
|---|---:|---:|---:|---:|
| Paragraph | 889 → 891 | 779 → 792 | 8.324% → 8.305% | 15.276% → 15.909% |
| Document | 376 → 376 | 530 → 529 | 10.106% → 9.574% | 26.792% → 32.325% |

These changes reflect restored structure and some changes in the measured population; they do not establish general authorship-detection accuracy. Seven machine recipe paragraphs regain bullet-list findings. Real paragraph boundaries remove seven human Tier-2 clusters that existed only after whole-document flattening. One fully quoted machine document is now explicitly detector-too-short after quote masking. Line preservation also exposes a historical title-case false positive and two archived-page zero-width/footer artifacts; see the discussion on #288 for diagnostics and follow-up scope.
