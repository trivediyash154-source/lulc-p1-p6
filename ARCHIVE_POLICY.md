# ARCHIVE POLICY

## Preservation Rules

1. **Original files are never modified.** The `archive/original_structure/` directory in each phase contains an exact copy of the source material as received.
2. **Navigation layer is additive.** Files in `reports/`, `figures/`, `data/`, `protocols/` are copies organized for discovery — they do not replace the originals.
3. **Failed and rejected experiments are preserved.** The experiment registry keeps FAIL, INVALID, INCONCLUSIVE, and NOT_SUPPORTED records alongside PASS/SUPPORTED ones.
4. **Superseded versions are preserved.** Protocol v1, design v1, T1 v1 are kept and marked as superseded — never deleted.
5. **Inconsistencies between phases are documented, not resolved.** If Phase 2 says X and Phase 5 corrects it, both records are preserved.

## File Integrity

- All original-structure files have SHA256 hashes in `provenance/SHA256_MANIFEST.csv`
- The manifest includes the original relative path, repository path, file size, hash, and phase
- Duplicate files (identical SHA256) are marked in the manifest but not deleted

## What Must Not Be Changed

- Frozen protocol documents
- Experiment registry records
- Decision records (D1, D2, D3)
- Gold/T1 sample designs
- Frozen SHA256 hashes
- Historical research reports
