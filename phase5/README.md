# Phase 5 — Pilot Cube and Infrastructure

## Purpose

Build pilot district 30 m composites for key epochs, validate against window composites, assess observation support across the full temporal range, and extend the product benchmark. Determine readiness for the full district cube.

## Research Questions

- Can the compositing engine produce valid district-wide 30 m composites?
- Are the pilot composites consistent with the benchmark-window composites?
- What is the observation support at 30 m across 37 years?
- What statements about the earlier phases are overstated or stale?

## Inputs

- Phase 1 compositing engine
- Landsat scenes from Planetary Computer (anonymous access)
- Phase 4 gold design, protocol, existing products

## Methods

- District 30 m dry-season compositing for 1990, 2000, 2010, 2020
- Bit-identical comparison with window composites on overlap
- Observation-support analysis (n_clear, reliable fraction)
- Product benchmark extension (boundary/context analysis)

## Experiments

- P5-X1: Observation support at 30 m (reliable ≥ 3 obs: 0.958–1.000)
- P5-X2: Product benchmark — boundary/context stratification
- P5-X3: Built-up envelope analysis (frozen)
- P5-X4: Water benchmark including JRC

## Results

- 4 dry-season district epochs validated: bit-identical to windows on overlap
- Valid coverage ≥ 0.999999 across all epochs
- Reliable (≥ 3 observations): 0.958–1.000
- Earlier claims about needing Earth Engine credentials were overstated
- District terrain OOM-killed (6 GB sandbox limit)

## Supported Findings

- The compositing engine works at district scale on Planetary Computer
- Pre-2000 observations are sparse but sufficient for epoch-level claims

## Unsupported / Rejected Findings

Not documented in the supplied Phase 5 archive.

## Limitations

- Only 4 of 72 registered products built (dry season for 4 epochs)
- Full district feature cube requires ~50 GB disk and >6 GB RAM
- Terrain OOM-killed in the sandbox

## Important Decisions

- Cube specification formalized
- Overstated EE dependency corrected

## Protocol Status

Protocol v2 unchanged.

## Key Artifacts

| File | Description |
|------|-------------|
| reports/phase5_results.md | Phase 5 results |
| reports/phase5_execution_plan.md | Execution plan |
| protocols/phase5_readiness_gate.md | Readiness gate (NOT_READY) |
| protocols/phase5_entry_audit.md | Entry audit |
| figures/P5F2_pilot_nclear_maps.png | Pilot n_clear maps |
| figures/P5F5_sensor_pairs.png | Sensor pair analysis |
| data/phase5_core.zip | Core Phase 5 archive |
| data/phase5_pilot_cube_QA_layers_*.zip | Pilot QA layers |
| data/phase5_T1_*.zip | T1 sample kits (v1, superseded) |

## Dependencies

- **Upstream:** Phases 1–4
- **Downstream:** Phases 6–7

## Reproducibility

Pilot composites are reproducible given the same Planetary Computer scene catalog. Bit-identical comparison verified.

## Historical Notes

The Phase 5 entry audit identified several stale or overstated claims from earlier phases and documented corrections (see reports/phase5_entry_audit.md §D).
