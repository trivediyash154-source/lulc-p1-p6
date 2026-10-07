# Phase 4 — readiness gate

| gate | status | evidence |
|---|---|---|
| G1 data foundation | FAIL | 30 m data cover 5.8 % of the district; district cube not built (export ready, needs Earth Engine); pre-2013 record sparse |
| G2 human validation | FAIL | 0 Tier-A labels; district design and blind kit ready |
| G3 existing-product benchmark | PARTIAL | GLC_FCS30D, GISA 2.0, WSF, GHSL, ESA CCI, Esri, WorldCover compared district-wide with documented class mapping and disagreement maps; NOT compared: JRC (used only for strata/PIFs), ALOS FNF, MS buildings (acquirable), Dynamic World and GAIA (need Earth Engine) |
| G4 training-label quality | FAIL | easy-cell bias and class gaps documented; label-quality family P4-C1@v2 pre-registered, needs gold |
| G5 historical validity | FAIL | no historical gold; the only reconstruction fails the whole-area test |
| G6 spatial generalisation | FAIL | 3 windows only; LORO design frozen (P4-C4@v2) |
| G7 uncertainty | PARTIAL | characterised in 3 windows on Tier C; not validated on gold (P4-C5@v2) |
| G8 confirmatory experiment | PASS (design) | protocol v2 frozen (code 3bb40f9c982d, protocol YAML fe1fad6f8b8e); automated check protocol.check_confirmatory_records(): ok=True for 6 active confirmatory records (all PENDING, none executed); v1 records superseded before execution |
| G9 district scale | FAIL | requires G1, G5, G6, G7 |

## FINAL DECISION: **NOT_READY**

Missing evidence, exactly:
1. Tier-A human gold on the district design (G2) — at minimum 2020 and 2000 for >= 80 % of points, >= 20 % double interpreted (the design provides 25 %).
2. District 30 m cube 1990-2026 (G1, G9) — run `scripts/gee/export_district_cube.py` with an Earth Engine project, then inventory it.
3. Confirmatory results P4-C1@v2..C6@v2 on gold (G4-G7), run once with the frozen protocol v2.
4. Historical reconstruction passing the whole-area test inside the independent-product envelope (G5).

## Against the standard 'would a skeptical reviewer believe it?'

| component | assessment |
|---|---|
| valid data | partly: archive and provenance sound; district 30 m data absent; pre-2013 sparse; residual sensor differences |
| valid labels | no: no human labels; silver labels biased to easy cells |
| independent validation | no: Tier B/C only |
| spatial generalisation | no: 3 windows |
| temporal generalisation | no: historical failure |
| uncertainty | partly characterised |
| reproducibility | yes: registry, frozen protocol, unit tests, inventory |
| scientific interpretation | partly: negative results reported; candidate causes examined only in exploratory analyses (none tested confirmatorily) |
