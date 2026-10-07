# Phase 6 — execution status

_Generated 2026-10-02T18:19:02Z by `scripts/write_phase6_docs.py` from saved files; git 48fbfca._

Protocol v2 unchanged: code `3bb40f9c982d`, YAML `fe1fad6f8b8e` (frozen values match: True); `check_confirmatory_records()` ok = True. **No confirmatory experiment was run**: every pre-run checklist says DO NOT RUN, and no result was overridden.

## 1. Decisions

| decision | outcome | record |
|---|---|---|
| D1 (P4-C1@v2 arms, tuning split, T1) | **APPROVE WITH SPECIFIC CORRECTION** | `experiments/registry4/addenda/P4-C1_at_v2_addendum_1.json` (sha `db13cfae510b`); proposal kept unchanged |
| D2 (P4-C2@v2 cross-sensor transform) | **PIF-fitted TM → cube OLI-equivalent (RMA)**, fit feasible | `experiments/registry4/addenda/P4-C2_at_v2_addendum_1.json`; `results/harmonization/c2_tm_pif_v1.yaml` |
| D3 (full cube compute) | **Route A**, not executable in this sandbox | `docs/phase6_D3_compute_decision.md` |

## 2. Human labels

| set | design | labels | ingestion | gate |
|---|---|---|---|---|
| Tier-A gold | 641 points, 160 double | 0 | FAIL | G2 FAIL |
| T1 v2 (training/selection) | 699 points ({'train': 525, 'tuning': 174}) | 0 | FAIL | required by P4-C1@v2 |

Blind kits (form + role-specific KML + interpreter protocol + metadata template only): `phase6_gold_kit_INTERPRETER_A.zip` (641 cells), `phase6_gold_kit_INTERPRETER_B.zip` (160 cells), `phase6_T1v2_kit_INTERPRETER_A.zip` (699 cells), `phase6_T1v2_kit_INTERPRETER_B.zip` (140 cells).

## 3. District data foundation

| component | state | evidence |
|---|---|---|
| Landsat standard family (18 years × 4 periods) | PILOT: 4/72 products | `validate_district_cube.py --require min`: ok = True, complete_for_required = False |
| C2 treatment family (11 TM years × 4 periods) + treatment feature cube | not built | needs Route A; checked by `c2_treatment_family_built` (transform SHA pinned) and `c2_treatment_feature_cube_validated` |
| Sentinel-1 RTC 2020 | not built (writer tested equal to the engine; strip driver untested, needs Planetary Computer) | `docs/phase6_sentinel1_spec.md` |
| district terrain | not built: attempted here, **OOM_KILLED** (6 GB memory limit) → Route A; district mode implemented, window behaviour unchanged | `results/phase6/terrain_run_status.json` |
| feature cube `pune_G30.zarr` | not built (strip builder tested equal to the window builder) | `scripts/phase6_build_feature_cube.py`, `src/data/validate_feature_cube.py` |
| district silver (arm A) | not built; **no district builder yet** (window code `src/pune_eo/labels/silver.py`) | must write `data/labels/pune_G30_district_silver` + `results/phase6/district_silver_validation.json` (ok) |

## 3b. Code still to be written before P4-C1@v2 can run (engineering, no data needed)

* District silver builder + validator (arm A; strip-wise version of `labels/silver.py`).
* The confirmatory runner for P4-C1@v2: arms A-E, PELT extension (arm B), RF/TempCNN training, selection on the T1 tuning split, gold evaluation, BH. It must call `assert_runnable()` first. It can be written and unit-tested on synthetic data before any label exists.

## 4. Confirmatory records (pre-run checklists, never overridden)

| record | decision | failed items |
|---|---|---|
| P4-C1@v2 | DO NOT RUN | 11: tier_A_gold_ingested, G2_passed_for_2020_and_2000, gold_labelled_epoch_2020, gold_labelled_epoch_2010, gold_labelled_epoch_2000, gold_labelled_epoch_1990, district_cube_validated_for_registered_years_and_periods, registered_feature_set_on_district_grid, researcher_countersignature_D1, human_training_labels_T1_ingested, district_silver_arm_A_built |
| P4-C2@v2 | DO NOT RUN | 10: tier_A_gold_ingested, G2_passed_for_2020_and_2000, gold_labelled_epoch_1990, gold_labelled_epoch_2000, district_cube_validated_for_registered_years_and_periods, registered_feature_set_on_district_grid, selection_prerequisite_P4-C1@v2_completed, researcher_countersignature_D2, c2_treatment_family_built, c2_treatment_feature_cube_validated |
| P4-C3@v2 | DO NOT RUN | 9: tier_A_gold_ingested, G2_passed_for_2020_and_2000, gold_labelled_epoch_1990, gold_labelled_epoch_2000, gold_labelled_epoch_2010, gold_labelled_epoch_2020, district_cube_validated_for_registered_years_and_periods, registered_feature_set_on_district_grid, selection_prerequisite_P4-C1@v2_completed |
| P4-C4@v2 | DO NOT RUN | 6: tier_A_gold_ingested, G2_passed_for_2020_and_2000, gold_labelled_epoch_2020, district_cube_validated_for_registered_years_and_periods, registered_feature_set_on_district_grid, selection_prerequisite_P4-C1@v2_completed |
| P4-C5@v2 | DO NOT RUN | 7: tier_A_gold_ingested, G2_passed_for_2020_and_2000, gold_labelled_epoch_2020, gold_labelled_epoch_2000, district_cube_validated_for_registered_years_and_periods, registered_feature_set_on_district_grid, selection_prerequisite_P4-C1@v2_completed |
| P4-C6@v2 | DO NOT RUN | 7: tier_A_gold_ingested, G2_passed_for_2020_and_2000, gold_labelled_epoch_2020, district_cube_validated_for_registered_years_and_periods, registered_feature_set_on_district_grid, selection_prerequisite_P4-C1@v2_completed, sentinel1_rtc_2020_on_district_grid |

## 5. Not done, by rule

* No confirmatory run, no model trained for any P4 record, no historical maps, no district-wide inference, no scenarios.
* No label of any kind was produced: gold and T1 come only from human interpreters.
* Protocol v2 unchanged; records changed only by append-only `addenda` pointers (P4-C1@v2, P4-C2@v2).