# Phase 4 — District Scaling Design

## Purpose

Design the complete infrastructure for district-wide scientific inference: freeze the evaluation protocol, design the gold validation sample, benchmark existing products, pre-register confirmatory experiments, and assess readiness for district scaling.

## Research Questions

- What is the state of existing products over Pune district?
- How should the gold validation sample be designed?
- What confirmatory experiments are needed to establish district-wide validity?
- Is there evidence of sensor-era effects on pseudo-invariant targets?

## Inputs

- Phase 1–3 complete archive
- Existing products: GLC_FCS30D, GISA, WSF, GHSL, ESA CCI, JRC, Esri, WorldCover
- Phase 3 gate framework (G1–G6)

## Methods

- Complete data inventory and quality audit
- Training and validation label audit
- Evaluation protocol design and unit testing (v1 → v2, frozen before confirmatory runs)
- District validation design: 641 points in 125 independent 6 km blocks, 13 strata, outside Phase 3 windows
- Existing product benchmark: area comparison, trajectory comparison, consensus/disagreement mapping
- Pseudo-invariant target sensor-era analysis

## Experiments

| ID | Kind | Status | Description |
|----|------|--------|-------------|
| P4-C1@v2 | confirmatory | PENDING | Training-label quality and temporal design |
| P4-C2@v2 | confirmatory | PENDING | Cross-sensor TM→OLI transform |
| P4-C3@v2 | confirmatory | PENDING | Era-aware model |
| P4-C4@v2 | confirmatory | PENDING | Leave-one-region-out (G6) |
| P4-C5@v2 | confirmatory | PENDING | Uncertainty/calibration (G7) |
| P4-C6@v2 | confirmatory | PENDING | Optical+SAR |
| P4-E1 | exploratory | PASS | Sensor-era PIF contrasts (60/77 differ) |
| P4-X1 | exploratory | PASS | Product agreement (60% three-way) |
| P4-X2 | exploratory | PASS | P3-F3 vs GLC_FCS30D by epoch |
| P4-X3 | exploratory | PASS | District disagreement by strata |

## Results

- Products disagree by up to 4x on built-up area in 2020
- 1990 built-up agreement F1: 0.32–0.43 between products
- All three land-cover products agree on 60% of district cells
- Disagreement highest on 3–8° slopes, transition rainfall zone, near water
- L5-dominated years show higher red and lower NDVI (confounded with time)
- 641-point gold design with independent blocks outside Phase 3 windows

## Supported Findings

- Product disagreement is systematic and landscape-dependent
- Sensor-era differences exist on pseudo-invariant targets

## Unsupported / Rejected Findings

Not documented in the supplied Phase 4 archive (all confirmatory experiments are PENDING).

## Limitations

- All confirmatory experiments blocked on human gold and district cube
- Product benchmark is exploratory (not confirmatory)
- Sensor-era analysis confounded with time (few years per regime)

## Important Decisions

- Protocol v2 frozen before any confirmatory run (superseding v1)
- Gold design v2 (641 pts) superseding v1 (672 pts)
- Earth Engine export pipeline written (but district cube not built)

## Protocol Status

Protocol v2 frozen. Code hash: `3bb40f9c...`. YAML hash: `fe1fad6f...`. Unchanged through Phase 7.

## Key Artifacts

| File | Description |
|------|-------------|
| reports/phase4_final_report.md | Phase 4 final report |
| reports/phase4_audit.md | Entry audit with weakness matrix |
| protocols/phase4_readiness_gate.md | Readiness gate (NOT_READY) |
| data/phase4_core.zip | Core Phase 4 archive |
| data/phase4_gold_KEY_evaluator_only.zip | Gold sample key (evaluator only) |
| data/phase4_gold_kit_INTERPRETER_A.zip | Interpreter A kit (SUPERSEDED by Phase 6) |
| data/phase4_gold_kit_INTERPRETER_B.zip | Interpreter B kit (SUPERSEDED by Phase 6) |

## Dependencies

- **Upstream:** Phases 1–3 (data, models, gate framework)
- **Downstream:** Phases 5–7 (evaluation protocol, gold design, confirmatory framework)

## Reproducibility

Protocol v2 is unit-tested (15 tests). Registry refuses non-conforming confirmatory runs. Gold design is deterministic given the stratification layers.

## Historical Notes

Protocol v1 and gold design v1 are superseded but preserved. The Phase 4 interpreter kits were later found to be flawed (shipped full protocol, B kit had all 641 points) and superseded by Phase 6 kits.
