# Phase 7 — district cube build report

_Generated 2026-10-03T01:22:45Z by `scripts/write_phase7_docs.py`; git `73e2ebc`._

## STATUS: **PILOT (not complete)** — last validation (`results/phase5/cube_validation/validation_report_min.json`, Phase 5): ok = True, complete_for_required = **False** (4/72 products). The four dry-season pilot composites are gitignored and are not present in the restored repository (0 products on disk). **STOP: C1 cannot run.**

## Compute environment (Stage 5)

| item | finding |
|---|---|
| required (D3 Route A) | 8 vCPU, 32 GB RAM, >= 200 GB SSD; Planetary Computer; Azure West Europe recommended |
| this_sandbox | {"cpus": 2, "mem_total": "7 GB", "free_disk": "29G", "memory_limit_note": "6 GB cgroup (OOM kills recorded in Phases 5-6)"} |
| cloud_credentials | none |
| azure_cli | not installed / no credentials |
| linked_computer | macOS arm64 laptop (MacBook Air, desktop assistant app), no folder connected (observed 2026-10-02T18:56Z via device info) - not the D3 machine |
| verdict | no Route-A environment reachable from this session; no credentials were invented; compute boundary stands |

The processing engine was **not** changed because the sandbox cannot run it (D3 Route A stands). No credentials were invented.

## Products (Stage 6-7)

| component | state | where |
|---|---|---|
| Landsat standard family (18 years x 4 periods) | 4/72 (dry-season 1990/2000/2010/2020 pilot) | Route A |
| C2 treatment family `landsat_c2tm` (11 TM years x 4) | 0/44 | Route A |
| Sentinel-1 RTC 2020 | NOT BUILT | Route A (`scripts/phase6_district_s1.py`) |
| district terrain | OOM_KILLED | Route A (> 6 GB RAM) |
| feature cube `pune_G30.zarr` (+ treatment cube) | NOT BUILT | after the products |
| **district automated-label set (silver L1, arm A)** | **BUILT + FROZEN** (manifest v1, SHA-256 `32e1ddb07982a198…`) | this session, strip-wise, same rule/code as the windows |

### District silver (Stage 7) — provenance

* Rule: WorldCover 2020 + 2021 and Esri 2020 + 2021 unanimous, L1 purity >= 0.78 (`configs/labels.yaml silver_consensus`), from the 10 m products on the nested 10 m lattice; code `src/pune_eo/labels/district_silver.py` (L1 rule identical to `labels/silver.py`; window fractions re-used unchanged).
* Coverage: 8155297 labelled cells = 46.9 % of the district; counts {'built_up': 383674, 'agriculture': 3622320, 'natural_vegetation': 3706418, 'water': 442783, 'bare_sparse': 102, 'other': 0}. Cells outside the district = 0; no gold, T1 or model output used.
* Restoration (2026-10-03): rebuilt after the session container was reclaimed - the label raster is **byte-identical** to the frozen original (SHA-256 `32e1ddb0…`), with identical overlap agreements (`results/phase7/restore/silver_rebuild_proof.json`); the window references were regenerated with the original L1 class counts (`results/phase7/restore/window_silver_restore.json`).
* Validation: window extents reproduced **exactly** by the district code (all three windows); overlap agreement with the window silver 0.9968 / 0.9965 / 0.9993. A first strict-equality check failed for this reason (evidence, transcribed from the session record after the container loss: `results/phase7/district_silver_validation_attempt1_strict_equality.json`): GDAL's approximate warp transformer picks nearest source pixels slightly differently for different output extents (row strips vs window extents). The criterion was revised to exact window-extent reproduction + >= 0.99 overlap agreement **before** any label existed and is disclosed here.
* Arm restrictions (training-eligible, >= 2 km from tuning blocks) are applied by the runner (`train_eligible_T1_30m.tif`), not baked into the product.

## Exact commands on the Route-A machine

**Correction to `docs/phase6_D3_compute_decision.md` §4:** do NOT re-run `scripts/phase4_validation_design.py` - it rewrites the pinned gold key, forms and design. Use `scripts/phase7_restore_spatial_context.py`, which regenerates only the validation-block / eligibility rasters and proves them against the frozen values.

```bash
python3 scripts/phase4_products_acquire.py                 # district Tier-B/C products (gitignored; GLC_FCS30D is needed by arm C)
python3 scripts/phase7_restore_spatial_context.py          # validation blocks + training eligibility, verified; never touches the key/design
# (district silver: already built and frozen - byte-identical rebuild proven; do not rebuild)
Y=1990,1991,1992,1998,1999,2000,2001,2002,2008,2009,2010,2011,2012,2018,2019,2020,2021,2022
python3 scripts/phase5_district_cube_pilot.py --years $Y --periods annual,dry,post_monsoon,wet --threads 8 --min-strips 8
python3 scripts/phase5_district_cube_pilot.py --years 1990,1991,1992,1998,1999,2000,2001,2008,2009,2010,2011 --periods annual,dry,post_monsoon,wet --transform c2_tm_pif_v1 --out-family landsat_c2tm --threads 8 --min-strips 8
python3 scripts/phase6_district_s1.py --year 2020 --threads 8
python3 scripts/phase6_district_terrain.py
python3 src/data/validate_district_cube.py --require min --out results/phase5/cube_validation/validation_report_min.json      # MUST be ok + complete
python3 src/data/validate_district_cube.py --family landsat_c2tm --require c2tm --expect-harmonization c2_tm_pif_v1 --expect-transform-sha256 782eaef3e95c9dbe4b79868d936fd1e96ff1aa681d93ad609b5463ddd119db47 --out results/phase6/cube_validation_c2tm.json
python3 scripts/phase5_pilot_vs_windows.py
python3 scripts/phase6_build_feature_cube.py && python3 src/data/validate_feature_cube.py
python3 scripts/phase6_build_feature_cube.py --family landsat_c2tm --fallback-family landsat && python3 src/data/validate_feature_cube.py data/features/cube/pune_G30_landsat_c2tm.zarr --family landsat_c2tm
python3 scripts/phase5_prerun_checklists.py && python3 scripts/phase7_run_confirmatory.py --preflight
```
