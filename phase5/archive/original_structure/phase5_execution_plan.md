# Phase 5: execution plan

Principle: execute only what is unblocked; stop at the dependency boundary.

* The frozen protocol v2 is unchanged. Code hash `3bb40f9c982d…` and YAML hash `fe1fad6f8b8e…` were re-verified on 2026-10-02.
* No confirmatory record has run. Every check is in `results/phase5/confirmatory/prerun_checklists.json`, and every record reads **DO NOT RUN**.

## 1. Critical path

HUMAN GOLD + DISTRICT 30 m CUBE → frozen confirmatory tests (P4-C1@v2 first; selection on its tuning split) → historical validity (G5) → spatial generalisation (G6) → uncertainty (G7) → readiness gate → district inference (only if G1, G5, G6, G7 pass). The dependency graph is §H of `docs/phase5_entry_audit.md`.

## 2. Executed in Phase 5 (unblocked)

| work | output | registry |
|---|---|---|
| Entry audit, with independent machine checks | `docs/phase5_entry_audit.md`, `results/phase5/entry/entry_checks.json` | — |
| Gold + T1 ingestion, freeze and audit pipeline; G2 evaluator (revision 2 after independent review) | `src/validation/`, `tests/test_gold_ingestion.py` (26 tests), `docs/phase5_gold_ingestion.md` | — |
| District cube spec, validator, static GEE check, aligned GEE script | `docs/phase5_district_cube_spec.md`, `src/data/validate_district_cube.py` (10 tests), `results/phase5/cube_validation/` | — |
| District 30 m key-epoch pilot (Landsat dry 1990, 2000, 2010, 2020; Planetary Computer; repository engine); bit-identical to the window composites on overlap | `data/composites/pune_G30/…` (regenerable), `results/phase5/cube_pilot/`, `results/phase5/cube_validation/pilot_vs_window_composites.json` | data product |
| Memory-lean district writer, tested equivalent to the engine writer | `src/pune_eo/compositing/lean_writer.py`, `tests/test_lean_writer.py` | — |
| Observation support 1990-2026 (G1 temporal-resolution rule) | `results/phase5/observation_support/` | P5-X1 |
| Product disagreement by class boundary and settlement context | `results/phase5/benchmark/disagreement_descriptors_p5x2.json` | P5-X2 |
| G5 built-up reference envelope frozen before any district prediction | `results/phase5/benchmark/g5_built_envelope_frozen.json` (+ .sha256) | P5-X3 |
| Near-coincident cross-sensor pairs on PIFs (sensor vs land change); post-hoc order split and scene-cluster CIs | `results/phase5/sensor_pairs/` | P5-E2 (+ post-hoc, labelled) |
| Human training sample T1 design (needed by P4-C1@v2 arms C-E) | `data/labels/training/phase5_T1/` | addendum (PROPOSED) |
| Pre-run checklists P4-C1..C6@v2 | `scripts/phase5_prerun_checklists.py`, `results/phase5/confirmatory/prerun_checklists.json` | — |

## 3. Decisions only the researcher can take (they block confirmatory runs)

| # | decision | why it is needed | where |
|---|---|---|---|
| D1 | Approve or modify **P4-C1@v2 addendum 1**: T1 sample, tuning-block rule, operational arms A-E (recommended: C = silver ∩ GLC_FCS30D agreement, D = T1 human, E = D ∪ C) | Arms C-E were named but never defined, and no training sample or tuning split existed. These gaps were found before any data existed. | `experiments/registry4/addenda/P4-C1_at_v2_addendum_1_PROPOSED.json` |
| D2 | Fix the **P4-C2@v2 transform** before the run: published TM/ETM+→OLI coefficients, **or** a PIF fit on non-validation blocks (P5-E2 provides near-coincident PIF pairs) | The record allows two alternatives; choosing after seeing gold results would be a forking path | registry record P4-C2@v2 |
| D3 | **Compute route** for the full cube: a VM running the repository engine (reference, Planetary Computer, no credentials), or an Earth Engine project (GEE script; documented differences) | The full cube needs about 22 wall-clock hours on 2 CPUs (≈ 40-45 CPU-hours; estimate from the pilot, §5) and about 50 GB of disk — beyond this sandbox | `docs/phase5_district_cube_spec.md` |

## 4. Human workstreams (cannot be done by Claude; no substitute will be produced)

| pass | content | interpretations | effort at 1-2 min each (planning estimate, not a result) |
|---|---|---|---|
| Gold 2020 + 2000 (priority) | 641 points × 2 epochs (A) + 160 × 2 (B) | 1 602 | 27-53 h |
| Gold 2010 + 1990 | the same | 1 602 | 27-53 h (many 1990 points will be legitimately `unavailable`) |
| T1 2020 + 2000 (after D1) | 635 × 2 (A) + 127 × 2 (B) | 1 524 | 25-51 h |
| Adjudication | definite A/B disagreements | — | third reviewer |

Kits: `data/labels/gold/phase4/blind/` (forms + KML) and `data/labels/training/phase5_T1/blind/`. Interpreters receive only these folders, never the repository (the key is in it).

After each pass:

1. Freeze it: `python3 src/validation/ingest_gold.py freeze <pass> --by <name>`.
2. Ingest it: `python3 src/validation/ingest_gold.py ingest`. If a violation needs a change to a returned form, the interpreter makes the correction and signs a correction record (`metadata/correction_*.json`: file, previous and new SHA-256, interpreter id, reason, date, `signed_by_interpreter: true`). Only then may the file be frozen again under a new pass id. A re-freeze without a signed record is refused (`RAW_REFROZEN`).
3. Audit it: `python3 src/validation/audit_gold.py` (G2).
4. T1 uses the same commands with `--kind training` (ingest) and `python3 src/validation/audit_gold.py --t1`; training-leakage rules apply.

## 5. Compute workstream

1. **Full district Landsat cube.**
   * Command: `python3 scripts/phase5_district_cube_pilot.py --years <list> --periods annual,dry,post_monsoon,wet --threads <n> --min-strips 4`.
   * Minimum for the registered features (B3 + 5-year window t−2…t+2): 1990-1992, 1998-2002, 2008-2012, 2018-2022, all four periods.
   * Then run `python3 src/data/validate_district_cube.py --require min --out results/phase5/cube_validation/validation_report_min.json`. It must return `ok` and `complete_for_required`.
2. **District feature cube** `data/features/cube/pune_G30.zarr` (B3 + 5-year window + terrain).
   * `feature_engineering/cube.py` was written for windows. At district size it needs the same strip-wise treatment as the composites; this is still to be done.
3. **Sentinel-1 RTC 2020** on the district grid for P4-C6@v2, via the repository engine. GEE GRD is not equivalent.
4. **District silver (arm A).**
   * Source: WorldCover 2020/2021 and Esri 2020/2021 at 10 m, giving purity ≥ 0.78.
   * Restricted to training-eligible cells.
5. Re-run `python3 scripts/phase5_prerun_checklists.py`. A record may run only when its decision is RUN.

## 6. Confirmatory order (unchanged from protocol v2)

1. **P4-C1@v2**: arms A-E × (RF, TempCNN-class) × 5 seeds. Evaluate on gold at 2020, 2010, 2000 and 1990.
2. **Selection rule.** Take the arm/model with the highest seed-mean macro-F1 on the T1/silver **tuning** split. Gold is never consulted for this.
3. **P4-C2@v2 … C6@v2**, each run once.
4. **Benjamini-Hochberg** within each family.
5. **Gates G4-G7**, then G9.

## 7. Stop rules

* G2 fails → no confirmatory run.
* Cube validation fails → fix the pipeline, rebuild and re-validate. Never patch the outputs.
* A genuine implementation bug in frozen code → stop the affected experiment, document whether the bug predates execution, and create a new protocol version only if scientifically necessary. Superseded records stay.
* A negative or failed result is reported as it is.
* Registry limitation (frozen code, cannot be changed): `protocol.register()` can overwrite a PENDING record and keep its first `registered_utc`. A timestamp ordering alone therefore does not prove that a specification came first. The git history of `experiments/registry4/` is the stronger record; registry files are committed immediately after registration.
* `protocol.Run` (frozen) does not call the pre-run checklist. Every confirmatory runner script must call `assert_runnable()` first (`tests/test_phase5_guards.py`).

## 8. Not done, by design

* No new ML models.
* No district-wide historical maps or annual 1990-2026 record.
* No scenarios and no digital twin.
* No silver-as-gold and no AI or product labels in Tier A.
* No change to protocol v2.
* No use of P5 exploratory results as confirmatory evidence.
