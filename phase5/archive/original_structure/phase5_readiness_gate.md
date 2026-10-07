# Phase 5 — readiness gate

_Generated 2026-10-02T16:52:52Z by `scripts/write_phase5_docs.py` from saved results. Framework G1-G9 of protocol v2 (unchanged)._

| gate | STATUS | EVIDENCE | BLOCKER | NEXT ACTION |
|---|---|---|---|---|
| G1 Data foundation | **PARTIAL** | 30 m district dry-season composites for 1990, 2000, 2010, 2020 (valid ≥ 0.999999, reliable ≥ 3 obs 0.958-1.000; validator ok; bit-identical to window composites on overlap); observation-support table P5-X1 | registered feature cube (annual/dry/post/wet for t−2..t+2 around 1990/2000/2010/2020, feature cube, S1 2020) not built: compute (estimate ~22 h wall-clock on 2 CPUs, i.e. ~40-45 CPU-h) and ~50 GB disk | decision D3; run route A on a VM (or GEE route B); `validate_district_cube.py --require min` must return ok |
| G2 Human validation | **FAIL** | results/phase5/gold/gold_audit.json: 0 Tier-A labels; ingest FAIL (NO_INTERPRETER_A) | human interpretation | interpreters A and B: 2020 then 2000 (kits in data/labels/gold/phase4/blind/); freeze; ingest; audit |
| G3 Existing-product benchmark | **PARTIAL** | P4-X1/X2/X3 + P5-X2 (boundary/context) + P5-X3 (envelope) + P5-X4 (water incl. JRC): 7 land-cover/built products + JRC compared with documented mappings and disagreement maps | Dynamic World, GAIA need Earth Engine; ALOS FNF and MS building footprints not acquired | Earth Engine project (GEE route) or acquire ALOS FNF (Planetary Computer) |
| G4 Training-label quality | **FAIL** | P4-C1@v2 pre-run checklist: DO NOT RUN (12 items) | gold; T1 human training labels; addendum approval (D1); district silver; district feature cube | decision D1, then T1 interpretation and district silver |
| G5 Historical validity | **FAIL** | no reconstruction evaluated (none allowed before gold); P5-X3 envelope frozen; P5-X1 supports epoch-level rather than annual claims | gold at 1990/2000/2010/2020; P4-C1..C3@v2 | gold passes for 2010 and 1990 after 2020/2000 |
| G6 Spatial generalisation | **FAIL** | P4-C4@v2 DO NOT RUN | gold, district cube, P4-C1@v2 selection | after G2 and G1 |
| G7 Uncertainty | **FAIL** | P4-C5@v2 DO NOT RUN; Phase-3 calibration on Tier C is not admissible for G7 (Phase 4 listed G7 as PARTIAL on that basis; Phase 5 applies the gate text 'on gold' strictly) | gold | after G2 |
| G8 Confirmatory experiment | **PARTIAL** | protocol.check_confirmatory_records() ok; code/YAML hashes unchanged; 0 of 6 confirmatory records executed; pre-run checklists in results/phase5/confirmatory/ | all prerequisites above | re-run scripts/phase5_prerun_checklists.py; run each record once when RUN |
| G9 District scale | **FAIL** | requires G1, G5, G6, G7 | G1, G5, G6, G7 | — |

## FINAL DECISION: **NOT_READY**

District-wide scientific inference is not supported. No gate passed on evidence; G1 and G3 are PARTIAL; G8 is PARTIAL (integrity of the pre-registration only).

Single highest-priority action: **Tier-A human interpretation of the 641 district gold points for 2020 and 2000 by two independent interpreters (160 double), then freeze → ingest → audit (G2).** Without it no confirmatory test, no historical-validity test and no uncertainty validation can run, whatever compute is available.