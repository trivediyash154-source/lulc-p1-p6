# Phase 6 — readiness gate

_Generated 2026-10-02T18:19:02Z by `scripts/write_phase6_docs.py`. Framework G1-G9 of protocol v2 (unchanged)._

| gate | STATUS | EVIDENCE | BLOCKER | NEXT ACTION |
|---|---|---|---|---|
| G1 Data foundation | **PARTIAL** | 4 dry-season district epochs validated and bit-identical to the windows; registered minimum 4/72 products; district terrain attempted here and OOM-killed; feature cube not built | Route A not executed (compute boundary) | run the Route-A command block (docs/phase6_D3_compute_decision.md §4) |
| G2 Human validation | **FAIL** | gold_audit.json: 0 Tier-A labels | no human interpretation yet | hand phase6_gold_kit_INTERPRETER_A/B to two independent interpreters (2020, then 2000) |
| G3 Existing-product benchmark | **PARTIAL** | unchanged from Phase 5 (7 products + JRC; DW/GAIA need Earth Engine) | Dynamic World, GAIA; ALOS FNF and MS buildings not acquired | optional; does not block confirmatory runs |
| G4 Training-label quality | **FAIL** | P4-C1@v2 DO NOT RUN (11 items); D1 resolved | gold, T1 labels, D1 countersignature, district silver (builder not yet written), feature cube, confirmatory runner | researcher countersigns D1; T1 interpretation (phase6_T1v2 kits); Route A; silver builder + runner |
| G5 Historical validity | **FAIL** | no admissible reconstruction (none allowed before C1-C3); envelope frozen (P5-X3); D2 fixed | gold 1990/2000/2010/2020, C1-C3 | after G2 and G1 |
| G6 Spatial generalisation | **FAIL** | P4-C4@v2 DO NOT RUN | gold, cube, C1 selection | after G2 and G1 |
| G7 Uncertainty | **FAIL** | P4-C5@v2 DO NOT RUN | gold | after G2 |
| G8 Confirmatory experiment | **PARTIAL** | check_confirmatory_records ok = True; hashes unchanged; addenda registered before any data; 0/6 executed | all prerequisites | re-run the checklists; run each record once when RUN |
| G9 District scale | **FAIL** | requires G1, G5, G6, G7 | G1, G5, G6, G7 | - |

## FINAL STATUS: **NOT_READY**

The hypotheses still cannot be tested. The project stopped at two boundaries:

1. **Human-label boundary:** 0 Tier-A gold labels and 0 T1 labels.
2. **Compute boundary:** the registered district cube needs the Route-A machine.

Every decision that could be taken without data has been taken and registered (D1, D2, D3), pending the researcher's countersignature of D1 and D2. Label ingestion, the cube/S1/terrain/feature-cube builders and the validators are built; most are tested on synthetic data (the S1 strip driver is not). Still to be written: the district silver builder and the confirmatory runner (`docs/phase6_execution_status.md` §3b). District terrain was attempted here and OOM-killed.

Single next action: **two independent human interpreters label the 641 gold points for 2020 (then 2000) using `phase6_gold_kit_INTERPRETER_A/B.zip`.** In parallel, the Route-A machine builds the cube.