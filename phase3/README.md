# Phase 3 — Systematic Evaluation

## Purpose

Comprehensive, pre-registered evaluation of the Phase 2 baseline: true accuracy, historical failure modes, temporal context, SAR/terrain contribution, transferability, uncertainty calibration, domain adaptation, agriculture without crop-type claims, flood inventory, and model selection. Establish the gate framework (G1–G6) for district scaling.

## Research Questions

- Does Phase 2 hold up under audit?
- Is there independent gold? What is the true 2021 baseline?
- Where does history fail?
- Does temporal information help? Does SAR help?
- Does multimodal-temporal transfer better?
- Which features differ between landscapes?
- Does adaptation work?
- Is uncertainty calibrated on unseen landscapes?
- Can change timing be trusted?
- Is the reconstruction defensible / scalable?

## Inputs

- Phase 2 trained models and outputs
- Silver labels, provisional AI labels (437 points, Tier C)
- Phase 1 composites and feature cubes
- Independent reference products (GHSL, WSF, JRC, WorldCover, Esri)

## Methods

63 experiments (pre-registered, ID'd as P3-A1 through P3-O3) covering baseline assessment (A), historical remedies (B), SAR/terrain (C), temporal depth (D), model-selected evaluation (E), deep temporal models (F,G), uncertainty/calibration (H), harmonisation (I), domain adaptation (J), change timing (K), SSL/representation (L), agriculture (M), flood (N), model selection (O).

## Experiments

63 registered records. Key results in `reports/phase3_results_1.md`. Selected experiments:
- P3-A1/A1b/A2p/A3/A4: True baseline assessment
- P3-B1-B5: Historical failure remedies
- P3-C1-C5: SAR and terrain
- P3-D1-D2: Era-specific failure analysis
- P3-E1-E5: Model selection and evaluation
- P3-F1-F3: Deep temporal models
- P3-H1: Uncertainty calibration
- P3-I1: Cross-landscape harmonisation
- P3-J1-J2: Domain adaptation
- P3-K1-K2: Change timing

## Results

- True 2021 baseline: 0.979 silver; ~0.88 vs provisional AI; built-up F1 0.23–0.72 vs GHSL
- Gate 2 PASSED: temporal context reduces false built-up (FBR -49%)
- Gate 3 PASSED: SAR improves unseen-window transfer (F1 +0.03–0.06)
- Gate 4 PASSED (not blind): B4+S1+terrain gives 0.78–0.92 on unseen windows
- Gate 1 NOT PASSED: 0/558 Tier-A labels
- Gate 5 NOT PASSED: conformal coverage fails on unseen landscapes (ECE 0.08–0.17)
- Gate 6 NOT PASSED: reconstruction not defensible for district scaling

## Supported Findings

- SAR + terrain improve unseen-window transfer in the S1 era
- Multi-year labels and temporal context reduce false built-up
- Few-shot target labels (100–250/class) close most of the transfer gap

## Unsupported / Rejected Findings

- P3-I1: S2→OLI outside corridor — NOT SUPPORTED
- P3-J1: Domain probability predicts error — NOT SUPPORTED (1 of 6 directions)
- P3-J2: CORAL/importance weighting/self-training — NOT SUPPORTED
- P3-K1/K2: Annual change timing — NOT SUPPORTED
- P3-F3 historical built share: implausible (corridor 0.50, baramati 0.63 vs WSF 0.21)

## Limitations

- 0 Tier-A human gold labels
- Gate 4 is not a blind test (P3-E5 registered after seeing C-group results)
- Several outcome-logic caveats documented in the results §15
- Timing of declarations noted: gate criteria committed before experiments, but model-selection rule added after B-group results

## Important Decisions

- Gate framework (G1–G6) established and frozen before experiments
- P3-F3 selected as the Phase 3 model (but rejected for historical use: implausible 1990 built share)

## Protocol Status

Not documented in the supplied Phase 3 archive. Formal protocol freezing began in Phase 4.

## Key Artifacts

| File | Description |
|------|-------------|
| reports/phase3_results_1.md | Complete Phase 3 results (63 experiments) |
| figures/F01_validation_ladder_2021.png | Validation ladder |
| figures/F15_gate_dashboard.png | Gate dashboard |
| data/phase3_core.zip | Core Phase 3 archive |

## Dependencies

- **Upstream:** Phase 1 (data), Phase 2 (models and results)
- **Downstream:** Phase 4 (gate framework carried forward), all later phases

## Reproducibility

63 experiment records preserved (including FAIL/INVALID). Results include verification notes (§15) documenting outcome-logic caveats and mismatches corrected after independent verification.

## Historical Notes

The Phase 3 findings about historical failure (75% corridor built-up in 1990) were re-measured and confirmed as reduced (to 50%) but not solved. This is an open problem carried forward.
