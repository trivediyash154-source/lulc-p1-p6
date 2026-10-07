# RESEARCH STATUS

_As of archival date: October 2026_

## Current Phase: 7 (NOT_READY)

## Gate Status

| Gate | Status | Evidence |
|------|--------|----------|
| G1 Data foundation | PARTIAL | 4/72 registered products built; pilot validated |
| G2 Human validation | FAIL | 0 Tier-A gold labels |
| G3 Existing-product benchmark | PARTIAL | 7 products + JRC compared; DW/GAIA need EE |
| G4 Training-label quality | FAIL | No gold, no T1, no district silver builder |
| G5 Historical validity | FAIL | No admissible reconstruction |
| G6 Spatial generalisation | FAIL | No gold |
| G7 Uncertainty | FAIL | No gold |
| G8 Confirmatory experiment | PARTIAL | Pre-registration intact; 0/6 executed |
| G9 District scale | FAIL | Requires G1, G5, G6, G7 |

## Blocking Dependencies

1. **Human Tier-A gold labels** (641 points × 2020 first, then 2000) — requires two independent interpreters with Google Earth Pro historical imagery
2. **T1 training labels** (699 points) — requires two independent interpreters
3. **District 30 m feature cube** — requires a machine with >6 GB RAM and >50 GB disk (Route A on Planetary Computer)
4. **D1 countersignature** — given by Arush (2026-10-02T18:49:28Z)

## Confirmatory Records

| Record | Status | Needs |
|--------|--------|-------|
| P4-C1@v2 (label quality) | PENDING | Gold, T1, cube |
| P4-C2@v2 (cross-sensor transform) | PENDING | Gold, cube |
| P4-C3@v2 (era-aware model) | PENDING | Gold, cube |
| P4-C4@v2 (LORO = G6) | PENDING | Gold, cube |
| P4-C5@v2 (uncertainty = G7) | PENDING | Gold, cube |
| P4-C6@v2 (optical+SAR) | PENDING | Gold, cube, S1 |

## Next Actions (Priority Order)

1. Two independent interpreters label 641 gold points for 2020, then 2000
2. Two independent interpreters label 699 T1 points
3. Run Route A on a machine with sufficient resources to build the district cube
4. Researcher reviews/adopts runner specifications
5. Execute confirmatory experiments in registered order
