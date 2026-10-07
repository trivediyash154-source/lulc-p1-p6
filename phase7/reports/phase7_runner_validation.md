# Phase 7 — confirmatory runner validation

_Generated 2026-10-03T01:22:45Z; git `73e2ebc`._

## STATUS: runner **implemented and registered** (v2, 2026-10-03T01:19:35Z, before any label: True) for **P4-C1@v2 and P4-C2@v2**; P4-C3..C6@v2 are refused (NOT_IMPLEMENTED) until their specification is adopted.

Restoration note: the runner was first registered on 2026-10-02T19:20:41Z in the original repository; that registration was lost with the reclaimed container. The code was re-applied from the session transcript (every patch matched its original context) and re-registered here, still before any label exists.

Tests: `14 passed, 1 skipped, 2 warnings in 66.43s (0:01:06)` (runner, label freeze, district silver; synthetic data only).

## Requirement → implementation → test

| requirement (brief Stage 8) | how it is enforced | code | test |
|---|---|---|---|
| frozen protocol | protocol code/YAML hash + version = frozen = record; frozen `protocol.Run` re-verifies | guards.preflight; Run(eid) | test_preflight_refuses_every_record_today |
| correct experiment IDs + order C1 -> C6 | record must be PENDING; earlier records must have run; C2-C6 need the C1 selection | guards.preflight (order_*, selection) | preflight output |
| pre-run checklist never overridden | `assert_runnable` logic of `scripts/phase5_prerun_checklists.py` must say RUN | guards.preflight (prerun_checklist) | preflight output |
| adopted + countersigned specification | record + ADOPTED addenda, SHA in the researcher countersignature, append-only record pointer | spec.status | test_specification_requires_adoption_countersignature_and_pointer |
| code changes after registration | SHA-256 of every runner file = registered version (amendments only with a recorded reason) | guards.preflight (implementation_registration) | test_registration_and_single_execution_guards |
| training / tuning / evaluation partitions + leakage | silver rows inside the T1 train population; no training row on a gold or tuning cell; >= 2 km from every gold point -> STOP (Leakage) | data.leakage_check | end-to-end test (two forced leaks raise) |
| no gold-based selection | tuning selection (T1 tuning split) written + hashed BEFORE any gold label is read; predictions stored without labels | experiments.run_c1 | end-to-end test (gold reader asserts selection.json exists) |
| D1 arm definitions | A, B (P3-B4m PELT), C (GLC 2020 & 2021 = silver), D (T1 train 2020/2000), E = D + C; caps 5000; equal-size control | data.Arms | end-to-end test (D rows: train split, 2020/2000 only) |
| D2 correction enforcement (C2) | fail closed unless c2_tm_pif_v1 SHA pinned, fit report + spec agree, no config shadowing, TM coefficients = fit, L7/8/9 = comparator, 44 products carry the SHA, family validated, treatment cube TM years from landsat_c2tm with unchanged sources | guards.verify_c2_transform | test_c2_transform_chain_passes_only_when_every_link_holds |
| feature sets / models / seeds | B4t (RF), B3 5-year sequence (TempCNN P3-F1); RF 300/2/sqrt; 5 protocol seeds | models, experiments | end-to-end test |
| registered metrics + tests | design-weighted OA (= Olofsson estimator) and macro-F1 incl. outside-legend errors; seed-pooled paired stratified block bootstrap (n 2000, seed 20261110) | analysis | test_design_weighted_metrics_equal_frozen_functions, test_paired_test_identical_maps_and_decision_rule |
| multiple-comparison correction | BH q 0.05 over the whole family at once (untestable p = 1); family files immutable | analysis.apply_bh, runner.finalise_family | test_bh_untestable..., test_runner_refuses...family_finalisation |
| artifact hashing + immutable manifests | SHA-256 of every output; run manifest written once; run directory made read-only; one execution per record | runner.write_manifest/_freeze_dir; guards (single_execution) | test_registration_and_single_execution_guards |

## Specification status (fail closed until adopted)

The records + protocol v2 + D1/D2 leave execution details open (protocol ambiguities). They are written as PROPOSED addenda in `docs/phase7_runner_specification.md` (pre-data). Two need a genuine researcher choice: **C3-1** (era-specific normalisation vs era-specific models) and **C6-1** (RF vs XGBoost).

| record | specification | missing |
|---|---|---|
| P4-C1@v2 | NOT adopted | P4-C1_at_v2_addendum_2.json: not adopted (only a _PROPOSED file or nothing exists) |
| P4-C2@v2 | NOT adopted | P4-C2_at_v2_addendum_2.json: not adopted (only a _PROPOSED file or nothing exists) |
| P4-C3@v2 | NOT adopted | P4-C3_at_v2_addendum_1.json: not adopted (only a _PROPOSED file or nothing exists) |
| P4-C4@v2 | NOT adopted | P4-C4_at_v2_addendum_1.json: not adopted (only a _PROPOSED file or nothing exists) |
| P4-C5@v2 | NOT adopted | P4-C5_at_v2_addendum_1.json: not adopted (only a _PROPOSED file or nothing exists) |
| P4-C6@v2 | NOT adopted | P4-C6_at_v2_addendum_1.json: not adopted (only a _PROPOSED file or nothing exists) |

## Limitations (honest)

* Validated on a synthetic world only (36 x 30 cells, 2 seeds, 40 bootstrap resamples). Production scale (5 seeds, 2000 resamples, about 80 000 rows per arm) is untested; expected several hours on the Route-A machine.
* P4-C3..C6@v2 pipelines are not implemented: implementing them before their open choices are adopted would decide those choices in code.
* The district built share in C2 (reported, not decisive) is estimated on a systematic 1/10 x 1/10 cell sample.
* P4-C2@v2 is completed in the registry with outcome 'NA' and its final outcome is written by the family finalisation (BH over C2 + C3, G-1) as an append-only `family_decision` pointer; a record that fails enters the family BH with p = 1 for every planned test.
* Design-weighted OA re-normalises over the strata present in the accuracy set (w = N_h / k_h); it equals the frozen stratified estimator when every stratum has at least one accuracy-set label (unit-tested), and differs only when a stratum has none.
* Registration covers the runner package and every repository module it imports for features, models, change points, harmonisation and the checklist; pre-flight refuses a run on a tree with modified tracked files and checks every input's schema before the single execution is consumed.

## Pre-flight today

| record | decision | failed preconditions |
|---|---|---|
| P4-C1@v2 | DO NOT RUN | prerun_checklist, specification, gold, T1, input_glc_fcs30d, cube |
| P4-C2@v2 | DO NOT RUN | prerun_checklist, specification, order_P4-C1@v2, selection, gold, T1, input_glc_fcs30d, cube, c2_transform_chain |
| P4-C3@v2 | DO NOT RUN | prerun_checklist, specification, implementation, order_P4-C1@v2, order_P4-C2@v2, selection, gold, T1, input_glc_fcs30d, cube |
| P4-C4@v2 | DO NOT RUN | prerun_checklist, specification, implementation, order_P4-C1@v2, order_P4-C2@v2, order_P4-C3@v2, selection, gold, T1, input_glc_fcs30d, cube |
| P4-C5@v2 | DO NOT RUN | prerun_checklist, specification, implementation, order_P4-C1@v2, order_P4-C2@v2, order_P4-C3@v2, order_P4-C4@v2, selection, gold, T1, input_glc_fcs30d, cube |
| P4-C6@v2 | DO NOT RUN | prerun_checklist, specification, implementation, order_P4-C1@v2, order_P4-C2@v2, order_P4-C3@v2, order_P4-C4@v2, order_P4-C5@v2, selection, gold, T1, input_glc_fcs30d, cube |
