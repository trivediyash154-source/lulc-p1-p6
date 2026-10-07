# Phase 7 — confirmatory runner specification (PROPOSED, pre-data)

_Written 2026-10-03T01:16:08Z by `scripts/phase7_write_runner_specs.py`. Data state: no Tier-A gold label, no T1 label, no district feature cube, no confirmatory model or result exists (docs/phase7_entry_audit.md)._

The six @v2 records, protocol v2 and the adopted D1/D2 addenda leave execution details open. Each open point is a **protocol ambiguity**: deciding it after gold exists would be a forking path. The proposals below are written before any label. They are **not adopted**: the runner refuses a record until its addendum is adopted and countersigned by the researcher. Nothing here changes protocol v2, a hypothesis, a comparator or the decision rule. Items that narrow or re-scope a registered metric or threshold are marked **DEVIATION** and need an explicit researcher decision (C2-2, C4-2, C5-2), as do the two open forks (C3-1, C6-1).

| record | proposal file | SHA-256 |
|---|---|---|
| P4-C1@v2 | `experiments/registry4/addenda/P4-C1_at_v2_addendum_2_PROPOSED.json` | `27b0353f26d3ab03…` |
| P4-C2@v2 | `experiments/registry4/addenda/P4-C2_at_v2_addendum_2_PROPOSED.json` | `befa15467b54f623…` |
| P4-C3@v2 | `experiments/registry4/addenda/P4-C3_at_v2_addendum_1_PROPOSED.json` | `84d08d6b08e1b99c…` |
| P4-C4@v2 | `experiments/registry4/addenda/P4-C4_at_v2_addendum_1_PROPOSED.json` | `caeb225318479306…` |
| P4-C5@v2 | `experiments/registry4/addenda/P4-C5_at_v2_addendum_1_PROPOSED.json` | `29f893b647141bc3…` |
| P4-C6@v2 | `experiments/registry4/addenda/P4-C6_at_v2_addendum_1_PROPOSED.json` | `7ce5d1be94a23e1d…` |

## Rules common to all records

* **G-1 family-level multiplicity.** BH (q = 0.05) is applied over ALL p-value tests of a family at once, after every record of that family has run. Until then a record's tests carry raw p-values and the outcome stays PENDING_FAMILY. Threshold-only criteria (G6 in C4, G7 in C5) have no p-value and do not enter BH.
* **G-2 pooled paired test over seeds.** statistic on a resample = (mean over the 5 seeds of the metric of the treatment) - (same for the comparator), on the SAME resampled gold units (evaluation4.stats.stratified_block_bootstrap: strata = gold stratum, PSU = 6 km block, n = 2000, seed 20261110). p = evaluation4.stats.bootstrap_p_two_sided of the resampled differences. Seed agreement: sign of the full-sample per-seed difference equals the sign of the pooled estimate for >= 4 seeds.
* **G-3 design-based metrics.** accuracy set = gold points with a definite reference class (in_accuracy_set); weights w = N_h / k_h with N_h = stratum population cells and k_h = accuracy-set points of stratum h in the (re)sample, recomputed on every bootstrap resample. OA = weighted accuracy (equal to the Olofsson stratified estimator, unit-tested against evaluation4.metrics.stratified_estimates); macro-F1 = evaluation4.metrics.summary with these weights over classes 1-6, i.e. bare_sparse / other references count as errors and enter the macro mean when present (protocol rules 'reference_outside_legend' and 'macro_rule'). Maps never abstain in primary metrics.
* **G-4 directional hypotheses.** two-sided 95 % CI and two-sided bootstrap p for every test; for direction 'improvement' SUPPORTED additionally needs the pooled estimate > 0 (CI entirely above 0); CI entirely below 0 is reported as NOT SUPPORTED (opposite direction).
* **G-5 outcome categories.** SUPPORTED / NOT_SUPPORTED / INCONCLUSIVE only (registry OUTCOMES). INCONCLUSIVE = evidence incomplete (e.g. an epoch NOT TESTABLE, a failed run) - never a softer 'partially supported'.

## P4-C1@v2

* **C1-1 label epochs of the silver arms**. Gap: addendum-1: A 'applied as the label for all epochs'.
  Proposal: A, B, C and the C part of E label the four gold epochs 2020, 2010, 2000, 1990 (features of that epoch). One model per (arm, model family, seed) is trained on the pooled epoch rows (<= 5000 cells per epoch and class, numpy default_rng(seed)) and predicts every evaluation epoch from that epoch's features.
  Alternatives: per-epoch models (4x more models; not what 'applied as the label for all epochs' describes).
* **C1-2 arm B change-free segments**. Gap: P3-B4m ran on an annual 1990-2026 series; the district cube holds the 18 registered years.
  Proposal: P3-B4m stable_range unchanged (changepoints.detect 'pelt_mean' on dry_ndvi and post_ndvi, breaks with |magnitude| >= 0.05; break year <= 2021 raises the lower bound, a later break lowers the upper bound) on the cube years with finite values; a cell carries its silver label at epoch e iff e lies in its stable range; then the same caps as A.
  Alternatives: none without new data.
* **C1-3 arm C agreement**. Gap: resolved by erratum-1.
  Proposal: GLC_FCS30D 2020 AND 2021 L1 == the arm-A silver class (data/products_ext/glc_fcs30d/glc_fcs30d_district_30m.tif; GLC L1 crosswalk of scripts/phase6_training_sample_T1v2.py).
* **C1-4 feature inputs**. Gap: record: 'B3 + 5-year window (B4) + terrain' for both model families.
  Proposal: Random Forest: historical_reconstruction.features 'B4t' = B3 at t-2..t+2 (80) + nine 5-year statistics + 12 terrain features (101), NaN kept (sklearn trees route missing values). TempCNN: the P3-F1 input = B3 sequence t-2..t+2 (5 x 16) standardised with training statistics, missing years 0-filled + observation-mask channels; terrain is not part of the sequence input (sequence model 'on the 5-year sequence', as registered and as P3-F1). Features are read from the validated district feature cube; years outside the archive (1988-1989) stay missing.
  Alternatives: terrain repeated as constant channels in the TempCNN (not registered, not the Phase-3 implementation).
* **C1-5 TempCNN early-stopping split**. Gap: '10 % block-held-out subset of the arm's own training labels'.
  Proposal: training rows whose 6 km block satisfies sha256('phase7-es-v1:{bx}:{by}') % 10 == 0 form the early-stopping set (never tuning, never gold); Adam lr 1e-3, batch 512, max 40 epochs, patience 6 (P3-F1 settings).
* **C1-6 tuning selection**. Gap: addendum-1 'highest seed-mean macro-F1 over present legend classes'.
  Proposal: T1 v2 TUNING-split points with a legend reference class (1-4) at 2020 and 2000, pooled; unweighted macro-F1 over present legend classes (T1 is a design sample for training, not an estimation sample); ties |diff| < 0.005 -> Random Forest before TempCNN, then arm order A, B, C, D, E. The selected (arm, model) is written to the C1 run manifest before any gold metric is computed.
  Alternatives: design-weighted tuning macro-F1.
* **C1-7 contrasts and family**. Gap: addendum-1 family of 64.
  Proposal: B-A, C-A, D-A, E-A x epochs 2020, 2010, 2000, 1990 x {OA, macro-F1} x {RF, TempCNN} = 64 paired tests (G-2, G-3). An epoch with < 100 accuracy-set gold labels is NOT TESTABLE: its tests are reported as such and enter BH with p = 1 (m stays 64; conservative, never dropped). Two-sided (record direction).
  Alternatives: BH over testable tests only (anti-conservative after seeing label counts).
* **C1-9 record outcome from the 64 tests**. Gap: the hypothesis is a conjunction ('C, D, E each differ from A; B differs from A'); no aggregation rule registered.
  Proposal: contrast X-A DIFFERS iff >= 1 of its 16 tests meets the decision rule after BH over the 64 (direction reported); it does NOT differ iff none does and all four of its epochs are testable; otherwise it is INCONCLUSIVE. Record outcome: SUPPORTED iff all four contrasts differ; NOT_SUPPORTED iff at least one contrast does not differ; otherwise INCONCLUSIVE. All 64 test results are reported.
  Alternatives: a majority of the 16 tests per contrast (stricter, not registered either).
* **C1-8 secondary equal-size control**. Gap: addendum-1 secondary.
  Proposal: A subsampled per class proportionally to the row count of D, 5 seeds, both model families; paired vs D (same procedure); reported, not part of the decision rule or BH.

## P4-C2@v2

* **C2-1 single-factor execution**. Gap: addendum-1 fixes the transform; execution details open.
  Proposal: the C1-selected arm and model (C1-6) with identical training rows (cells, epochs, labels), seeds and hyperparameters in both arms; the ONLY difference is the feature cube: comparator data/features/cube/pune_G30.zarr (standard family), treatment data/features/cube/pune_G30_landsat_c2tm.zarr (c2_tm_pif_v1, SHA-256 782eaef3...). Arm-B stable ranges, if B is selected, are computed once from the standard cube for both arms.
* **C2-2 decisive metrics** — **researcher choice required**. Gap: record: 'gold OA 1990/2000 and district built share vs envelope'; only the gold part is blind (record).
  Proposal: DECISIVE: two paired tests, treatment - comparator gold OA at 1990 and at 2000 (G-2, G-3, direction improvement G-4), in BH family 'historical' with C3 (G-1). SUPPORTED iff both tests meet the decision rule. REPORTED, NOT DECISIVE: gold built-up user's accuracy (false built-up) 1990/2000; district built share 1990/2000 vs the frozen envelope of independent products (both envelope variants, D1 correction 5); PIF regime differences.
  **DEVIATION:** the record's primary_metric lists 'gold OA 1990/2000 AND district built share vs envelope'; this proposal makes the envelope part reported-only because it is not blind (record: 'only the Tier-A gold part of its metrics is blind'). This changes the scope of a registered primary metric and needs an explicit researcher decision.
  Alternatives: envelope as a co-decisive criterion: SUPPORTED additionally requires the treatment's 1990 and 2000 district built shares inside the envelope.
* **C2-3 fail-closed transform verification**. Gap: -.
  Proposal: the runner refuses to start unless: the transform file SHA equals the pinned value, the fit report and the addendum agree, every landsat_c2tm product records harmonization c2_tm_pif_v1 and the pinned transform SHA, the family validation is ok and complete, the treatment feature cube is validated with every TM year from landsat_c2tm, and the comparator cube uses local_pune_g30_v2. The proof is written into the run manifest.

## P4-C3@v2

* **C3-1 OPEN FORK: which era-aware approach** — **researcher choice required**. Gap: record: 'era-specific normalisation OR era-specific models'.
  Proposal: RECOMMENDED: era-specific normalisation - every feature standardised per era with the median and IQR of all training-eligible district cells in that era's cube years (label-free), one model trained on the normalised features; comparator: the same model on raw features. Reason: era-specific models need training labels in every era, which the selected arm may lack (arm D has labels only in 2020 and 2000), so the alternative may be infeasible for the very eras the hypothesis concerns.
  Alternatives: era-specific models (one per era with label rows; requires a silver-based selected arm).
* **C3-2 eras and tests**. Gap: 'gold OA per era' in the early-historical and Landsat-dominant eras.
  Proposal: protocol eras: early_historical = gold epoch 1990; landsat_dominant = gold epochs 2000 and 2010 pooled (design-weighted). Two paired tests (era-aware - unified gold OA), direction improvement, BH family 'historical' with C2; SUPPORTED iff both meet the decision rule. Macro-F1 and calibration per era reported.

## P4-C4@v2

* **C4-1 leave-one-region-out design**. Gap: LORO and 'within-region' not operationalised.
  Proposal: regions = the CHIRPS rainfall zones of regions.json (wet_west, transition, dry_east). Held-out: model trained on the selected arm's rows outside the target region, evaluated on the target region's gold. Within: model trained on rows of all regions, evaluated on the same gold. Gap = within - held-out (seed means).
* **C4-2 threshold criteria** — **researcher choice required**. Gap: G6.
  Proposal: per region and epoch: held-out design-weighted macro-F1 (seed mean) 95 % CI lower bound (stratified block bootstrap on that region's gold) >= 0.70 AND gap point estimate <= 0.10. Primary epoch 2020: SUPPORTED iff EVERY region passes; a region with < 100 accuracy-set gold labels makes the outcome INCONCLUSIVE (never skipped); any failing region -> NOT_SUPPORTED. 2000 evaluated where a region has >= 100 accuracy-set labels and reported. No p-value, no BH (G-1).
  **DEVIATION:** G6 is evaluated at the primary epoch 2020 only; 2000 is reported (record years: '2020 (and 2000 where gold exists)'). Researcher to confirm whether 2000 should also be decisive.
  Alternatives: 2000 co-decisive; gap CI instead of point estimate.

## P4-C5@v2

* **C5-1 calibration and conformal fit**. Gap: 'calibration fitted on non-target regions'; protocol: calibration fitting uses tuning blocks only.
  Proposal: for each target region: temperature scaling (calibration.metrics.Temperature) and conformal thresholds (LAC primary, APS reported; alpha 0.10; calibration.conformal) fitted on T1 v2 TUNING labels (legend classes, 2020 + 2000) located OUTSIDE the target region; applied to the selected model's gold-point probabilities inside the target region.
  Alternatives: silver labels in tuning blocks (not human).
* **C5-2 criteria** — **researcher choice required**. Gap: G7.
  Proposal: per region x era (2020 = sentinel_dense, 2000 = landsat_dominant): ECE (15 equal-width bins, top label) <= 0.05 and LAC 90 % set coverage >= 0.85 (point estimates, CIs reported); abstention: accuracy at 80 % coverage minus accuracy at 100 % coverage, stratified block bootstrap CI entirely > 0 (pooled over regions per era). SUPPORTED iff all hold; any region x era with < 100 accuracy-set labels -> INCONCLUSIVE.
  **DEVIATION:** G7 says 'every region and era'; the record's years are '2020, 2000', so the eras early_historical (1990) and the 2010 epoch are not evaluated by P4-C5@v2 under this proposal. Researcher to confirm this reading (or add 1990/2010).
  Alternatives: evaluate all four gold epochs (eras early_historical, landsat_dominant x2, sentinel_dense).

## P4-C6@v2

* **C6-1 model** — **researcher choice required**. Gap: record: 'XGBoost / RF'.
  Proposal: Random Forest with the C1 settings (300 trees, min_samples_leaf 2, max_features sqrt) for both feature sets; XGBoost is not run (not in the D1-approved model set; running both would add an unregistered choice).
  Alternatives: XGBoost only, or both with BH.
* **C6-2 design**. Gap: 'C1 vs C4 sets', LORO 2020.
  Proposal: feature sets from historical_reconstruction.features: C1 = B3 (optical) vs C4 = B3 + S1F + terrain; labels = the C1-selected arm's rows at epoch 2020 only (S1 era); LORO over the three rainfall regions; every gold point (2020) predicted by the model trained without its region; one paired test (C4 - C1 design-weighted macro-F1, pooled over regions, G-2, G-3), direction improvement, BH family 'spatial' (G-1; C4 has no p-values).
  Alternatives: per-region tests (3 tests).

## Adoption

For each record: review, edit if needed, save without `_PROPOSED` with status `ADOPTED`, and countersign its SHA-256 in `experiments/registry4/addenda/phase7_countersignature.json` (template `docs/templates/phase7_countersignature_TEMPLATE.json`). Adoption must happen before the gold labels of the epochs the record uses are ingested; the runner records the adoption time against the gold freeze time.
