# Phase 6 — T1 human training labels for P4-C1@v2 (arms D and E; selection reference)

**Status: designed and kitted; 0 labels.** T1 v2 implements the adopted addendum (`experiments/registry4/addenda/P4-C1_at_v2_addendum_1.json`, decision D1). T1 labels are **training and selection data, never evaluation data**: they live outside the validation blocks and never overlap the gold.

## 1. Design (`data/labels/training/phase6_T1v2/`, seed 20261221, `scripts/phase6_training_sample_T1v2.py`)

| item | value |
|---|---|
| points | 699: **525 TRAIN**, **174 TUNING**; 140 double-interpretation (20 %) |
| TRAIN population | training-eligible cells (outside validation blocks and ≥ 2 km from them) **and** ≥ 2 km from every tuning cell (`sampling_design/train_eligible_T1_30m.tif`). Minimum observed distance to a tuning cell: 2 010 m (to the non-eligible remainder of a tuning 6 km block: 1 860 m; erratum-1) |
| TUNING population | training-eligible cells inside tuning blocks (`sha256('phase5-tuning-v1:{bx}:{by}') % 100 < 25` on training-share 6 km blocks) = `sampling_design/tuning_blocks_30m.tif` (committed; the T1 script refuses to overwrite the pinned design) |
| separation from gold | 0 points in validation blocks; minimum distance to any gold point 2 192 m (leakage audit in `design.json`) |
| stratification | Phase-4 strata × {class boundary, interior}; boundary share 0.511. Boundary cells are deliberately included because silver labels lack them |
| class balance | √area allocation with ×1.6 for hard strata. Floors: water-permanent 40 train / 15 tuning; urban core 30 / 10. The floors were added after a first draw was checked against the GLC 2020 proxy (design information only), before any label existed |
| class-balance proxy | GLC_FCS30D 2020 at the points — **a design check, never a label**: train agriculture 300, natural vegetation 92, built 78, water 36, other 19; tuning 100 / 28 / 26 / 10 / 9 (+1 bare) |
| machine-checkable IDs | `T2-0001…T2-0699` (the key holds row/col, UTM, stratum, boundary, split, distance to the nearest tuning block) |
| supersedes | T1 v1 (`data/labels/training/phase5_T1`, 635 points, no train/tuning separation). It was never interpreted; its kits must not be used |

## 2. Kits

| kit | contents |
|---|---|
| `phase6_T1v2_kit_INTERPRETER_A.zip` | 699 cells: form + KML |
| `phase6_T1v2_kit_INTERPRETER_B.zip` | 140 cells: form + KML |

Each kit also holds `INTERPRETER_PROTOCOL.md` (the same protocol as gold) and a metadata template. Hashes are in `results/phase6/kit_manifest.json`. Interpreters never see the split, strata or boundary flag.

Recommended: T1 interpreters should preferably be different people from the gold interpreters, and must at least work on T1 and gold in separate sessions, keeping the two kits apart.

## 3. Epochs and use in P4-C1@v2 (fixed by the addendum)

* **Before T1 interpretation starts**, the researcher countersigns D1 (`docs/templates/phase6_countersignature_TEMPLATE.json` → `experiments/registry4/addenda/phase6_countersignature.json`).
* **Interpret 2020 and 2000.** C1 uses only these two epochs for arms D and E. 2010 and 1990 are optional and are not used by C1.
* **Training labels.** Only TRAIN-split labels of the 4 legend classes (built_up, agriculture, natural_vegetation, water) are used. bare_sparse, other, uncertain, ambiguous and unavailable are not training labels.
* **Selection reference.** TUNING-split labels (2020 + 2000) are the **common** selection reference for every arm and model: highest seed-mean macro-F1. Silver labels in tuning blocks are never used for selection.
* **Never evaluation.** T1 labels never enter any gold evaluation, calibration on gold, or gate decision.

## 4. Freeze, ingest, audit (same tested code; training-leakage rules)

```bash
python3 src/validation/ingest_gold.py --kind training freeze <pass_id> --by <evaluator>
python3 src/validation/ingest_gold.py --kind training ingest
python3 src/validation/audit_gold.py --t1          # -> results/phase5/t1/t1_audit.json; the P4-C1@v2 checklist requires ingest PASS and >= 80 % labelled in 2020 and 2000 in BOTH splits
```

Training-mode checks:

* every point is training-eligible, outside validation blocks and ≥ 2 km from every gold point;
* TUNING points lie inside tuning blocks;
* TRAIN points are ≥ 2 km from tuning blocks;
* if the tuning raster is missing, ingestion FAILS (`TUNING_PARTITION_UNCHECKABLE`); the check is never skipped;
* all provenance, freeze, correction, value and adjudication rules of the gold pipeline also apply.

## 5. Known limitations

* The tuning metric rests on about 10 water and 26 built-up tuning points per epoch (proxy), so selection between near-equal arms is noisy. This affects only which arm is carried forward, never the gold evaluation of C1 itself.
* Interpreter effort for 2020 + 2000 (planning estimate, not a result): 699 × 2 (A) + 140 × 2 (B) = 1 678 interpretations, 28-56 h.
