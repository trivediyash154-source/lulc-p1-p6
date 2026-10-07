# Phase 5 — results

_Generated 2026-10-02T16:52:51Z from saved results by `scripts/write_phase5_docs.py`; git 7cdcef4. Protocol v2 unchanged (code 3bb40f9c982d, YAML fe1fad6f8b8e)._

**No confirmatory experiment was executed** (pre-run checklists: 6/6 DO NOT RUN). No accuracy, F1, IoU, area estimate or calibration on Tier-A gold exists, because **0 human labels exist**. Everything below is data-foundation, design or EXPLORATORY evidence; exploratory records never decide a gate and are not confirmatory evidence.

## 1. Completed

| item | status | evidence |
|---|---|---|
| entry audit | done; 31/33 machine checks PASS (FAIL: tier_A_gold_files_present, earth_engine_configured) | `results/phase5/entry/entry_checks.json` |
| gold + T1 ingestion/audit pipeline (revision 2 after independent review) | built; 26 unit tests on synthetic data; real run: ingest FAIL (no Tier-A gold), G2 FAIL | `results/phase5/gold/gold_audit.json` |
| district cube validator + static GEE check | built; 10 unit tests; GEE static check PASS (16/16) | `results/phase5/cube_validation/` |
| district 30 m key-epoch pilot | 4 epochs built (1990, 2000, 2010, 2020), validator ok; identical to the window composites on 3,846,463 overlapping cell-epochs (reflectance and n_clear, max diff 0) | §2.2 |
| T1 human training-sample design | 635 points, {'train': 485, 'tuning': 150}, boundary share 0.507, leakage audit PASS (min distance to gold 2220 m); arms C-E definitions PROPOSED, need approval | `data/labels/training/phase5_T1/` |
| exploratory analyses | P5-X1, X2, X3, X4, E2 registered before running | registry4 |

## 2. Results

### 2.1 Observation support (P5-X1)

*Dataset*: 240 m composite metadata for every year/season (Phase 1) and the 30 m pilot. *Sample*: all district cells (census). *Metric*: share of district cells with ≥ 3 clear observations in the season (G1 rule threshold 0.80). *Uncertainty*: no sampling uncertainty (census); cloud-mask omission errors are not quantified; 240 m counts are conservative (a coarse cell is clear only if all QA samples are clear). *Test*: none.

* Dry season meets the G1 annual rule in **32 of 37 years**; it fails in 1995, 1997, 2003, 2004, 2005.
* Post-monsoon meets it in 17 years, the earliest 1996; before 2013 only 1996, 2004, 2008, 2009.
* The wet season meets it only in 2023 (optical; SAR is required for the monsoon). 2026 post-monsoon is incomplete (archive cut-off 2026-10-01).
* 30 m pilot vs 240 m at the key epochs: 1990: 0.958 vs 0.957; 2000: 0.984 vs 0.979; 2010: 1.000 vs 0.996; 2020: 1.000 vs 1.000.
* Consequence: the G1 annual rule (dry season) holds in most but not all years; the registered feature set (B3) also uses post-monsoon features, whose support reaches the same threshold in only 4 years before 2013, so pre-2013 feature vectors are partly missing (NaN, never filled). Epoch-level claims are therefore better supported than an annual 1990-2026 multi-season record.

### 2.2 District 30 m key-epoch pilot (data product, not an experiment)

*Dataset*: Landsat C2 L2 (Planetary Computer, manifest cut-off 2026-10-01), repository engine, harmonisation local_pune_g30_v2. *Sample*: all district cells. *Metric*: shares of cells by clear-observation count. *Uncertainty*: census; per-pixel SE of the median is stored in `uncertainty.tif`.

| year | scenes (sensors) | valid (≥ 1 clear obs) | reliable (≥ 3) | n_clear median | never observed | 240 m reliable, same year | validator |
|---|---|---|---|---|---|---|---|
| 1990 | 16 (landsat-5 16) | 1.0000 | 0.958 | 7 | 0.0000 | 0.957 | PASS |
| 2000 | 17 (landsat-5 9, landsat-7 8) | 1.0000 | 0.984 | 4 | 0.0000 | 0.979 | PASS |
| 2010 | 34 (landsat-5 12, landsat-7 22) | 1.0000 | 1.000 | 10 | 0.0000 | 0.996 | PASS (1 warnings) |
| 2020 | 71 (landsat-7 37, landsat-8 34) | 1.0000 | 1.000 | 16 | 0.0000 | 1.000 | PASS |

Consistency with the windows the Phase-3 models were trained on (`results/phase5/cube_validation/pilot_vs_window_composites.json`): in 12 epoch x window overlaps (3,846,463 cell-epochs valid in both) reflectance and n_clear are bit-identical (max |diff| 0 x1e-4). The validator checks internal consistency; this checks comparability.

### 2.3 G5 built-up reference envelope (P5-X3), frozen before any district prediction

*Dataset*: WSF-Evolution, GHSL BUILT-S (fraction-weighted), GISA 2.0, GLC_FCS30D impervious on the district grid. *Sample*: all district land cells. *Metric*: built area / land area. *Uncertainty*: the spread between products is the definitional uncertainty; no product is truth. File SHA-256 `76ec5714f1225a3a…`.

| epoch | WSF | GHSL | GISA | GLC_FCS30D | envelope | G5 band (±0.02) |
|---|---|---|---|---|---|---|
| 1990 | 0.0171 | 0.0075 | 0.0111 | 0.0117 | 0.0075-0.0171 | 0.0000-0.0371 |
| 2000 | 0.0291 | 0.0122 | 0.0209 | 0.0246 | 0.0122-0.0291 | 0.0000-0.0491 |
| 2010 | 0.0456 | 0.0183 | 0.0316 | 0.0477 | 0.0183-0.0477 | 0.0000-0.0677 |
| 2020 | — | 0.0271 | 0.0476 | 0.0585 | 0.0271-0.0585 | 0.0071-0.0785 |

The envelope is wide (max/min ratio 1990: 2.3, 2000: 2.4, 2010: 2.6, 2020: 2.2). Because the ±0.02 tolerance exceeds the lower envelope values, the lower bound of the G5 band is 0 for 1990, 2000, 2010: **under-prediction of built-up can never fail the whole-area test in those epochs**, and over-prediction fails only above the upper bound. The whole-area test is therefore a weak discriminator; G5 rests mainly on the gold accuracy criterion (protocol text unchanged; caveat recorded here before any prediction).

### 2.4 Where products disagree (P5-X2) and water products (P5-X4)

*Dataset*: GLC_FCS30D, Esri, WorldCover 2020 (L1 crosswalk), WSF, GHSL, JRC. *Sample*: all district cells with a class in all three products. *Metric*: share of cells without full L1 consensus (agreement, never accuracy). *Uncertainty*: census; descriptors are not independent of each other. *Test*: none.

* District no-consensus share 0.400 (P4-X3: 0.40).
* Class boundary (registered definition): 0.540 on boundary cells vs 0.153 in interiors; boundary cells are 0.638 of the district.
* Leave-one-product-out (boundary from one product, disagreement between the other two; less circular; fixed before running but NOT in the registration text - unregistered robustness): GLC_FCS30D: 0.433 vs 0.264 (Esri vs WorldCover); Esri: 0.424 vs 0.316 (GLC_FCS30D vs WorldCover); WorldCover: 0.244 vs 0.170 (GLC_FCS30D vs Esri) — the boundary excess persists in all three.
* Settlement context: urban_core_ghsl2020_ge0.3 0.251 (area 0.023); fringe_le1km_from_wsf2015_not_core 0.428 (area 0.589); rural_gt1km 0.367 (area 0.388). The fringe rate is higher than rural and urban-core, but the fringe class covers most of the district, so it is not a concentration.
* Rainfall region x slope (highest rates): dry_east_lt700mm|ge8deg 0.952 (area 0.009); dry_east_lt700mm|3_8deg 0.706 (area 0.034); transition_700_1200mm|3_8deg 0.644 (area 0.088); transition_700_1200mm|ge8deg 0.612 (area 0.069) - small-area sloping land in the dry east and transition zone.
* Water 2020 (km²): JRC_occ_ge50 400, JRC_occ_ge25 507, GLC_FCS30D_2020 291, Esri_2020 657, WorldCover_2020 501. Share of cells mapped as water, by JRC occurrence class (GLC_FCS30D / Esri / WorldCover): occ_ge75: 0.837/1.000/0.994; occ_25_75: 0.180/0.951/0.614; occ_1_25: 0.084/0.631/0.332; occ_0: 0.000/0.005/0.003. Products agree on permanent water and diverge on seasonal water.

### 2.5 Sensor era: near-coincident cross-sensor pairs (P5-E2)

*Dataset*: Landsat C2 L2 scene pairs 8 days apart on the same WRS-2 path/row, Jan-May, scene cloud ≤ 20 %, read at 240 m. *Sample*: pure 240 m PIF cells in training-eligible areas (water 781, forest 8748, impervious 0 cells). *Metric*: mean over pairs of the per-pair median difference (a − b). *Uncertainty*: registered pair-bootstrap 95 % CI and post-hoc scene-cluster bootstrap 95 % CI (2000 each). *Test*: none; no multiplicity correction (exploratory).

| contrast | variant | PIF | band | pairs | mean diff | 95 % CI pair bootstrap (registered) | 95 % CI scene-cluster bootstrap (post-hoc) |
|---|---|---|---|---|---|---|---|
| E2ab_L5_vs_L7 | b_harmonised | forest_pif | blue | 24 | +0.0209 | [+0.0181, +0.0239] * | [+0.0174, +0.0241] (17 clusters) * |
| E2ab_L5_vs_L7 | b_harmonised | forest_pif | green | 24 | +0.0184 | [+0.0155, +0.0211] * | [+0.0150, +0.0213] (17 clusters) * |
| E2ab_L5_vs_L7 | b_harmonised | forest_pif | red | 24 | +0.0169 | [+0.0142, +0.0196] * | [+0.0139, +0.0194] (17 clusters) * |
| E2ab_L5_vs_L7 | b_harmonised | forest_pif | nir08 | 24 | -0.0011 | [-0.0033, +0.0011] | [-0.0035, +0.0010] (17 clusters) |
| E2ab_L5_vs_L7 | b_harmonised | forest_pif | swir16 | 24 | +0.0091 | [+0.0062, +0.0120] * | [+0.0053, +0.0123] (17 clusters) * |
| E2ab_L5_vs_L7 | b_harmonised | forest_pif | swir22 | 24 | +0.0064 | [+0.0043, +0.0084] * | [+0.0039, +0.0084] (17 clusters) * |
| E2ab_L5_vs_L7 | b_harmonised | forest_pif | ndvi | 24 | -0.0779 | [-0.0898, -0.0667] * | [-0.0913, -0.0656] (17 clusters) * |
| E2ab_L5_vs_L7 | b_harmonised | water_pif | blue | 55 | +0.0188 | [+0.0152, +0.0225] * | [+0.0142, +0.0238] (32 clusters) * |
| E2ab_L5_vs_L7 | b_harmonised | water_pif | green | 55 | +0.0165 | [+0.0130, +0.0205] * | [+0.0112, +0.0216] (32 clusters) * |
| E2ab_L5_vs_L7 | b_harmonised | water_pif | red | 55 | +0.0179 | [+0.0145, +0.0215] * | [+0.0135, +0.0222] (32 clusters) * |
| E2ab_L5_vs_L7 | b_harmonised | water_pif | nir08 | 55 | +0.0234 | [+0.0196, +0.0273] * | [+0.0188, +0.0278] (32 clusters) * |
| E2ab_L5_vs_L7 | b_harmonised | water_pif | swir16 | 55 | +0.0078 | [+0.0052, +0.0103] * | [+0.0048, +0.0102] (32 clusters) * |
| E2ab_L5_vs_L7 | b_harmonised | water_pif | swir22 | 55 | +0.0014 | [-0.0005, +0.0032] | [-0.0007, +0.0032] (32 clusters) |
| E2ab_L5_vs_L7 | raw | forest_pif | blue | 24 | +0.0023 | [-0.0006, +0.0052] | [-0.0010, +0.0053] (17 clusters) |
| E2ab_L5_vs_L7 | raw | forest_pif | green | 24 | +0.0077 | [+0.0049, +0.0104] * | [+0.0043, +0.0104] (17 clusters) * |
| E2ab_L5_vs_L7 | raw | forest_pif | red | 24 | +0.0031 | [+0.0004, +0.0057] * | [+0.0000, +0.0056] (17 clusters) * |
| E2ab_L5_vs_L7 | raw | forest_pif | nir08 | 24 | -0.0006 | [-0.0028, +0.0016] | [-0.0029, +0.0015] (17 clusters) |
| E2ab_L5_vs_L7 | raw | forest_pif | swir16 | 24 | +0.0029 | [-0.0000, +0.0056] | [-0.0008, +0.0064] (17 clusters) |
| E2ab_L5_vs_L7 | raw | forest_pif | swir22 | 24 | +0.0002 | [-0.0018, +0.0021] | [-0.0021, +0.0021] (17 clusters) |
| E2ab_L5_vs_L7 | raw | forest_pif | ndvi | 24 | -0.0134 | [-0.0237, -0.0036] * | [-0.0243, -0.0018] (17 clusters) * |
| E2ab_L5_vs_L7 | raw | water_pif | blue | 55 | -0.0000 | [-0.0032, +0.0034] | [-0.0046, +0.0046] (32 clusters) |
| E2ab_L5_vs_L7 | raw | water_pif | green | 55 | +0.0050 | [+0.0014, +0.0087] * | [+0.0001, +0.0102] (32 clusters) * |
| E2ab_L5_vs_L7 | raw | water_pif | red | 55 | +0.0026 | [-0.0007, +0.0062] | [-0.0016, +0.0069] (32 clusters) |
| E2ab_L5_vs_L7 | raw | water_pif | nir08 | 55 | +0.0036 | [+0.0002, +0.0071] * | [-0.0009, +0.0077] (32 clusters) |
| E2ab_L5_vs_L7 | raw | water_pif | swir16 | 55 | -0.0014 | [-0.0037, +0.0011] | [-0.0041, +0.0011] (32 clusters) |
| E2ab_L5_vs_L7 | raw | water_pif | swir22 | 55 | -0.0016 | [-0.0035, +0.0002] | [-0.0037, +0.0003] (32 clusters) |
| E2c2_L7h_vs_L8_2017_2021 | harmonised | forest_pif | blue | 14 | -0.0004 | [-0.0044, +0.0039] | [-0.0043, +0.0045] (13 clusters) |
| E2c2_L7h_vs_L8_2017_2021 | harmonised | forest_pif | green | 14 | -0.0037 | [-0.0074, +0.0001] | [-0.0072, +0.0002] (13 clusters) |
| E2c2_L7h_vs_L8_2017_2021 | harmonised | forest_pif | red | 14 | -0.0044 | [-0.0080, -0.0007] * | [-0.0078, -0.0007] (13 clusters) * |
| E2c2_L7h_vs_L8_2017_2021 | harmonised | forest_pif | nir08 | 14 | -0.0046 | [-0.0102, +0.0016] | [-0.0103, +0.0013] (13 clusters) |
| E2c2_L7h_vs_L8_2017_2021 | harmonised | forest_pif | swir16 | 14 | -0.0042 | [-0.0084, -0.0004] * | [-0.0086, -0.0007] (13 clusters) * |
| E2c2_L7h_vs_L8_2017_2021 | harmonised | forest_pif | swir22 | 14 | -0.0051 | [-0.0084, -0.0020] * | [-0.0084, -0.0022] (13 clusters) * |
| E2c2_L7h_vs_L8_2017_2021 | harmonised | forest_pif | ndvi | 14 | +0.0157 | [-0.0022, +0.0347] | [-0.0015, +0.0340] (13 clusters) |
| E2c2_L7h_vs_L8_2017_2021 | harmonised | water_pif | blue | 13 | +0.0029 | [-0.0078, +0.0173] | [-0.0083, +0.0161] (13 clusters) |
| E2c2_L7h_vs_L8_2017_2021 | harmonised | water_pif | green | 13 | -0.0018 | [-0.0115, +0.0089] | [-0.0121, +0.0098] (13 clusters) |
| E2c2_L7h_vs_L8_2017_2021 | harmonised | water_pif | red | 13 | -0.0014 | [-0.0110, +0.0095] | [-0.0107, +0.0098] (13 clusters) |
| E2c2_L7h_vs_L8_2017_2021 | harmonised | water_pif | nir08 | 13 | -0.0030 | [-0.0141, +0.0106] | [-0.0134, +0.0105] (13 clusters) |
| E2c2_L7h_vs_L8_2017_2021 | harmonised | water_pif | swir16 | 13 | -0.0033 | [-0.0088, +0.0023] | [-0.0086, +0.0027] (13 clusters) |
| E2c2_L7h_vs_L8_2017_2021 | harmonised | water_pif | swir22 | 13 | +0.0009 | [-0.0027, +0.0043] | [-0.0028, +0.0045] (13 clusters) |
| E2c_L7h_vs_L8_2013_2016 | harmonised | forest_pif | blue | 18 | -0.0082 | [-0.0102, -0.0062] * | [-0.0103, -0.0060] (15 clusters) * |
| E2c_L7h_vs_L8_2013_2016 | harmonised | forest_pif | green | 18 | -0.0058 | [-0.0076, -0.0039] * | [-0.0079, -0.0040] (15 clusters) * |
| E2c_L7h_vs_L8_2013_2016 | harmonised | forest_pif | red | 18 | -0.0055 | [-0.0074, -0.0037] * | [-0.0076, -0.0036] (15 clusters) * |
| E2c_L7h_vs_L8_2013_2016 | harmonised | forest_pif | nir08 | 18 | -0.0013 | [-0.0049, +0.0023] | [-0.0050, +0.0026] (15 clusters) |
| E2c_L7h_vs_L8_2013_2016 | harmonised | forest_pif | swir16 | 18 | +0.0008 | [-0.0020, +0.0034] | [-0.0018, +0.0035] (15 clusters) |
| E2c_L7h_vs_L8_2013_2016 | harmonised | forest_pif | swir22 | 18 | -0.0019 | [-0.0045, +0.0005] | [-0.0046, +0.0010] (15 clusters) |
| E2c_L7h_vs_L8_2013_2016 | harmonised | forest_pif | ndvi | 18 | +0.0235 | [+0.0139, +0.0345] * | [+0.0144, +0.0352] (15 clusters) * |
| E2c_L7h_vs_L8_2013_2016 | harmonised | water_pif | blue | 26 | +0.0068 | [+0.0004, +0.0133] * | [+0.0001, +0.0137] (23 clusters) * |
| E2c_L7h_vs_L8_2013_2016 | harmonised | water_pif | green | 26 | +0.0022 | [-0.0025, +0.0072] | [-0.0027, +0.0077] (23 clusters) |
| E2c_L7h_vs_L8_2013_2016 | harmonised | water_pif | red | 26 | +0.0011 | [-0.0041, +0.0061] | [-0.0045, +0.0070] (23 clusters) |
| E2c_L7h_vs_L8_2013_2016 | harmonised | water_pif | nir08 | 26 | -0.0034 | [-0.0090, +0.0022] | [-0.0096, +0.0029] (23 clusters) |
| E2c_L7h_vs_L8_2013_2016 | harmonised | water_pif | swir16 | 26 | -0.0031 | [-0.0065, +0.0003] | [-0.0071, +0.0008] (23 clusters) |
| E2c_L7h_vs_L8_2013_2016 | harmonised | water_pif | swir22 | 26 | +0.0014 | [-0.0014, +0.0041] | [-0.0021, +0.0043] (23 clusters) |
| E2d_L8_vs_L9 | harmonised | forest_pif | blue | 22 | +0.0000 | [-0.0007, +0.0008] | [-0.0007, +0.0007] (17 clusters) |
| E2d_L8_vs_L9 | harmonised | forest_pif | green | 22 | -0.0001 | [-0.0021, +0.0019] | [-0.0022, +0.0019] (17 clusters) |
| E2d_L8_vs_L9 | harmonised | forest_pif | red | 22 | -0.0012 | [-0.0027, +0.0006] | [-0.0025, +0.0005] (17 clusters) |
| E2d_L8_vs_L9 | harmonised | forest_pif | nir08 | 22 | +0.0007 | [-0.0009, +0.0025] | [-0.0007, +0.0024] (17 clusters) |
| E2d_L8_vs_L9 | harmonised | forest_pif | swir16 | 22 | -0.0013 | [-0.0035, +0.0008] | [-0.0032, +0.0004] (17 clusters) |
| E2d_L8_vs_L9 | harmonised | forest_pif | swir22 | 22 | -0.0021 | [-0.0040, -0.0002] * | [-0.0040, -0.0001] (17 clusters) * |
| E2d_L8_vs_L9 | harmonised | forest_pif | ndvi | 22 | +0.0071 | [-0.0034, +0.0160] | [-0.0027, +0.0149] (17 clusters) |
| E2d_L8_vs_L9 | harmonised | water_pif | blue | 30 | -0.0018 | [-0.0063, +0.0023] | [-0.0062, +0.0028] (25 clusters) |
| E2d_L8_vs_L9 | harmonised | water_pif | green | 30 | -0.0011 | [-0.0064, +0.0038] | [-0.0060, +0.0041] (25 clusters) |
| E2d_L8_vs_L9 | harmonised | water_pif | red | 30 | -0.0012 | [-0.0056, +0.0037] | [-0.0057, +0.0034] (25 clusters) |
| E2d_L8_vs_L9 | harmonised | water_pif | nir08 | 30 | +0.0009 | [-0.0050, +0.0070] | [-0.0050, +0.0068] (25 clusters) |
| E2d_L8_vs_L9 | harmonised | water_pif | swir16 | 30 | +0.0010 | [-0.0022, +0.0046] | [-0.0024, +0.0047] (25 clusters) |
| E2d_L8_vs_L9 | harmonised | water_pif | swir22 | 30 | +0.0007 | [-0.0017, +0.0034] | [-0.0018, +0.0035] (25 clusters) |

\* CI excludes 0. Water-PIF NDVI rows are omitted (ratio of near-zero reflectances, unstable; full table in `contrast_summary.csv`; L8-vs-L9 water NDVI is NaN in 2 pairs = NOT MEASURED). The scene-cluster bootstrap resamples pairs sharing an acquisition day of sensor a together (post-hoc, added after independent review; the registered pair bootstrap treats pairs as independent and is too narrow). The L7→OLI RMA was fitted on 2014-2016 dry-season composites, so the 2013-2016 L7-vs-L8 contrast is not independent of the fit period. Identifiability limits (not separated by this design): 8-day phenology (forest, dry season: small) and reservoir level/turbidity change (water PIF); acquisition-time / sun-angle differences (L7 drift after ~2017: secondary period only); BRDF and view geometry (same path: nominally identical; residual); atmosphere: random per date, averages out over pairs but not within a pair; 240 m aggregation: PIF purity enforced, QA conservative; differences in SR overviews vs native 30 m not tested here.

POST-HOC (not pre-registered) order check, forest PIF: L5 vs L7-harmonised NDVI is -0.068 when L7 is 8 days later (n=13) and -0.090 when L7 is earlier (n=11): same sign and similar size, so monotonic within-season phenology does not explain it; the raw L5 vs L7 difference is order-dependent (-0.003 vs -0.026), i.e. within phenology/atmosphere noise. Reading (exploratory, against the registered expectations): (a) L5 vs raw L7 is small but not zero (several CIs exclude 0, e.g. forest green and NDVI; part is order-dependent, i.e. phenology/atmosphere); (b) L5 vs L7 harmonised to OLI is large (forest NDVI about −0.08, visible-band offsets +0.017 to +0.021) and survives the cluster bootstrap and the order split: the L7→OLI correction applied only to L7 leaves Landsat-5 data offset from the OLI-equivalent record; (c) **contradicts the registered expectation** for forest: after harmonisation L7 still differs from L8 in 2013-2016 (visible bands −0.006 to −0.008, NDVI +0.023), i.e. a residual of the RMA itself; (d) L8 vs L9 is near zero in all bands except a small swir22 offset. This is consistent with P4-E1 and with the direction hypothesised by P4-C2@v2, which still needs its gold test.

### 2.6 Human gold and confirmatory experiments

* Tier-A labels: **0**. Ingestion FAIL; G2 FAIL (no Tier-A gold has been ingested (0 labels)).

| record | decision | failed checklist items |
|---|---|---|
| P4-C1@v2 | DO NOT RUN | 12: tier_A_gold_ingested, G2_passed_for_2020_and_2000, gold_labelled_epoch_2020, gold_labelled_epoch_2010, gold_labelled_epoch_2000, gold_labelled_epoch_1990, district_cube_validated_for_registered_years_and_periods, registered_feature_set_on_district_grid, tuning_split_rule_adopted, arms_operationally_defined (addendum approved), human_training_labels_T1_ingested, district_silver_arm_A_built |
| P4-C2@v2 | DO NOT RUN | 9: tier_A_gold_ingested, G2_passed_for_2020_and_2000, gold_labelled_epoch_1990, gold_labelled_epoch_2000, district_cube_validated_for_registered_years_and_periods, registered_feature_set_on_district_grid, tuning_split_rule_adopted, selection_prerequisite_P4-C1@v2_completed, cross_sensor_transform_fixed (decision D2) |
| P4-C3@v2 | DO NOT RUN | 10: tier_A_gold_ingested, G2_passed_for_2020_and_2000, gold_labelled_epoch_1990, gold_labelled_epoch_2000, gold_labelled_epoch_2010, gold_labelled_epoch_2020, district_cube_validated_for_registered_years_and_periods, registered_feature_set_on_district_grid, tuning_split_rule_adopted, selection_prerequisite_P4-C1@v2_completed |
| P4-C4@v2 | DO NOT RUN | 7: tier_A_gold_ingested, G2_passed_for_2020_and_2000, gold_labelled_epoch_2020, district_cube_validated_for_registered_years_and_periods, registered_feature_set_on_district_grid, tuning_split_rule_adopted, selection_prerequisite_P4-C1@v2_completed |
| P4-C5@v2 | DO NOT RUN | 8: tier_A_gold_ingested, G2_passed_for_2020_and_2000, gold_labelled_epoch_2020, gold_labelled_epoch_2000, district_cube_validated_for_registered_years_and_periods, registered_feature_set_on_district_grid, tuning_split_rule_adopted, selection_prerequisite_P4-C1@v2_completed |
| P4-C6@v2 | DO NOT RUN | 8: tier_A_gold_ingested, G2_passed_for_2020_and_2000, gold_labelled_epoch_2020, district_cube_validated_for_registered_years_and_periods, registered_feature_set_on_district_grid, tuning_split_rule_adopted, selection_prerequisite_P4-C1@v2_completed, sentinel1_rtc_2020_on_district_grid |

## 3. Hypotheses

| hypothesis | status | why |
|---|---|---|
| P4-C1@v2: training-label quality and temporal design change gold accuracy of historical maps: arms C (independently vali… | NOT TESTED (PENDING) | pre-run checklist failed |
| P4-C2@v2: replacing the identity TM transform with a cross-sensor TM/ETM+->OLI transform (published coefficients or PIF-… | NOT TESTED (PENDING) | pre-run checklist failed |
| P4-C3@v2: an era-aware approach (era-specific normalisation or era-specific models) beats one unified model on gold accu… | NOT TESTED (PENDING) | pre-run checklist failed |
| P4-C4@v2: leave-one-region-out gold macro-F1 of the selected model is >= 0.70 (CI lower bound) with a within-minus-held-… | NOT TESTED (PENDING) | pre-run checklist failed |
| P4-C5@v2: on gold, calibration fitted on non-target regions reaches ECE <= 0.05 and 90 % conformal coverage >= 0.85 in e… | NOT TESTED (PENDING) | pre-run checklist failed |
| P4-C6@v2: optical + SAR + terrain beats optical-only on gold macro-F1 in leave-one-region-out (S1 era, 2020)… | NOT TESTED (PENDING) | pre-run checklist failed |
| P5-X1..X4, P5-E2 (exploratory) | descriptive results above | exploratory: never confirmatory evidence |

## 4. What changed compared with Phases 3-4

* District 30 m Landsat data now exist for four key epochs (dry season) instead of 5.8 % of the district, bit-identical to the window composites where they overlap; for Landsat the 'needs Earth Engine' blocker was overstated (Planetary Computer route works; compute/disk is the constraint). The district Sentinel-1 RTC route is untested.
* Observation support quantified for every year and season district-wide: the dry-season G1 rule fails in 5 of 37 years and post-monsoon support (needed by the registered features) is rarely sufficient before 2013.
* The G5 whole-area envelope is frozen (hash) before any district prediction exists.
* Gold (and T1) can now enter only through a fail-loud ingestion that refuses unfrozen, edited or silently re-frozen raw files; confirmatory runs are gated on a passing ingestion by the pre-run checklist (the frozen evaluation code itself does not read provenance). The Phase-4 claim 'evaluation refuses unfrozen files' was not true before Phase 5; it is now true for ingestion, not for the frozen evaluation modules.
* An independent review of Phase 5 (no HIGH findings) led to: stricter G2 'labelled' definition and a minimum n for kappa, re-freeze detection, symmetric adjudication, T1 ingestion, scene-cluster CIs and corrected wording (docs/phase5_gold_ingestion.md §8).
* Specification gaps in P4-C1@v2 (arms C-E, tuning split, human training labels) and P4-C2@v2 (transform choice) were found BEFORE any data and are documented, not silently fixed.
* The Phase-4 GEE script differed from the reference engine in 8 respects; 7 were aligned and Sentinel-1 was removed from the GEE route (GRD ≠ RTC), all before any execution.
* Unchanged: no human labels, no confirmatory result, decision NOT_READY.

## 5. Scientific interpretation (what the current evidence supports)

Supported now (descriptive, data-foundation level; no model involved):

1. **Observation.** The Landsat archive supports district-wide dry-season composites at 30 m for the four gold epochs (≥ 3 clear observations over 95.8-100.0 % of the district), but post-monsoon observations before 2013 are rarely sufficient and 5 years (1995, 1997, 2003, 2004, 2005) fail the dry-season rule. An epoch-level historical reconstruction is observationally better founded than an annual 1990-2026 record (P5-X1).
2. **Reference uncertainty is large and structured.** Existing products disagree on 40 % of the district, much more on class boundaries than in interiors, and most on sloping land in the dry east and transition zone (P5-X2); built-up products differ by a factor of 2.2-2.6 within each epoch (P5-X3); water products agree only on permanent water (P5-X4). Any claim about 'urban growth' or 'water extent' must name its definition and report this envelope; by inference (not measured here), validation that samples only interiors would overstate accuracy, consistent with the Phase-4 silver-label audit.
3. Sensor era (P5-E2, exploratory, 8-day pairs on PIFs): L5 vs L7 (raw): forest-PIF NDVI -0.013 [-0.024, -0.004], n=24 pairs; L5 vs L7 harmonised to OLI: forest-PIF NDVI -0.078 [-0.090, -0.067], n=24 pairs; L7 harmonised vs L8 (2013-2016): forest-PIF NDVI +0.023 [+0.014, +0.034], n=18 pairs; L8 vs L9: forest-PIF NDVI +0.007 [-0.003, +0.016], n=22 pairs. Pair differences remove multi-year land change but not 8-day phenology/water-level change, acquisition-time or atmosphere differences (identifiability notes). This is evidence about the harmonisation, NOT a test of P4-C2@v2 (which needs gold).

Not supported / not yet testable: every model-based claim (classification accuracy, historical land-cover maps, change rates from our models, transfer across the district, calibrated uncertainty), drivers or causal statements, stress indices, hindcasts, scenarios and a digital twin.

Thesis spine, narrowed by the evidence:

| component | status after Phase 5 |
|---|---|
| OBSERVE | supported as a contribution: district observation-support analysis, 30 m key-epoch cube, validated provenance |
| HARMONISE | partially: cross-sensor pair evidence (exploratory); formal test P4-C2@v2 pending gold |
| DETECT CHANGE | pending gold; epoch-level (1990/2000/2010/2020) rather than annual |
| UNDERSTAND INTERACTIONS / DRIVERS | not started; would be associational only (no causal design) |
| QUANTIFY STRESS / RISK | not supported yet (needs validated maps; water limited to surface-water extent) |
| LEARN WITH AI | pending P4-C1..C6@v2 on gold |
| HINDCAST | pending G5 |
| SCENARIO ANALYSIS, DIGITAL TWIN | out of scope until G9 passes; recommended to remove from the thesis core |
