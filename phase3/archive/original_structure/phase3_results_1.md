# Phase 3 — results

_Generated from `experiments/registry3/` and `results/phase3/` by `scripts/write_phase3_docs.py`. No Tier-A (human) gold exists: every accuracy-like number below is **agreement with reference products** (Tier B independent products, Tier C silver / provisional AI)._

Registry: 63 records — FAIL 1, INVALID 6, PASS 54, PENDING 2; outcomes of PASS records: INCONCLUSIVE 5, NA 6, NOT_SUPPORTED 15, SUPPORTED 28.

## 0. Answer to the central question

*Can multi-temporal and multi-sensor models reconstruct land-cover transitions more reliably across time and space than single-year optical models, under sensor shift, seasonal variability, drought and landscape transfer?*

**Partly yes, measurably — and not yet defensibly.**

* **Across time (history):** multi-year information is what helps. A 5-year Landsat window cuts pooled dry-year false built-up on never-settled land by about half versus single-date (Gate 2 PASSED), and training labels spanning many years cut it further (by 39-41 % for single-year feature sets, 19 % on top of the 5-year window). A Transformer on the 5-year sequence (P3-F3) is best on every historical criterion, yet Baramati keeps dry-year false built-up of 0.18 on the never-settled frame (> 0.4 with every tree model), and window-wide the selected model still maps half of the corridor as built in 1990 (WSF: 21 %) with a built share that falls until 2000 — not a credible history. Drought context (SPI) and terrain add nothing on top.
* **Drought:** once dry years are compared with normal years of the SAME sensor era, the pooled drought effect disappears; only Baramati keeps a smaller dry-year excess. Much of the apparent drought effect coincides with sensor era and distance from the label year (confounded; association only).
* **Across space (transfer):** SAR helps on unseen landscapes in the S1 era (Gate 3 PASSED); a multimodal-temporal model beats single-date optical on every held-out window (Gate 4 PASSED). Transfer gaps remain large, unsupervised domain adaptation fails (CORAL is destructive), a few hundred target labels close roughly half of the gap.
* **Uncertainty:** held-out ECE is 0.03-0.12 and source-chosen recalibration does not reliably bring it under 0.05 (P3-H2/H3 NOT SUPPORTED); conformal sets calibrated on the source landscape under-cover badly (31-36 % empty sets, P3-H4 NOT SUPPORTED); Gate 5 NOT PASSED. Calibrating on a third landscape largely repairs coverage.
* **Defensibility:** without human gold (Gate 1 NOT PASSED) and with dry-year false built-up above 0.10, the history is not defensible (Gate 6 NOT PASSED); nothing is scaled district-wide.

The hypothesis was falsifiable and parts of it are refuted: drought context, terrain-in-history, 10 m cropping-regime separability, unsupervised adaptation, source-calibrated conformal coverage, domain-shift-predicts-error and harmonisation transfer all came out NOT SUPPORTED.

## 1. Phase-2 audit

File-level audit (`docs/phase3_start_audit.md`): checks {'PASS': 20, 'WARN': 2, 'FAIL': 1}. WSF corrected in every window and no experiment used pre-fix WSF; old-reader S2 families quarantined; valid/no-data separation holds; maps equal the exported COGs. The FAIL is the gold sample: 0 of 558 points interpreted.

## 2. Gold sample

Blind kit rebuilt (the Phase-2 kit exposed strata). Human Tier-A interpretation is not possible here -> **P3-A2 PENDING, Gate 1 NOT passed**. A provisional, blind AI interpretation of Sentinel-2 10 m 2021 chips (Tier C, never gold) was frozen before the key was opened: abstention 21.7%; repeat-pass κ 0.69 (agreement complete where both passes labelled; all disagreement is about abstaining); abstention differs between AI instances (p 4e-05).

## 3. The true baseline: what 0.979 is

| evaluation of the 2021 configuration (XGBoost, Landsat seasonal + terrain) | value | tier |
|---|---|---|
| silver agreement, TEST blocks (Phase 2: 0.979) | 0.979 | C |
| macro-F1 vs blind provisional AI interpretation (non-abstained, 95 % CI) | 0.884 [0.856, 0.910] | C |
| overall agreement vs provisional AI (non-abstained, 95 % CI) | 0.892 [0.866, 0.916] | C |
| … overall agreement on points without silver consensus | 0.758 | C |
| built-up F1 vs GHSL 2020 over ALL TEST cells khadakwasla_mutha/baramati_agri/mulshi_ghats | 0.72 / 0.32 / 0.23 | B |
| unseen window (LOWO) macro-F1 khadakwasla_mutha/baramati_agri/mulshi_ghats (Phase 2: 0.57-0.82) | 0.863 / 0.784 / 0.687 | C |
| held-out ECE khadakwasla_mutha/baramati_agri/mulshi_ghats | 0.064 / 0.043 / 0.119 | C |
| 2020 silver macro-F1 (temporal hold-out) | 0.975 | C |
| 1990 false built-up on never-settled TEST cells khadakwasla_mutha/baramati_agri/mulshi_ghats | 0.43 / 0.77 / 0.29 | B |

Note: Phase-3 LOWO uses the class-capped HET silver cells (balanced per class and split), Phase 2 used every silver cell, so the two LOWO ranges are not directly comparable; the gap to within-window agreement is the comparable quantity. 0.979 is silver agreement on easy, pure cells of the label epoch. Against independent built-up products, against an independent visual reference, on unseen landscapes and back in time, agreement is far lower. **It must not be called accuracy.**

## 4. Historical failure — details in `docs/historical_reconstruction.md`

## 5. Temporal AI — `docs/temporal_ai.md`

Deep temporal models (run because B4 beat B1): P3-F1 PASS / SUPPORTED; P3-F2 PASS / SUPPORTED; P3-F3 PASS / SUPPORTED.

Self-supervised pretraining: P3-G1 PASS / NA, P3-G2 PASS / NOT_SUPPORTED, P3-G3 PASS / INCONCLUSIVE.

## 6. Multimodal fusion — `docs/multimodal_fusion.md`

Unseen-window macro-F1 C1 optical -> C3 +SAR -> C4 +terrain: khadakwasla_mutha 0.776 -> 0.839 -> 0.902; baramati_agri 0.755 -> 0.806 -> 0.893; mulshi_ghats 0.680 -> 0.712 -> 0.731. SAR alone is worse than optical.

## 7. Transferability — `docs/domain_shift.md`

Single-source B3 transfer: cross 0.61-0.75 vs within 0.90-0.94 (Baramati within 0.74 is deflated by an absent class; 0.98 over present classes); SAR helps in all six directions. Windows are separable with AUC 0.97-1.00, yet cell-level domain probability predicts error in only 1 of 6 directions. Few-shot labels close part of the gap (median 0.56 with 100-250 labels/class); CORAL/importance weighting do not.

## 8. Calibration — `docs/uncertainty_calibration.md`

Held-out LAC coverage at 90 % (B3T): khadakwasla_mutha 0.645, baramati_agri 0.630, mulshi_ghats 0.628 (Phase 2: 0.508 / 0.423 / 0.742) with 31-36 % empty sets; APS 0.933, 0.944, 0.826; third-landscape calibration 0.85-0.94 (B3T; 0.80-0.99 including C4). The Phase-2 calibration failure persists for source-calibrated conformal sets. Outcomes P3-H2 PASS / NOT_SUPPORTED, P3-H3 PASS / NOT_SUPPORTED, P3-H4 PASS / NOT_SUPPORTED.

## 9. Change, harmonisation, agriculture, flood

* Change timing (K1): recall +-2 y 0.390; 82% of misses fall in data/reference categories (PASS / SUPPORTED). Probabilistic timing (K2): 80 % sets cover the WSF year for 5% (PASS / NOT_SUPPORTED).
* Harmonisation (I1): PASS / NOT_SUPPORTED — window-specific residuals up to ~0.025 reflectance (NIR/SWIR, Baramati).
* Agriculture: state framework produced with OBSERVED / INFERRED / PROXY tags (PASS / SUPPORTED); no crop types; 10 m vs 30 m separability PASS / NOT_SUPPORTED; official statistics exist only as district totals (PASS / INCONCLUSIVE).
* Flood (N1): only the Baramati May-2025 event has an S1 acquisition (+1 day); the candidate extent (1.1-3.6 km² depending on threshold) is UNVALIDATED; no flood AI.

## 10. Selection, product, scaling

Pre-declared multi-criteria rule (`configs/phase3.yaml:model_selection`, declared before the C/E/H/F/G results) selected **P3-F3** (`results/phase3/product/model_selection_table.csv`). Era-aware probabilistic rasters exist for the three benchmark windows, 1990-2026. It is best on every never-settled-frame criterion, but window-wide it maps half of the corridor and 63 % of Baramati as built in 1990 and its built share falls until 2000 while WSF rises (docs/historical_reconstruction.md) — not a credible history. Error taxonomy (P3-O2): most false built-up cell-years fall in years far from the 2021 label epoch. Gate 6 NOT PASSED -> P3-O3 (district scaling) PENDING.

## 11. Decision gates

| gate | status | evidence |
|---|---|---|
| G1 | NOT PASSED | 0/558 Tier-A (human VHR) labels; provisional AI labels (Tier C) cannot pass by protocol; AI self-consistency kappa 0.69 (not inter-annotator) |
| G2 | PASSED | P3-B4 vs P3-B1 pooled dry-year FBR -0.136 (rel -49%, CI -0.176..-0.093), built-recall change -0.011; per-window dry FBR after B4: {'khadakwasla_mutha': 0.037, 'baramati_agri': 0.513, 'mulshi_ghats': 0.016} (baramati_agri remains > 0.5) |
| G3 | PASSED | C3-C1 unseen macro-F1: baramati_agri +0.052 [+0.033,+0.066], khadakwasla_mutha +0.062 [+0.050,+0.078], mulshi_ghats +0.032 [+0.018,+0.053] (S1 era, 2021) |
| G4 | PASSED | held-out macro-F1 B4+S1+terrain vs B1: khadakwasla_mutha 0.922 vs 0.789 (+0.134 [+0.096,+0.166]); baramati_agri 0.872 vs 0.703 (+0.169 [+0.100,+0.203]); mulshi_ghats 0.776 vs 0.622 (+0.154 [+0.123,+0.199]) — caveat: P3-E5 was registered after the C-group (SAR/terrain) results were seen, so this is not a blind test |
| G5 | NOT PASSED | B3T: khadakwasla_mutha ECE 0.064 / LAC cov 0.645, baramati_agri ECE 0.046 / LAC cov 0.630, mulshi_ghats ECE 0.173 / LAC cov 0.628; C4: khadakwasla_mutha ECE 0.034 / LAC cov 0.655, baramati_agri ECE 0.007 / LAC cov 0.692, mulshi_ghats ECE 0.080 / LAC cov 0.661 |
| G6 | NOT PASSED | G1 not passed; selected P3-F3: dry-year FBR baramati_agri 0.184, khadakwasla_mutha 0.005, mulshi_ghats 0.002; block MAE vs WSF <= 0.10 everywhere: False |

## 12. The Phase-2 findings that had to stay visible

* **75 % of the corridor mapped built-up in 1990** -> re-measured the same way: 0.74 with the re-fitted 2021 configuration (reproduced) and 0.50 with the selected Phase-3 model, against WSF 0.21 (GHSL mean built fraction 0.07). On the stricter never-settled frame the 1990 rate falls from 0.43 (2021 configuration) to the values in docs/historical_reconstruction.md. Reduced, not solved.
* **0.57-0.82 transfer** -> Phase-3 re-assessment 0.86/0.78/0.69; still the largest gap in the system; SAR + terrain + temporal raise it (P3-E5).
* **42-74 % calibration coverage** -> source-calibrated LAC on unseen landscapes 0.65, 0.63, 0.63; not fixed by recalibration on the source.

## 13. Answers to the Phase-3 questions (by component)

| question | answer (evidence) |
|---|---|
| Does Phase 2 hold up under audit? | Files consistent; WSF/S2 bugs fixed and not propagated; gold absent (audit). |
| Is there independent gold? | No human gold (0/558). Provisional AI reference only (Tier C). |
| What is the true 2021 baseline? | 0.979 silver; ~0.88 vs provisional AI; built-up F1 0.23-0.72 vs GHSL; 0.69-0.86 unseen (section 3). |
| Where does history fail? | Early era (TM-only, 1990-1998) and the agricultural Baramati window; single-year training labels make it worse; failures coincide with era more than with drought once era-matched (D, post-hoc). Mechanisms are not established. |
| Does temporal information help? | Yes for history (Gate 2 PASSED); deep temporal models: P3-F1 PASS / SUPPORTED; P3-F2 PASS / SUPPORTED; P3-F3 PASS / SUPPORTED. |
| Does SAR help? | Yes on unseen windows in the S1 era (Gate 3 PASSED); untestable before 2015; drought robustness untested (one dry year). |
| Does multimodal-temporal transfer better? | Yes vs single-date on all 3 held-out windows (Gate 4 PASSED). |
| Which features differ between landscapes? | Brightness, terrain and moisture indices; windows ~perfectly separable; domain probability predicts cell error in 1 of 6 directions only (J1 NOT SUPPORTED). |
| Does adaptation work? | Few-shot target labels yes (about half the gap with 100-250/class); CORAL/importance weighting/self-training no (J2). |
| Is uncertainty calibrated on unseen landscapes? | ECE moderate; conformal coverage fails when calibrated on the source (Gate 5 NOT PASSED); third-landscape calibration repairs most of it. |
| Can change timing be trusted? | Not at annual precision: K1/K2 (section 9). |
| Is the reconstruction defensible / scalable? | No (Gate 6 NOT PASSED); product stays a benchmark-window research artefact. |

## 14. End-of-Phase-3 checklist

| item | status |
|---|---|
| Phase-2 audit | done (docs/phase3_start_audit.md) |
| gold sample processed | blind kit + provisional Tier-C pass done; human Tier A PENDING |
| true baseline | done (A1, A1b, A2p, A3, A4) |
| historical failure attacked | done (B, D, K) |
| temporal tested | done (B, F, G) |
| multimodal tested | done (C, E5) |
| transferability tested | done (E, J, L) |
| uncertainty calibrated/tested | done (H, K2) |
| harmonisation validated | done — fails outside corridor (I1) |
| agriculture without crop-type claims | done (M1-M3) |
| flood inventory, no flood AI | done (N1) |
| error taxonomy, model selection, product | done (O1, O2) |
| district scaling | PENDING (Gate 6) |
| future scenarios | LOCKED |
| every experiment pre-registered, nothing overwritten | yes (63 records; FAIL/INVALID kept) |
| figures / docs / tests | results/figures/phase3/, 10 docs, tests/test_phase3.py |

## 15. Independent verification and outcome-logic caveats

An independent verification pass (separate agent, read-only) checked ~150 numbers in these documents against the registry and result files; the mismatches it found were corrected in the text. It also found outcome-logic issues in completed records, which cannot be edited (immutability) and are disclosed here:
* **E1-E3r** are worded 'temporal and SAR features reduce the gap (within - cross)' but were scored on cross-window differences only, requiring temporal OR SAR to help. On gap terms the 5-year window WIDENS the gap in E3 and E1r (it improves within-window agreement more than cross).
* **B-group thresholds differ:** B2-B4 used the Gate-2 >= 25 % rule (P3-B3 at -19 % INCONCLUSIVE); the m-variant @v2 re-scoring used 'lower, CI excluding 0' (P3-B4m@v2 at -19 % SUPPORTED).
* **P3-F1** is SUPPORTED with unseen gains in 2 of 3 windows (Baramati -0.016, CI including 0); the 2-of-3 rule was in the script docstring before the run, not in the hypothesis text.
* **P3-K1** counts 'no NDVI signal' (41 % of misses) as non-detector although the hypothesis lists only gradual growth, sensor transitions, noisy series and reference years (together 41 %): as worded, the hypothesis is only partly supported.
* **P3-C3..C5** were scored on the unseen-window part only; the within-window part is reported but not tested, the S1-era dry-year part is untestable (one dry year).
* **P3-J2** was scored on 100/250 labels per class; with 25/50 labels the median gap closed is only 0.13/0.34, so 'few labels close most of the gap' holds only at 100-250 per class.
* **P3-L1** compares SSL with XGBoost rather than with the no-SSL Transformer (G1); **P3-A2p** compares with the Phase-2 silver value on a different cell set, without a paired CI; **P3-B4t@v2** tests dry years only; P3-D2 pairs B5m/B4t against B1m.
* **Timing of declarations:** gate criteria G1-G6 were committed in 0d659da (08:23 UTC) before any Phase-3 experiment ran; the model-selection rule was appended to configs/phase3.yaml at 09:16 UTC, after the B-group results and before C/E/F/G/H. **P3-E5 (the Gate-4 model: B4 + S1 + terrain) was registered at 09:38 UTC, 19 s after the C4 (terrain) result was written** — its feature design was chosen knowing that terrain and SAR help on unseen windows, so Gate 4 is not a blind test.
* Held-out ECE values for the same B3T configuration differ slightly between P3-A3 (early stopping on all source validation blocks) and P3-H1 (early stopping on half of them).

## 16. PENDING experiments

| id | reason | needs |
|---|---|---|
| P3-A2 | no human VHR interpretation available in this environment (0/558 Tier-A labels); blind kit ready in data/labels/gold/blind/ | two human interpreters with VHR access (Google Earth Pro historical imagery), ~558 points x 5 epochs + 20 % double interpretation |
| P3-O3 | Gate 6 not passed (no Tier-A gold; dry-year false built-up above 0.10 in at least one window) -> district-wide scaling of an undefended history is not run | Tier-A gold (G1) and a model meeting G6 on the benchmark windows |
