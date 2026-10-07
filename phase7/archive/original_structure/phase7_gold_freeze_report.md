# Phase 7 — Tier-A gold freeze report

_Generated 2026-10-03T00:52:36Z by `scripts/phase7_label_freeze.py --kind gold` (git `20ade33`)._

## STATUS: **NOT_AVAILABLE**

no interpreter-A form in data/labels/gold/phase4/raw_interpreter_A: no human Tier-A gold labels exist; nothing ingested or frozen.

No label was created, inferred or filled. The confirmatory experiments that need this label set stay DO NOT RUN.

## What exists (unchanged, hash-verified)

| item | SHA-256 |
|---|---|
| key (evaluator only) | `42aaf33cfcdd9f836d606c3d0d9f27379222b15d05d09780a49ce6616f65b95e` |
| design | `acc8692a3203944d7d791802b00c799dba75d82768db68d71a4c642608b48dd6` |
| kit gold_A | `0ce97841771e3c8e44350a87eb6f8c24b0b182f41261bddff3290155ebdb8223` |
| kit gold_B | `b26d25c2a62393f47842e3ad41f09d12acfc22508439c92d3c22b8fc59877669` |

## Procedure when the interpreters return the forms

1. Interpreter A's form → `data/labels/gold/phase4/raw_interpreter_A/`, B's → `raw_interpreter_B/`, each with its `.meta.json` (template in the kit).
2. `python3 src/validation/ingest_gold.py freeze pass1 --by <evaluator>` (SHA-256 of the raw files; immutable).
3. Disagreements on double points with a definite class → blind adjudication file in `adjudicated/`, then freeze it as a new pass.
4. `python3 scripts/phase7_label_freeze.py --kind gold --frozen-by <evaluator>` → this report, the verification list and `results/phase7/manifests/gold/manifest_v1.json`.
5. Genuine recording errors after freezing: only a signed correction record (`metadata/correction_<role>_<pass>.json`); a re-frozen raw file without it is rejected (RAW_REFROZEN).
