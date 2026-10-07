# RESEARCH HISTORY — Chronological Evolution

## Phase 1 — Data Foundation
↓

**What was attempted:** Build a multi-sensor, multi-decadal analysis-ready data archive for Pune district. Compositing pipelines for Landsat (1990–2026), Sentinel-2 (2018–2026), and Sentinel-1 (2017–2025) at three 30 m benchmark windows plus district-wide at 240 m.

**What was learned:**
- 37-year Landsat dry/annual composites are reproducible, co-registered to < 0.1 px
- Sentinel-2 L2A (Sen2Cor) is systematically brighter than Landsat C2 L2 (LaSRC) in every band — 20–50% in blue, gain-type, not PSF/spatial
- The difference is between processors; which processor is closer to truth is unknown
- Monsoon-season optical composites are not usable (optical blindness)
- District-wide products at 240 m only; district 30 m composites were never built in this phase

**What failed:**
- JRC water-shift registration test was invalid (replaced by image cross-correlation)
- 693 of 702 products had 'no-git' or '-dirty' code versions
- WSF-Evolution decoding bug: GDAL warper bumped valid 0 to 1 (every cell 'settled')
- S2 SCL nodata bug: COGs with no declared nodata caused count overstatement

**What was changed:** S2→OLI transform fitted (RMA, 19 same-day pairs). QA now runs inside compositing jobs. WSF decoding fixed with nodata sentinel.

**What was frozen:** Composite archive for 3 benchmark windows.

**What carried forward:** The three benchmark windows' data foundation; the S2-Landsat radiometric finding (blocking fusion without transform).

---

## Phase 2 — Baseline Modelling
↓

**What was attempted:** Train L1 land-cover classifiers, reconstruct historical LULC maps (1990–2026), perform thematic analysis (urban, water, vegetation, flood, stress, change trajectories, drivers). 35 experiments.

**What was learned:**
- Within-window macro-F1 ≈ 0.98 (silver standard)
- Leave-one-window-out macro-F1: 0.66–0.81 (major transferability gap)
- S1 SAR adds information for unseen windows (macro-F1 0.70–0.88)
- HMM temporal smoothing reduces false built-up
- Drought creates false built-up in bare agricultural landscapes
- Reservoir surface area correlates with monsoon rainfall (lag 0–1 year)
- Urban expansion modes characterized (infill/edge/outlying)
- Crop-type clustering failed (silhouette < 0.25)

**What failed:**
- Crop-type classification — clusters are a greenness gradient, not types
- 30 m district maps — only 240 m exists
- TerraClimate water-balance access — Planetary Computer zarr error
- Flood inundation maps — no S1 acquisition during cited events
- Mutha river width at 30 m — cannot resolve dry-season channel

**What was changed:** DEM/terrain layer built. S2→OLI transform applied for fusion.

**What was frozen:** Experiment registry with 35 records (all results against silver standard).

**What carried forward:** Transfer gap finding; S2-Landsat offset; gold labels needed; the 10 PENDING items.

---

## Phase 3 — Systematic Evaluation
↓

**What was attempted:** Comprehensive evaluation with 63 experiments covering: true baseline, historical failure, temporal context, SAR/terrain, transferability, uncertainty, domain adaptation, agriculture, flood, model selection. Gate framework (G1–G6) established.

**What was learned:**
- True 2021 baseline: 0.979 silver; ~0.88 vs AI; 0.23–0.72 vs GHSL (built-up)
- SAR + terrain improve unseen-window transfer (Gate 3 PASSED, Gate 4 PASSED but not blind)
- Multi-year labels and temporal context reduce false built-up (Gate 2 PASSED)
- Conformal coverage fails when calibrated on the source (Gate 5 NOT PASSED)
- Annual change precision not observationally supported (K1/K2)
- S2→OLI transform fails outside corridor (I1 NOT SUPPORTED)
- Domain adaptation: only few-shot target labels work (J2)
- Historical reconstruction: corridor mapped 50% built in 1990 (implausible)
- P3-E5 (Gate 4 model) was registered after seeing results — not a blind test

**What failed:**
- Gate 1: 0/558 Tier-A labels (NOT PASSED)
- Gate 5: Calibration coverage fails on unseen landscapes
- Gate 6: Reconstruction not defensible for district scaling
- P3-I1, P3-J1, P3-J2, P3-K1, P3-K2: NOT SUPPORTED
- P3-F3 selected model: implausible 1990 built share

**What was frozen:** Gate criteria (committed before experiments); 63 experiment records (including FAIL/INVALID).

**What carried forward:** Gate framework; confirmed need for gold labels; SAR+terrain value; transfer gap unsolved.

---

## Phase 4 — District Scaling Design
↓

**What was attempted:** Design the infrastructure for district-wide scientific inference. Freeze evaluation protocol. Design gold validation sample. Benchmark existing products. Pre-register confirmatory experiments.

**What was learned:**
- Existing products disagree by up to 4x on built-up area in 2020
- 1990 built-up agreement F1 is 0.32–0.43 between products
- Three products agree on only 60% of district cells
- Product disagreement highest on 3–8° slopes and transition rainfall zones
- L5-dominated years show higher red and lower NDVI than L8/9 (confounded with time)
- Silver labels biased toward interior cells (edge share 7–25%)

**What failed:** Nothing was designed to fail — this was a design phase. All 6 confirmatory experiments remain PENDING (blocked on gold + cube).

**What was changed:** Protocol v1 → v2 (before any confirmatory run). Gold design v1 (672 pts) → v2 (641 pts) with independent blocks outside Phase 3 windows.

**What was frozen:** Evaluation protocol v2; gold sample design v2; 6 confirmatory pre-registrations (P4-C1@v2 through P4-C6@v2).

**What carried forward:** Frozen protocol; gold design; confirmatory framework; all NOT_READY.

---

## Phase 5 — Pilot Cube + Infrastructure
↓

**What was attempted:** Build pilot district 30 m composites for key epochs (1990, 2000, 2010, 2020 dry season). Validate against window composites. Assess observation support. Extend product benchmark.

**What was learned:**
- District pilot composites are bit-identical to window composites on overlap
- Valid coverage ≥ 0.999999; reliable ≥ 3 obs: 0.958–1.000 across epochs
- Pre-2000 observation support is sparse but sufficient for epoch-level claims
- Earlier reports overstated the need for Earth Engine credentials — Planetary Computer works
- District terrain attempted and OOM-killed (6 GB memory limit)

**What failed:** Full district cube not built (compute/disk boundary); terrain OOM.

**What was changed:** Cube specification formalized. Overstated claims about EE dependency corrected.

**What was frozen:** 4 pilot epoch composites (validated).

**What carried forward:** Pilot validated; cube spec; all gates still FAIL or PARTIAL.

---

## Phase 6 — Gold Workflow + Decisions
↓

**What was attempted:** Take all remaining decisions that don't need data. Build gold and T1 interpreter kits. Formalize the human gold workflow end-to-end.

**What was learned:**
- D1: P4-C1@v2 addendum has a hidden model-selection advantage (corrected)
- D2: Local PIF RMA preferred over Roy 2016 (internal consistency for mixed-sensor epochs)
- D3: Route A (repository engine on Planetary Computer) is the only tested path
- Phase 4/5 interpreter kits were flawed (shipped full protocol; B kit had all 641 points)
- Superseded kits replaced with Phase 6 versions

**What failed:** District terrain OOM-killed again. No labels obtained.

**What was changed:** T1 v1 → v2 (train/tuning separation with 2 km buffer). Interpreter kits redesigned (Phase 6 versions).

**What was frozen:** D1/D2/D3 decisions; T1 v2 design; gold/T1 interpreter kits.

**What carried forward:** All decisions taken; kits ready; 0 labels; NOT_READY.

---

## Phase 7 — Runner Implementation + Restoration
↓

**What was attempted:** Implement and validate the confirmatory runner code. Generate all freeze reports. After container loss, restore repository from Phase 6 snapshot.

**What was learned:**
- Runner implementation works (C1/C2 tested on synthetic data)
- Container was reclaimed — original Phases 1–7 git history lost
- Restoration from Phase 6 snapshot + transcript re-application succeeded
- Spatial context restored: validation blocks byte-identical, training-eligible pixel-exact, district silver byte-identical
- D1/D2 countersignature re-materialized from transcript

**What failed:** Container reclaimed before Phase 7 package was delivered. Git history before Phase 6 snapshot not recoverable.

**What was changed:** Runner specifications PROPOSED (not adopted). C3–C6 runners not implemented yet.

**What was frozen:** Confirmatory state (no test was run); gold freeze report (NOT_AVAILABLE); T1 freeze report (NOT_AVAILABLE); cube manifest (pilot only).

**What carried forward:** Everything needed for confirmatory runs exists except: human labels and the full district cube.
