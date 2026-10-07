# Phase 6 — decision D3: how the full district cube is built

## DECISION: **Route A** — the repository's own engine on Planetary Computer, run on a larger machine.

Route A is the only route already shown to reproduce the dataset on which the Phase-1..3 window cubes, and therefore every earlier result, were built. On overlap the district pilot is bit-identical to the window composites: 12 epoch × window comparisons, 3 846 463 cell-epochs, maximum difference 0 (`results/phase5/cube_validation/pilot_vs_window_composites.json`). Route B is a different code path that has never been executed.

**This sandbox cannot run the full job, so the cube was NOT built here.** The machine has 2 CPUs, a 6 GB memory limit (two OOM kills recorded in Phase 5) and about 13 GB of free disk. The registered minimum needs ≈ 65-80 GB of products, caches and feature cube, and ≈ 85-110 GB with the C2 treatment feature cube (§3); district terrain alone exceeded the 6 GB memory limit here. No reduced cube is labelled complete. The four dry-season pilot epochs remain a **pilot**.

## 1. What must be built (registered minimum; nothing less is "complete")

| component | content | used by |
|---|---|---|
| Landsat standard family `landsat` | annual, dry, post_monsoon and wet composites for 18 years: 1990-1992, 1998-2002, 2008-2012, 2018-2022 and 2021 (the t−2…t+2 windows around 1990/2000/2010/2020 plus the silver years) — **1 203 selected scenes** | all six records, G1 |
| Landsat C2-treatment family `landsat_c2tm` | the same periods for the 11 years with TM observations (1990-1992, 1998-2001, 2008-2011; 477 scenes), composited with the D2 transform | P4-C2@v2 treatment only |
| Sentinel-1 RTC 2020 | dry and wet seasonal composites plus ≥ 6 monthly composites. 107 selected scenes, all descending (configs/sentinel1.yaml keeps one orbit direction per year) | P4-C6@v2 (S1F features) |
| terrain, district | 12 bands (elevation, slope, aspect sin/cos, curvatures, TPI 150/1050 m, flow accumulation, HAND, distance to drainage, TWI) | all six (registered features include terrain) |
| district feature cube | `data/features/cube/pune_G30.zarr` with the window-cube schema: `landsat` (year, 23 features), `quality` (3), `terrain` (12), `sentinel` (syear, 7) | all six |
| district silver (arm A) | WorldCover 2020/21 + Esri 2020/21 at 10 m, purity ≥ 0.78, training-eligible cells | P4-C1@v2 |

## 2. Route comparison

| criterion | Route A: repository engine, Planetary Computer, larger machine | Route B: Earth Engine (`scripts/gee/export_district_cube.py`) |
|---|---|---|
| scientific equivalence | **identical code path** to the window cubes; bit-identical on overlap (shown) | aligned in 7 of 8 respects (Phase 5). Still missing: SE-of-median band, median-DOY band, 1 % overlap rule; EE vs numpy median tie handling differs. Never executed, so the EE graph itself is untested |
| sensor consistency | same manifest (Tier 1, cloud ≤ 70 %, L7 cut at 2021-12-31) and same harmonisation | same rules re-implemented in EE; scene set not pinned to the manifest |
| QA consistency | same QA bits 0-5, DN range, clipping, per-platform counts, DOY, valid mask | QA bits/DN aligned; no DOY, no uncertainty band |
| Sentinel-1 | RTC gamma0 (sentinel-1-rtc), same as the windows | **GRD only — not equivalent; removed from the route.** P4-C6@v2 would still need Route A for S1 |
| reproducibility / provenance | every product's `metadata.json` lists source item IDs, dates, platforms, per-file SHA-256 and code/config hashes; scene list pinned by the manifest (cut-off 2026-10-01) | outputs carry no per-scene source list; EE asset versions are opaque |
| credentials | none (anonymous Planetary Computer) | the user's EE project, plus Drive storage and download of ≈ 50 GB |
| cloud dependency | Planetary Computer STAC + Azure blob (best run in Azure West Europe) | EE batch queue + Google Drive |
| processing time | measured 27-30 s wall per scene on 2 CPUs. Estimate: 1 203 scenes ≈ 9-10 h on 2 CPUs, ≈ 2.5-3.5 h on 8 vCPU; plus ≈ 4 h for the C2 family on 2 CPUs (< 1.5 h on 8 vCPU); S1 2020 not yet measured (10 m reads, ≈ 1-3 h on 8 vCPU, estimate) | queue-dependent, possibly faster, unmeasured |
| storage | §3 | Drive ≈ 50 GB, plus local copies |
| failure recovery | strip-level resume (per-strip reductions kept); per-year skip; per-item retries | per-task re-export |
| registry compatibility | `dataset_version: district cube v1` = the products validated by `validate_district_cube.py --require min` with their recorded hashes | needs an extra overlap comparison against Route A before any mixing with the window cubes |
| risk of silently changing the dataset definition | low, and re-testable (bit-identity check vs windows; validator) | moderate to high: a different implementation that cannot be bit-compared until both exist |

## 3. Resource requirements (Route A)

| item | estimate | basis |
|---|---|---|
| Landsat standard products | ≈ 25 GB (18 years × 4 periods; ≈ 0.35 GB per dry product, annual similar, wet smaller) | pilot sizes |
| C2-treatment products | ≈ 15 GB (11 years × 4 periods) | pilot sizes |
| stage-1 cache (transient, per strip) | 2-6 GB | pilot |
| S1 2020 products | ≈ 2-4 GB | 2 bands float32 per period |
| feature cube (zarr, compressed) | ≈ 20-30 GB (uncompressed ≈ 61 GB: 23 features × 18 years × 36.8 M cells × 4 B; 53 % of the bounding box is outside the district) | arithmetic |
| C2-treatment feature cube | ≈ 20-30 GB (TM years from `landsat_c2tm`, other years from the standard family) | as above |
| **disk** | **≥ 200 GB SSD** recommended (≥ 120 GB minimum) | sum (≈ 85-110 GB) + headroom |
| **CPU / RAM** | **8 vCPU, 32 GB RAM** (memory is bounded by `--min-strips`; 8 threads × ≈ 1 GB per strip read, plus reduction) | pilot: 2 threads ≈ 1-2 GB |
| terrain (measured here) | district terrain (`build_terrain('district')`, pysheds, ≈ 6.7 k × 5.8 k cells with the 3 km buffer) was attempted in this sandbox and **OOM-killed at 6.1 GB RSS** (6 GB cgroup; `results/phase6/terrain_run_status.json`). Peak need > 6 GB, so it runs on the 32 GB machine | kernel log |
| network | outbound HTTPS to `planetarycomputer.microsoft.com` and `*.blob.core.windows.net`; Azure **West Europe** VM recommended | Planetary Computer storage region |
| wall-clock | ≈ 5-8 h total on 8 vCPU (Landsat + C2 family + S1 + terrain + feature cube) | estimate from the pilot; S1 unmeasured |

## 4. Exact commands (to run on the VM)

```bash
git clone <repo> pune-eo-phd && cd pune-eo-phd && pip install -e . && pip install pysheds zarr
# inputs that are gitignored and regenerable (Phase 4 scripts):
python3 scripts/phase4_products_acquire.py          # district products, DEM, mask (data/products_ext/, data/labels/pune_G30/)
python3 scripts/phase4_validation_design.py         # validation blocks, training eligibility, strata (deterministic, seed 20261111)
# T1 v2 design rasters are COMMITTED (data/labels/training/phase6_T1v2/sampling_design/*.tif); do not re-run the T1 script (it refuses to overwrite the pinned key)
Y=1990,1991,1992,1998,1999,2000,2001,2002,2008,2009,2010,2011,2012,2018,2019,2020,2021,2022
python3 scripts/phase5_district_cube_pilot.py --years $Y --periods annual,dry,post_monsoon,wet --threads 8 --min-strips 8
python3 scripts/phase5_district_cube_pilot.py --years 1990,1991,1992,1998,1999,2000,2001,2008,2009,2010,2011 \
        --periods annual,dry,post_monsoon,wet --transform c2_tm_pif_v1 --out-family landsat_c2tm --threads 8 --min-strips 8   # name per results/phase6/d2/pif_fit_report.json
python3 scripts/phase6_district_s1.py --year 2020 --threads 8                      # S1 RTC 2020: dry, wet, monthly (strip driver)
python3 scripts/phase6_district_terrain.py                                          # terrain on the district grid (3 km buffer; > 6 GB RAM, OOM-killed in the sandbox)
python3 src/data/validate_district_cube.py --require min --out results/phase5/cube_validation/validation_report_min.json   # MUST: ok=true, complete_for_required=true
python3 src/data/validate_district_cube.py --family landsat_c2tm --require c2tm --expect-harmonization c2_tm_pif_v1 \
        --expect-transform-sha256 782eaef3e95c9dbe4b79868d936fd1e96ff1aa681d93ad609b5463ddd119db47 --out results/phase6/cube_validation_c2tm.json      # MUST: ok + complete
python3 scripts/phase5_pilot_vs_windows.py                                          # re-check bit-identity with the window cubes
python3 scripts/phase6_build_feature_cube.py                                        # data/features/cube/pune_G30.zarr (strip-wise)
python3 src/data/validate_feature_cube.py                                           # schema, CRS, grid, coverage, features, missingness, S1, terrain, windows; report bound to the cube
python3 scripts/phase6_build_feature_cube.py --family landsat_c2tm --fallback-family landsat    # P4-C2@v2 treatment cube (fallback only for years with zero TM observations; checked)
python3 src/data/validate_feature_cube.py data/features/cube/pune_G30_landsat_c2tm.zarr --family landsat_c2tm
# NOT YET IMPLEMENTED (must be written first): district silver builder for arm A -> data/labels/pune_G30_district_silver + results/phase6/district_silver_validation.json
python3 scripts/phase5_prerun_checklists.py                                         # never overridden
```

The `phase6_*` builders (S1 strip driver, district terrain, feature cube and its validator) are part of Phase 6. Their status and tests are in `docs/phase6_execution_status.md`; the S1 strip driver has no automated test. Still to be written: the district silver builder and the confirmatory runner (§3b of the execution status).

## 5. What would invalidate the run

* `validate_district_cube.py --require min` not returning ok and complete. The cube is then incomplete, whatever was built.
* The bit-identity check against the window composites failing on any overlap.
* Any product whose `metadata.json` hashes do not match its files.
* Mixing in Route-B (GEE) products without a separate equivalence study.
