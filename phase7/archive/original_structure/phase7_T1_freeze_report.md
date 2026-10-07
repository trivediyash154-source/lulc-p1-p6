# Phase 7 — T1 v2 (training + tuning) freeze report

_Generated 2026-10-03T00:52:37Z by `scripts/phase7_label_freeze.py --kind training` (git `20ade33`)._

## STATUS: **NOT_AVAILABLE**

no interpreter-A form in data/labels/training/phase6_T1v2/raw_interpreter_A: no human T1 v2 (training + tuning) labels exist; nothing ingested or frozen.

No label was created, inferred or filled. The confirmatory experiments that need this label set stay DO NOT RUN.

## What exists (unchanged, hash-verified)

| item | SHA-256 |
|---|---|
| key (evaluator only) | `62868f8da968db6ba55595c1127630433097f3dde5a42960425d427548c955fd` |
| design | `4058193bc6da90d81c42604a4834c0ef049d3f61c33b91236adb955e1d7d953e` |
| kit T1v2_A | `a92eeff089a59e9003c79bed4cab964090f3a8d2979c3241df426cab80015ff3` |
| kit T1v2_B | `bf1ca352a1d7af442c64f9e0eae22f7f0ab1aa84e43de62ca3896b380bca7c22` |

## Procedure when the interpreters return the forms

1. Interpreter A's form → `data/labels/training/phase6_T1v2/raw_interpreter_A/`, B's → `raw_interpreter_B/`, each with its `.meta.json` (template in the kit).
2. `python3 src/validation/ingest_gold.py --kind training freeze pass1 --by <evaluator>` (SHA-256 of the raw files; immutable).
3. Disagreements on double points with a definite class → blind adjudication file in `adjudicated/`, then freeze it as a new pass.
4. `python3 scripts/phase7_label_freeze.py --kind training --frozen-by <evaluator>` → this report, the verification list and `results/phase7/manifests/T1/manifest_v1.json`.
5. Genuine recording errors after freezing: only a signed correction record (`metadata/correction_<role>_<pass>.json`); a re-frozen raw file without it is rejected (RAW_REFROZEN).
