# Phase 7 — P4-C1@v2 report

_Generated 2026-10-03T01:22:45Z; git `73e2ebc`._

## STATUS: **DO NOT RUN** — not executed; no model was trained and no metric exists.

## Registered (frozen, unchanged)

| field | value |
|---|---|
| hypothesis | training-label quality and temporal design change gold accuracy of historical maps: arms C (independently validated), D (human gold-training), E (gold + filtered silver) each differ from A (single-year silver); B (multi-year silver) differs from A |
| comparator | arm A (single-year silver 2020-21) with identical features, model and seeds |
| primary_metric | gold overall accuracy and macro-F1 (present classes) per epoch (2020, 2010, 2000, 1990) |
| direction | two-sided |
| threshold | decision rule of the protocol (paired CI excludes 0, BH q<=0.05 within family, >=4/5 seeds) |
| family | label_quality |
| features | Landsat seasonal B3 + 5-year window (B4) + terrain |
| model | Random Forest and TempCNN-class temporal model |
| evaluation_set | Tier-A district gold, design v2 (data/labels/gold/phase4/_key/phase4_gold_key.csv) |
| seeds | [20261201, 20261202, 20261203, 20261204, 20261205] |

Decision rule (protocol v2): SUPPORTED only if the pooled paired CI excludes 0 in the hypothesised direction, the FDR-adjusted p <= 0.05 and >= 4 of 5 seeds agree in sign

## Why it did not run

* Pre-run checklist: **DO NOT RUN** — failed: tier_A_gold_ingested, G2_passed_for_2020_and_2000, gold_labelled_epoch_2020, gold_labelled_epoch_2010, gold_labelled_epoch_2000, gold_labelled_epoch_1990, district_cube_validated_for_registered_years_and_periods, registered_feature_set_on_district_grid, human_training_labels_T1_ingested.
* Runner pre-flight: **DO NOT RUN** — failed preconditions: prerun_checklist, specification, gold, T1, input_glc_fcs30d, cube.
* Operational specification: not adopted (PROPOSED in docs/phase7_runner_specification.md); implemented: True.

## Reporting template (filled only by a run)

exact commit · configuration hash · dataset hashes (gold, T1, silver, cube, S1, D2 correction) · model hashes · seed list · metrics · CIs · tests · BH · missing data · failures · interpretation limited to the registered hypothesis — all written by the runner into `results/phase7/runs/P4-C1_at_v2/run_manifest.json`.

Machine-readable: `results/phase7/manifests/P4-C1/manifest.json`.
