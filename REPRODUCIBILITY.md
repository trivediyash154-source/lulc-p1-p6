# REPRODUCIBILITY

## What Is Reproducible

1. **Compositing pipeline:** Given the same Landsat/S2/S1 scenes from Planetary Computer, the compositing engine produces bit-identical outputs (verified: district pilot vs window composites, 3,846,463 cell-epochs, max difference 0)
2. **Silver labels:** District silver rebuilt byte-identical (SHA-256 `32e1ddb0...`)
3. **Spatial context:** Validation blocks byte-identical to frozen SHA; training-eligible pixel-exact on T1 footprint
4. **Evaluation protocol:** Protocol v2 hashes unchanged; 15 unit tests; registry refuses non-conforming runs
5. **Phase 3 experiment registry:** 63 records, all kept (including FAIL/INVALID)

## What Is NOT Reproducible

1. **Original git history (Phases 1–6):** Lost when the session container was reclaimed; restored from Phase 6 snapshot
2. **Planetary Computer scene catalog:** Scene availability may change over time; the manifest pins the scene list
3. **Earth Engine products:** EE asset versions are opaque; not pinned
4. **AI/Claude conversation context:** The original Claude conversation is not preserved in this repository; findings are derived from the conversation but recorded in the research reports

## Verification Steps

1. Check SHA256 hashes: `provenance/SHA256_MANIFEST.csv`
2. Verify protocol v2 code/YAML hashes match frozen values
3. Re-run compositing on any window to verify bit-identical output
4. Re-run spatial context generation and verify block SHA
5. Re-run silver builder and verify byte-identical output
