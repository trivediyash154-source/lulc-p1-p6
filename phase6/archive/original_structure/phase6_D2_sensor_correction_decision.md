# Phase 6 — decision D2: the P4-C2@v2 cross-sensor (TM → OLI) correction

## DECISION: **B — PIF-fitted on non-validation areas** (per-band RMA mapping TM into the cube's OLI-equivalent space). Fit feasible; transform frozen.

| item | location / value |
|---|---|
| treatment transform | `results/harmonization/c2_tm_pif_v1.yaml`, SHA-256 `782eaef3e95c9dbe4b79868d936fd1e96ff1aa681d93ad609b5463ddd119db47` |
| specification | `experiments/registry4/addenda/P4-C2_at_v2_addendum_1.json` (SHA-256 `c000e80c…`), committed in `0633008` **before** the fit script ran |
| fit report | `results/phase6/d2/pif_fit_report.json` |
| fitting sample | `results/phase6/d2/pif_fit_sample.parquet` |

No gold label, validation block or model result was used. P4-C2@v2 was **not** run.

## 1. The fork to be closed

* P4-C2@v2: "replacing the identity TM transform with a cross-sensor TM/ETM+→OLI transform (**published coefficients or PIF-fitted on non-validation blocks**) reduces early-era false built-up and improves 1990/2000 gold accuracy".
* Comparator: `local_pune_g30_v2`, in which landsat-5/4 use identity and landsat-7 uses a local RMA to OLI.
* Leaving the choice open until gold results exist would let the better-scoring variant be chosen afterwards.

**Scope fixed in the addendum (single-factor contrast):** the treatment changes **only** the landsat-5/4 (TM) transform. Landsat-7, -8 and -9 are identical in comparator and treatment.

## 2. Candidates compared

| criterion | A. Published: Roy et al. 2016 ETM+→OLI applied to TM | B. PIF fit: TM → cube OLI-equivalent (landsat-7 after `local_pune_g30_v2`) |
|---|---|---|
| provenance | peer-reviewed (Roy et al. 2016, *RSE* 185:57-70, Table 2). RMA SR form OLI = a + b·ETM+: blue −0.0095/0.9785, green −0.0016/0.9542, red −0.0022/0.9825, NIR −0.0021/1.0073, SWIR1 −0.0030/1.0171, SWIR2 0.0029/0.9949. Checked against the open-access paper and the Earth Engine community tutorial (which uses the Table-2 OLS set) | deterministic local fit; specification registered before fitting; code, seeds and sample stored |
| physical interpretation | spectral-response and processing differences between ETM+ and OLI. TM is assumed equal to ETM+ | maps TM onto exactly the OLI-equivalent space the cube already uses for ETM+, so the two pre-OLI sensors become consistent with each other and with the comparator's L7 treatment |
| applicability to this dataset | **weak.** CONUS, 2013-2014; **LEDAPS SR for both sensors**, whereas Collection-2 OLI uses LaSRC; ETM+, not TM. Coefficients differ strongly from the local ETM+→OLI fit (blue slope 0.98 vs 1.09; NIR 1.01 vs 1.12) | Pune, Collection 2 SR exactly as decoded and masked by the repository loader; 30 m native lattice |
| internal consistency in mixed-sensor epochs | **creates a new inconsistency.** 2000 and 2010 composites mix TM and ETM+ observations per pixel (2000: 9 L5 + 8 L7 scenes; 2010: 12 + 22). Roy-mapped TM and locally mapped ETM+ would disagree by up to ~10 % in blue and NIR inside the same median | consistent by construction: TM and ETM+ reach the same OLI-equivalent space |
| calibration assumptions | linear; Roy's sample spans all CONUS land covers (59 M pixel pairs) | linear RMA; pre-declared PIF classes (water, forest, impervious; 3×3-eroded masks); assumes no land change in ≤ 8 days on PIFs |
| dynamic range | full range | TM p1-p99: blue 0.031-0.111, red 0.024-0.167, NIR 0.021-0.288, SWIR1 0.004-0.323. The impervious PIFs (3 406 distinct cells, 37 445 cell × pair samples) bring bright targets into the fit; brighter surfaces are an extrapolation |
| temporal stability | single 2013-14 calibration | pairs searched in 1999-2011; the pairs that exist span 2000-2001 and 2008-2011 (36 TM days); cluster-bootstrap CIs over days are reported |
| geographic transfer | CONUS → India: unknown | leave-one-path/row-out residual (QA only): median residual \|·\| ≤ 0.005 in every band and path/row, against 0.015-0.025 for identity in the visible bands |
| risk of leakage | none | none: training-eligible cells only (≥ 2 km from validation blocks); no gold point, no tuning split |
| compatibility with frozen protocol | allowed alternative ("published coefficients") | allowed alternative ("PIF-fitted on non-validation blocks") |
| reproducibility | trivially reproducible | deterministic: seeds 20261141 (sampling) and 20261142 (bootstrap); script `scripts/phase6_c2_pif_fit.py`; sample saved |

**Why B.** A would apply a mapping fitted for a different processing chain (LEDAPS-LEDAPS) and region to the sensor it was not fitted for. Worse, inside the same cube it would make TM and ETM+ observations disagree, precisely in the mixed-sensor epochs where C2 is evaluated. That would introduce a new sensor artefact rather than remove one.

B targets the offset documented in the exploratory evidence (P5-E2: L5 vs harmonised L7 on forest PIFs, NDVI ≈ −0.08) and keeps the contrast single-factor. Whether it removes that offset in the cube is not shown here; it is part of what P4-C2@v2 tests.

**The choice was made on these a-priori grounds, before any fit and before any gold.** A pre-declared feasibility rule (≥ 15 TM days with ≥ 50 clear PIF cells, ≥ 500 impervious cells, |r| ≥ 0.7 in every band) decided automatically between B and the pre-declared fallback A.

## 3. Fit result (`results/phase6/d2/pif_fit_report.json`)

| band | intercept | slope | r | 95 % CI slope (cluster bootstrap over TM days) | local landsat-7 → OLI (comparator; descriptive) |
|---|---|---|---|---|---|
| blue | −0.0243 | 1.0867 | 0.80 | 1.004-1.195 | −0.0235 / 1.0880 |
| green | −0.0163 | 0.9856 | 0.88 | 0.923-1.066 | −0.0143 / 1.0479 |
| red | −0.0203 | 1.0393 | 0.94 | 0.993-1.097 | −0.0167 / 1.0313 |
| NIR | −0.0294 | 1.1300 | 0.97 | 1.095-1.166 | −0.0245 / 1.1161 |
| SWIR1 | −0.0083 | 0.9969 | 0.98 | 0.977-1.015 | −0.0095 / 1.0158 |
| SWIR2 | −0.0015 | 0.9674 | 0.98 | 0.951-0.982 | −0.0025 / 0.9739 |

**Feasibility check: passed.**

* 61 L5 × L7 pairs were planned over **45 distinct TM scenes** (TM date × path/row). The report field `n_pairs_used` = 45 counts these TM scenes, not pairs (erratum, below); per-pair contribution was not recorded.
* 16 TM scenes have two L7 partners (one before, one after). Their cells enter the fit once per pair: 104 831 of 195 457 sample rows. This follows the registered pairing rule (every pair with |dt| ≤ 8 days). The bootstrap resamples TM dates, so both entries stay in one cluster.
* 36 TM acquisition days had ≥ 50 cells.
* Samples used (cell × pair): water 110 000, forest 48 012, impervious 37 445. Distinct eroded, eligible PIF cells: water 75 246, forest 986 475, impervious 3 406. The feasibility rule (≥ 500 impervious) passes under either count.
* Minimum |r| = 0.80 (blue).

The fitted TM transform is close to the existing ETM+ transform, as expected if TM ≈ ETM+ (P5-E2 raw contrast). The largest difference is green (slope 0.99 vs 1.05).

**Treatment in force:** `c2_tm_pif_v1` = `local_pune_g30_v2` for landsat-7/8/9, plus the coefficients above for landsat-5 and landsat-4.

## 4. Process notes (full disclosure)

* The first run of the fit script was killed by the sandbox memory limit (OOM) while still reading scenes, before any coefficient was computed. The script was changed only to re-load cached per-scene vectors instead of holding them all in memory; the specification is unchanged (git `a77c7f0` vs `0633008`). The second run produced the result above.
* The fitting pairs (L5/L7, 1999-2011) overlap the data already examined in the exploratory P5-E2. This is acceptable for **constructing** a treatment, because the confirmatory test of P4-C2@v2 is the blind gold comparison. The record already states that the PIF and envelope parts of its metrics are not blind.
* An independent verification found the count mislabelling and the double pairing above, and the "1998-2002" years in the addendum's execution note (2002 has no selected TM scene). These are corrected in `experiments/registry4/addenda/P4-C2_at_v2_addendum_1_erratum_1.json` (pointer appended to the record). The fit was **not** re-run, and the frozen transform is unchanged.
* "No tuning-split use" in the addendum means no tuning **label** is used. PIF imagery cells come from training-eligible cells and may lie in tuning blocks. No label of any split enters the fit.
* The decision was taken by the Phase-6 lead under the researcher's instruction. P4-C2@v2 cannot run until the researcher countersigns it (checklist item `researcher_countersignature_D2`; template `docs/templates/phase6_countersignature_TEMPLATE.json`).
* Limitations:
  * Surfaces brighter than the fitting range are extrapolated.
  * The local L7→OLI transform keeps its own residual against L8 (P5-E2: forest NDVI +0.023, 2013-2016), and B inherits it. That residual concerns the comparator's L7 treatment, which is identical in both arms, so it does not bias the C2 contrast.

## 5. Consequences for execution

* The P4-C2@v2 treatment requires a separate composite family (`landsat_c2tm`) for every year with TM observations: 1990-1992, 1998-2001 and 2008-2011, 477 scenes (`docs/phase6_D3_compute_decision.md`).
* The comparator uses the standard family.
* For 2002 and 2012 (no TM scene) the treatment composite is identical to the standard one by construction. The feature-cube builder uses the standard product for such a year only after checking that it holds zero landsat-4/5 observations (`family_for_year`); otherwise it stops.
* Checklist items:
  * `cross_sensor_transform_fixed`;
  * `c2_treatment_transform_frozen` (SHA-256 pinned to `782eaef3…`);
  * `c2_treatment_family_built` (all 44 products made with `c2_tm_pif_v1` at that SHA, plus `validate_district_cube.py --family landsat_c2tm --require c2tm` ok and complete);
  * `c2_treatment_feature_cube_validated`;
  * `researcher_countersignature_D2`.

Sources: [Roy et al. 2016, open-access PDF (SDSU OpenPRAIRIE)](https://openprairie.sdstate.edu/gsce_pubs/34/) · [Earth Engine community tutorial: Landsat ETM+ to OLI harmonization](https://github.com/google/earthengine-community/blob/master/tutorials/landsat-etm-to-oli-harmonization/index.md)
