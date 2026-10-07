# Phase 7 — P4-C2@v2 report

_Generated 2026-10-03T01:22:45Z; git `73e2ebc`._

## STATUS: **DO NOT RUN** — not executed; no model was trained and no metric exists.

## Registered (frozen, unchanged)

| field | value |
|---|---|
| hypothesis | replacing the identity TM transform with a cross-sensor TM/ETM+->OLI transform (published coefficients or PIF-fitted on non-validation blocks) reduces early-era false built-up and improves 1990/2000 gold accuracy |
| comparator | current harmonisation local_pune_g30_v2 (L5 identity) |
| primary_metric | gold OA 1990/2000 and district built share vs envelope of independent products |
| direction | improvement |
| threshold | protocol decision rule |
| family | historical |
| features | selected per protocol v2 selection rule: P4-C1@v2 arm/model with the highest seed-mean macro-F1 on the TUNING split of the training-label set (ties -> simpler); validation-block gold never consulted |
| model | selected per protocol v2 selection rule: P4-C1@v2 arm/model with the highest seed-mean macro-F1 on the TUNING split of the training-label set (ties -> simpler); validation-block gold never consulted |
| evaluation_set | Tier-A district gold, design v2 (data/labels/gold/phase4/_key/phase4_gold_key.csv) |
| seeds | [20261201, 20261202, 20261203, 20261204, 20261205] |

Decision rule (protocol v2): SUPPORTED only if the pooled paired CI excludes 0 in the hypothesised direction, the FDR-adjusted p <= 0.05 and >= 4 of 5 seeds agree in sign

## Why it did not run

* Pre-run checklist: **DO NOT RUN** — failed: tier_A_gold_ingested, G2_passed_for_2020_and_2000, gold_labelled_epoch_1990, gold_labelled_epoch_2000, district_cube_validated_for_registered_years_and_periods, registered_feature_set_on_district_grid, selection_prerequisite_P4-C1@v2_completed, c2_treatment_family_built, c2_treatment_feature_cube_validated.
* Runner pre-flight: **DO NOT RUN** — failed preconditions: prerun_checklist, specification, order_P4-C1@v2, selection, gold, T1, input_glc_fcs30d, cube, c2_transform_chain.
* Operational specification: not adopted (PROPOSED in docs/phase7_runner_specification.md); implemented: True.

## Reporting template (filled only by a run)

exact commit · configuration hash · dataset hashes (gold, T1, silver, cube, S1, D2 correction) · model hashes · seed list · metrics · CIs · tests · BH · missing data · failures · interpretation limited to the registered hypothesis — all written by the runner into `results/phase7/runs/P4-C2_at_v2/run_manifest.json`.

Machine-readable: `results/phase7/manifests/P4-C2/manifest.json`.

## C2 correction chain today (fail closed)

* Transform file `results/harmonization/c2_tm_pif_v1.yaml` SHA-256 `782eaef3e95c9dbe4b79868d936fd1e96ff1aa681d93ad609b5463ddd119db47` (pinned `782eaef3e95c9dbe…`): OK; fit report/specification binding: OK; config shadowing: none; TM coefficients = fit report: OK; landsat-7/8/9 = comparator: OK.
* Not yet verifiable (not built): treatment_products, comparator_products, treatment_family_validation, treatment_cube, comparator_cube → C2 refused.
