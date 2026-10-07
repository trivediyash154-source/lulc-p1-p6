# Phase 7 — P4-C5@v2 report

_Generated 2026-10-03T01:22:45Z; git `73e2ebc`._

## STATUS: **DO NOT RUN** — not executed; no model was trained and no metric exists.

## Registered (frozen, unchanged)

| field | value |
|---|---|
| hypothesis | on gold, calibration fitted on non-target regions reaches ECE <= 0.05 and 90 % conformal coverage >= 0.85 in every region and era; abstaining on the least confident 20 % raises accuracy (Gate G7) |
| comparator | uncalibrated / no abstention |
| primary_metric | ECE, coverage, accuracy at 80 % coverage |
| direction | threshold/improvement |
| threshold | G7 of the protocol |
| family | uncertainty |
| features | selected per protocol v2 selection rule: P4-C1@v2 arm/model with the highest seed-mean macro-F1 on the TUNING split of the training-label set (ties -> simpler); validation-block gold never consulted |
| model | selected per protocol v2 selection rule: P4-C1@v2 arm/model with the highest seed-mean macro-F1 on the TUNING split of the training-label set (ties -> simpler); validation-block gold never consulted |
| evaluation_set | Tier-A district gold, design v2 (data/labels/gold/phase4/_key/phase4_gold_key.csv) |
| seeds | [20261201, 20261202, 20261203, 20261204, 20261205] |

Decision rule (protocol v2): SUPPORTED only if the pooled paired CI excludes 0 in the hypothesised direction, the FDR-adjusted p <= 0.05 and >= 4 of 5 seeds agree in sign

## Why it did not run

* Pre-run checklist: **DO NOT RUN** — failed: tier_A_gold_ingested, G2_passed_for_2020_and_2000, gold_labelled_epoch_2020, gold_labelled_epoch_2000, district_cube_validated_for_registered_years_and_periods, registered_feature_set_on_district_grid, selection_prerequisite_P4-C1@v2_completed.
* Runner pre-flight: **DO NOT RUN** — failed preconditions: prerun_checklist, specification, implementation, order_P4-C1@v2, order_P4-C2@v2, order_P4-C3@v2, order_P4-C4@v2, selection, gold, T1, input_glc_fcs30d, cube.
* Operational specification: not adopted (PROPOSED in docs/phase7_runner_specification.md); implemented: False.

## Reporting template (filled only by a run)

exact commit · configuration hash · dataset hashes (gold, T1, silver, cube, S1, D2 correction) · model hashes · seed list · metrics · CIs · tests · BH · missing data · failures · interpretation limited to the registered hypothesis — all written by the runner into `results/phase7/runs/P4-C5_at_v2/run_manifest.json`.

Machine-readable: `results/phase7/manifests/P4-C5/manifest.json`.
