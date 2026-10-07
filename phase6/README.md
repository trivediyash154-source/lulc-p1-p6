# Phase 6 — Gold Workflow and Decisions

## Purpose

Take all remaining decisions that can be made without data. Design and build the gold and T1 interpreter kits. Formalize the human gold workflow end-to-end. Address sensor-correction and compute-route decisions.

## Research Questions

- D1: Is the P4-C1@v2 addendum scientifically coherent? Does it introduce leakage?
- D2: Should the cross-sensor TM correction use published Roy 2016 coefficients or local PIF-fitted ones?
- D3: Should the district cube be built via Route A (repository engine) or Route B (Earth Engine)?
- How should the human gold interpretation workflow be structured?

## Inputs

- Phase 4 gold design v2 (641 points, 125 blocks)
- Phase 4 protocol v2
- Phase 5 cube specification and pilot validation
- P4-C1@v2 addendum proposal

## Methods

- D1: Independent review of addendum; 7 corrections identified and applied
- D2: Comparison of Roy 2016 (CONUS, LEDAPS) vs local PIF RMA (Pune, Collection 2)
- D3: Route A vs Route B comparison on 8 criteria
- T1 v2 redesign with train/tuning separation (2 km buffer)
- Gold interpreter kit redesign (fixing Phase 4/5 kit flaws)

## Experiments

No numbered experiments in Phase 6. This was a decision and design phase.

## Results

- **D1:** APPROVED WITH CORRECTION — hidden model-selection advantage found and corrected; 7 corrections total
- **D2:** Local PIF RMA selected — internal consistency for mixed TM/ETM+ epochs
- **D3:** Route A selected — only tested path; bit-identical to windows
- T1 v2 designed: 699 points (525 train, 174 tuning), 140 double-interpretation
- Phase 6 gold kits created (superseding Phase 4/5 kits)
- Phase 6 T1v2 kits created
- 0 labels obtained

## Supported Findings

- D1 addendum is scientifically coherent after corrections
- Route A is the only reproducible path (bit-identical on overlap)
- Phase 4/5 kits had design information leakage (B kit had all 641 points)

## Unsupported / Rejected Findings

Not documented in the supplied Phase 6 archive.

## Limitations

- 0 Tier-A gold labels and 0 T1 labels
- District terrain OOM-killed again
- Compute boundary prevents cube construction

## Important Decisions

- D1: Addendum approved with correction (countersigned by Arush 2026-10-02)
- D2: Local PIF RMA over Roy 2016 (countersigned by Arush 2026-10-02)
- D3: Route A (repository engine on Planetary Computer)
- T1 v1 superseded by T1 v2

## Protocol Status

Protocol v2 unchanged. No confirmatory run attempted.

## Key Artifacts

| File | Description |
|------|-------------|
| reports/phase6_execution_status.md | Execution status |
| reports/phase6_human_gold_workflow.md | Complete gold workflow |
| reports/phase6_D1_review.md | D1 decision review |
| protocols/phase6_D2_sensor_correction_decision.md | D2 decision |
| protocols/phase6_D3_compute_decision.md | D3 decision |
| protocols/phase6_T1_training_protocol.md | T1 training protocol |
| protocols/phase6_readiness_gate.md | Readiness gate (NOT_READY) |
| data/phase6_gold_kit_INTERPRETER_A.zip | Gold kit for Interpreter A |
| data/phase6_gold_kit_INTERPRETER_B.zip | Gold kit for Interpreter B |
| data/phase6_T1v2_kit_INTERPRETER_A.zip | T1v2 kit for Interpreter A |
| data/phase6_T1v2_kit_INTERPRETER_B.zip | T1v2 kit for Interpreter B |
| data/phase6_KEYS_evaluator_only.zip | Evaluator-only keys |
| data/phase6_repository_snapshot.zip | Repository snapshot |

## Dependencies

- **Upstream:** Phases 1–5
- **Downstream:** Phase 7 (runner implementation)

## Reproducibility

Decisions are documented with full reasoning. Kits are deterministic given the sample design. Countersignature recorded with timestamp.

## Historical Notes

The Phase 6 session discovered that Phase 4/5 interpreter kits had a design flaw: the B kit contained KML for all 641 points (should have been only the 160 double-interpretation points), and the full gold protocol was shipped (naming strata and products). Phase 6 kits correct both issues.
