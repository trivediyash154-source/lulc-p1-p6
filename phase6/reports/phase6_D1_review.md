# Phase 6 — decision D1: review of the P4-C1@v2 addendum

## VERDICT: **APPROVE WITH SPECIFIC CORRECTION**

The proposal resolves an under-specified implementation without changing the scientific question. As written, however, it contained one hidden model-selection advantage and several open degrees of freedom. Six corrections were made; a seventh (arm-C agreement years) and three clarifications were registered by erratum-1 after an independent verification.

| status | file | SHA-256 / note |
|---|---|---|
| adopted | `experiments/registry4/addenda/P4-C1_at_v2_addendum_1.json` | `db13cfae…` |
| original proposal | `experiments/registry4/addenda/P4-C1_at_v2_addendum_1_PROPOSED.json` | kept unchanged |
| erratum (clarification, after independent verification) | `experiments/registry4/addenda/P4-C1_at_v2_addendum_1_erratum_1.json` | arm C wording, correction 7, tuning-block definition, countersignature |

The P4-C1@v2 record received only an append-only `addenda` pointer; no field changed (verified in `git diff`). Protocol v2 is unchanged (code `3bb40f9c…`, YAML `fe1fad6f…`). No gold label, T1 label, district feature cube or model existed when this review was written.

## 1. What the registered record fixes, and what it leaves open

| element | P4-C1@v2 record | status before D1 |
|---|---|---|
| hypothesis | arms C, D and E each differ from A; B differs from A (two-sided) | fixed |
| comparator | arm A (single-year silver 2020-21), identical features, models and seeds | fixed |
| metrics | gold OA and macro-F1 (present classes) per epoch, 2020/2010/2000/1990 | fixed |
| features, models, seeds | B3 + 5-year window + terrain; RF and TempCNN-class; seeds 20261201-05 | fixed |
| arms C, D, E | named only ("independently validated", "human gold-training", "gold + filtered silver") | **open** |
| training sample, tuning split, sampling sizes, hyperparameters, BH family membership | not specified | **open** |

## 2. Review questions

| question | finding |
|---|---|
| Scientifically coherent with P4-C1@v2? | **Yes.** A, B, D and E follow directly from their names. For C, "independently validated" is operationalised as agreement with an independent product (GLC_FCS30D, Landsat-based, sharing no input with WorldCover/Esri), not as human verification. This is a legitimate reading of the record and must be reported as such. The two alternatives considered (C′ human-verified, C″ reliability-filtered) are listed in the proposal. |
| Introduces leakage? | **Not into gold.** T1 lies outside validation blocks: minimum 2 192 m to any gold point, 0 points in validation blocks. Silver arms are restricted to training-eligible cells. **Circularity found:** GLC_FCS30D is both a training filter for C/E and a Tier-B product in the G5 / P4-C2 built-up envelope. A model trained on GLC-filtered labels is no longer independent of GLC. → **Correction 5.** |
| Hidden model-selection pathway? | **Yes, in the proposal.** "Highest seed-mean macro-F1 on the TUNING split of the training-label set" would score each arm on its own tuning labels. Arms A, B and C would be scored on silver interior cells, which are easy by construction (edge share 7-25 % vs 65-72 %). Arm D would be scored on human labels that include boundaries, so selection would favour silver arms for reasons unrelated to gold accuracy. → **Correction 1.** TempCNN early stopping and free hyperparameters were a second, smaller pathway. → **Correction 3.** |
| Tuning split independent of final gold evaluation? | **Yes.** Tuning blocks are training-share blocks, all ≥ 2 km from validation blocks; no gold point is in a tuning block. In the proposal, however, training cells could sit directly next to tuning points. Labels are autocorrelated up to ~4 km, so densely sampled (silver) arms would gain tuning score. → **Correction 2.** |
| Arms operational enough to execute? | **No, in the proposal.** Sampling sizes, seeds, legend handling of T1 bare/other/abstentions, the label epochs of D/E ("2010, 1990 when interpreted" was open-ended), hyperparameters and BH family membership were all missing. → **Corrections 3, 4, 6.** |
| Changes the scientific question? | **No.** The question remains whether training-label composition changes gold accuracy. The corrections remove degrees of freedom; they add no hypothesis, contrast or metric. |

## 3. Corrections (now part of the adopted addendum)

1. **Common selection reference.** All arms are scored for selection on the same T1 v2 **TUNING-split human labels** (2020 and 2000). Silver labels in tuning blocks are never used for selection.
   * Criterion: highest seed-mean macro-F1 over present legend classes.
   * Ties (|Δ| < 0.005) go to fewer features / the simpler model (RF before TempCNN), then arm order A…E.
2. **Spatial train/tuning partition.** No arm trains inside tuning blocks, and every training cell or T1-train point is ≥ 2 km from every tuning block (the same separation used for validation).
   * Training area remaining: 27.2 % of the district, against 36.6 % without the buffer.
   * T1 was redrawn as **v2** with this partition; v1 was never interpreted and must not be used.
3. **Fixed hyperparameters.**
   * RF: 300 trees, `min_samples_leaf=2`, `max_features='sqrt'`, no class weights (the Phase-3 settings).
   * TempCNN (`pune_eo.agriculture.timeseries`): at most 40 epochs, early stopping (patience 6) on a 10 % block-held-out subset of the arm's **own** training labels. Never on tuning or gold labels.
4. **Sampling and labels.**
   * Silver arms: ≤ 5 000 cells per class and epoch, drawn with `default_rng(run seed)`.
   * D: all T1-train labels at **2020 and 2000 only**.
   * E: D ∪ C, unweighted.
   * Only the 4 legend classes are used for training. T1 bare_sparse/other/abstentions are not training labels; gold bare_sparse/other still count as errors.
   * C requires GLC_FCS30D 2020 **and** 2021 to agree with the silver class.
5. **Envelope independence.** If the selected arm/model used GLC_FCS30D (C or E), the G5 / P4-C2 built-share envelope is computed from WSF, GHSL and GISA only. Their per-product shares are already frozen in `results/phase5/benchmark/g5_built_envelope_frozen.json`. Both envelopes are reported for every arm.
6. **BH family.** The family is 4 contrasts × 4 epochs × 2 metrics × 2 models = **64 tests**. An epoch with < 100 accuracy-set gold labels at freeze is reported as NOT TESTABLE; it is not dropped silently.
7. **Arm C agreement years** (listed by erratum-1; it was missing from `corrections_vs_proposal`). The proposal checked GLC agreement "at the label epoch". The adopted rule checks it in 2020 and 2021, the years of the single-year silver itself. An epoch-specific filter would add multi-year information to C and confound C−A with B−A.

**Erratum-1 clarifications** (registered before any label existed):

* **Arm C** = arm-A cells where the GLC_FCS30D 2020 **and** 2021 L1 classes are both equal to the arm-A silver class.
* **"Tuning block"** in every population and distance rule means the cells of `tuning_blocks_30m.tif`, i.e. the training-eligible cells of the tuning 6 km blocks.
* **Training population of every arm** is `train_eligible_T1_30m.tif`. One T1 train point is 1 860 m from a non-eligible remainder of a tuning 6 km block; it is 2 010 m from the nearest tuning cell.
* **Researcher countersignature.** This review was made by the Phase-6 lead under the researcher's instruction. P4-C1@v2 cannot run until the researcher countersigns (checklist item `researcher_countersignature_D1`; template `docs/templates/phase6_countersignature_TEMPLATE.json`). T1 interpretation should start only after the countersignature.

**Secondary analysis (not decisive):** an equal-size control, with A subsampled to the size of D over 5 seeds.

## 4. T1 v2 (design only; 0 labels)

| item | value |
|---|---|
| points | 699 (525 train, 174 tuning), 140 double-interpretation (20 %), boundary share 0.511 |
| separation | minimum distance to gold 2 192 m; train ≥ 2 010 m from tuning blocks; 0 points in validation blocks |
| class-balance proxy (GLC_FCS30D 2020 L1 at the points — **for design checking only, never a label**) | train: agriculture 300, natural vegetation 92, built 78, water 36, other 19; tuning: 100 / 28 / 26 / 10 / 9 (+1 bare) |
| water/built floors | added after a first draw was checked against the GLC 2020 proxy (about 4 water-proxy tuning points); set before any label existed |
| residual risk | tuning macro-F1 rests on about 10 water points per epoch (proxy), so selection between close arms is noisy. This affects only which arm is carried forward, never the gold evaluation. |

Files: `data/labels/training/phase6_T1v2/` (design, blind forms A/B, separate KMLs for A and B, key) and `scripts/phase6_training_sample_T1v2.py` (seed 20261221).

## 5. What D1 does NOT do

* It does not approve running P4-C1@v2. Gold, T1 labels, district silver and the district feature cube are still missing (`docs/phase6_readiness_gate.md`).
* It does not change protocol v2, the hypothesis, the comparator or the decision rule.
* It does not use any result: none exists.
