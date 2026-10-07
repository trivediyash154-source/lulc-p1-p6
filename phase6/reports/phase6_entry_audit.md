# Phase 6 — entry audit (state at the start of Phase 6, 2026-10-02)

This audit was checked against files and commands, not against earlier reports. It describes the committed state **before** any Phase-6 change: HEAD `21b422c`, working tree clean.

## 1. Current repository state

| item | finding |
|---|---|
| git | HEAD 21b422c ("Phase 5: provenance"); no uncommitted changes; 127 tests pass |
| human labels | `data/labels/gold/phase4/{raw_interpreter_A, raw_interpreter_B, adjudicated, metadata}` all **empty**; `data/labels/training/phase5_T1/raw_interpreter_A` empty; no uploads from the researcher |
| district cube | `data/composites/pune_G30/{1990,2000,2010,2020}/landsat/seasonal/dry/` only (Phase-5 pilot); no annual, post-monsoon or wet composites; no S1; no district terrain; no `data/features/cube/pune_G30.zarr` |
| window cubes | 3 zarr cubes (khadakwasla_mutha, baramati_agri, mulshi_ghats) |
| compute here | 2 CPUs, a 6 GB memory cgroup (OOM kills recorded), ≈ 13 GB free disk |

## 2. Current protocol hashes

| item | value | check |
|---|---|---|
| evaluation code | `3bb40f9c982d601db88f75913f7d00acb4944136a3f9771079b9413ec31a14ab` | equals frozen |
| protocol YAML (minus frozen block) | `fe1fad6f8b8eda1618302d6886ab8d90fbb65aefdf6da17f961aee93144cbdbe` | equals frozen |
| `check_confirmatory_records()` | ok = true | 6 active records |

## 3. Current six confirmatory records

| record | status | family | byte-identical to 5b9f947 (Phase-4 registration) |
|---|---|---|---|
| P4-C1@v2 | PENDING | label_quality | yes |
| P4-C2@v2 | PENDING | historical | yes |
| P4-C3@v2 | PENDING | historical | yes |
| P4-C4@v2 | PENDING | spatial | yes |
| P4-C5@v2 | PENDING | uncertainty | yes |
| P4-C6@v2 | PENDING | spatial | yes |

Stale text inside the records (kept unchanged; history is not rewritten):

* `needs: "human interpretation (G2) + Earth Engine export"` — Earth Engine is not required (Phase 5).
* `dataset_version: "district cube v1 (to be built)"` — still true.

Pre-run checklists at entry: 6/6 **DO NOT RUN** (C1 12 failed items, C2 9, C3 10, C4 7, C5 8, C6 8).

## 4. Current blockers

1. **Tier-A gold: 0 labels** — blocks every record and G2, G4-G7, G9.
2. **District cube**: not built beyond the dry-season pilot — blocks the feature set of every record and G1.
3. **District S1 2020** (P4-C6), **district terrain** (all records), **district silver** (P4-C1 arm A): not built.
4. **D1 open**: P4-C1@v2 arms C-E and the tuning split were not operational (PROPOSED addendum only).
5. **D2 open**: the P4-C2@v2 transform was "published OR PIF-fitted", an open fork.
6. **D3 open**: no compute route chosen.
7. **T1 labels: 0**.

## 5. Current human-label kits

| kit | delivered | problem found at entry |
|---|---|---|
| `outputs/phase4/phase4_gold_kit_INTERPRETER_A.zip` | Phase 4 | contains the full `phase4_gold_protocol.md`, which describes the strata and names the products used to stratify (general design information an interpreter should not see); no metadata template |
| `outputs/phase4/phase4_gold_kit_INTERPRETER_B.zip` | Phase 4 | as A, and its KML contains all 641 points instead of B's 160 |
| `outputs/phase5/phase5_T1_kit_INTERPRETER_{A,B}.zip` | Phase 5 | same full protocol; T1 v1 had no spatial separation between train and tuning points; arms not yet approved |

→ Superseded in Phase 6 by blind kits that contain only the form, a role-specific KML, `docs/phase6_interpreter_protocol.md` and a metadata template (`scripts/phase6_make_kits.py`, `results/phase6/kit_manifest.json`).

## 6. Current district-cube state

| component | state |
|---|---|
| Landsat dry 1990/2000/2010/2020 | built, validated (`validate_district_cube.py`: ok), bit-identical to the window composites on overlap |
| registered minimum (18 years × 4 periods) | 68 of 72 products missing (`validation_report_min.json`: ok = true, complete_for_required = **false**) |
| C2 treatment family | not defined (D2 open) |
| S1 2020 / terrain / feature cube | absent; no district-size S1 writer, no district terrain mode, no strip-wise feature-cube builder existed |

## 7. Current unresolved researcher decisions

| id | decision | where |
|---|---|---|
| D1 | approve / correct the P4-C1@v2 addendum (arms A-E, tuning split, T1) | `experiments/registry4/addenda/P4-C1_at_v2_addendum_1_PROPOSED.json` |
| D2 | fix the P4-C2@v2 cross-sensor transform before any gold result | record P4-C2@v2 |
| D3 | compute route for the full cube | `docs/phase5_execution_plan.md` |

## 8. Exact actions required to reach RUN status (all six records)

1. Resolve D1, D2 and D3 (Phase 6: `docs/phase6_D1_review.md`, `docs/phase6_D2_sensor_correction_decision.md`, `docs/phase6_D3_compute_decision.md`).
2. Human gold for 2020 + 2000 (A: 641, B: 160) → freeze → ingest → `audit_gold.py` with G2 = PASS.
3. Gold for 2010 and 1990: C1 and C3 evaluate those epochs; C2 evaluates 1990 and 2000. The checklist requires ≥ 80 % of points labelled (including `unavailable`) in every epoch a record uses.
4. T1 v2 labels for 2020 + 2000 → freeze → ingest (`--kind training`) → `audit_gold.py --t1` = PASS (P4-C1@v2).
5. On the Route-A machine:
   * the full Landsat cube (18 years × 4 periods) and the C2 treatment family (11 TM years);
   * S1 2020, district terrain and district silver;
   * `validate_district_cube.py --require min` → ok + complete;
   * the district feature cube → `validate_feature_cube.py` ok.
6. `python3 scripts/phase5_prerun_checklists.py` → RUN for P4-C1@v2. Run C1 once; selection on the T1 tuning split.
7. Re-run the checklists → RUN for C2-C6 (each needs C1 completed). Run each once; BH within families.
