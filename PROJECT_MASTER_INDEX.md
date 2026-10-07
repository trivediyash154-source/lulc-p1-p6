# PROJECT MASTER INDEX

## 1. What the PhD Project Is

A PhD research project in remote sensing / Earth observation, studying multi-decadal land use and land cover (LULC) change in the Pune district, Maharashtra, India, using satellite imagery from Landsat (1990–2026), Sentinel-2 (2018–2026), and Sentinel-1 SAR (2017–2025).

## 2. Overall Research Objective

To build a defensible, uncertainty-quantified, multi-decadal land-cover change record for the Pune district at 30 m resolution, validated against human-interpreted gold-standard labels, and to use this record to characterize urban expansion, water body dynamics, vegetation stress, and land-change trajectories.

## 3. Geographic Scope

- **District:** Pune district, Maharashtra, India
- **Benchmark windows (30 m, 5.8% of district):**
  - `pune_G30_khadakwasla_mutha` — urban–periurban corridor with reservoirs
  - `pune_G30_baramati_agri` — semi-arid agricultural plain
  - `pune_G30_mulshi_ghats` — Western Ghats forested/reservoir landscape
- **District-wide:** 240 m overview; 30 m pilot for 4 dry-season epochs (1990, 2000, 2010, 2020)

## 4. Temporal Scope

- **Landsat:** 1990–2026 (L5 TM, L7 ETM+, L8 OLI, L9 OLI-2)
- **Sentinel-2:** 2018–2026 (10–20 m, resampled to 30 m for fusion)
- **Sentinel-1 SAR:** 2017–2025 (C-band, VV/VH)
- **CHIRPS rainfall:** 1981–2026 (monthly, 5 km)
- **Reference products:** GHSL (1975–2030), WSF-Evolution (1985–2015), JRC GSW (1984–2020), WorldCover (2020/21), Esri (2017–2023), GLC_FCS30D (1990–2022)

## 5. Major Datasets

| Dataset | Location | Size | Status |
|---------|----------|------|--------|
| Landsat composites (3 windows, 37 years) | phase1/data/, phase2/data/ | ~150 MB (ZIPs) | READY |
| Sentinel-2 composites | phase1/data/ | ~26 MB | READY |
| Gold sample design (641 pts, 125 blocks) | phase4/data/, phase6/data/ | interpreter kits | NOT INTERPRETED |
| T1 training sample (699 pts) | phase6/data/ | interpreter kits | NOT INTERPRETED |
| District pilot composites (4 epochs) | phase5/data/ | ~30 MB (ZIPs) | VALIDATED |
| Phase 7 repository snapshot | phase7/data/ | ~19 MB | FROZEN |
| Phase 7 git bundle | phase7/data/ | ~19 MB | FROZEN |

## 6. Overall Research Architecture

```
Satellite data (Landsat/S2/S1) → Compositing → Harmonisation → Feature engineering
    → Classification (XGBoost/TempCNN/Transformer)
    → Temporal smoothing (HMM) → Uncertainty quantification
    → Thematic analysis (urban/water/vegetation/flood)
    → Validation (silver → gold) → District scaling
```

## 7. Phase 1 → Phase 7 Progression

| Phase | Purpose | Status | Key Output | Important Limitation |
|-------|---------|--------|------------|---------------------|
| 1 | Data foundation: compositing, harmonisation, QA for 3 benchmark windows | Completed | 37-year Landsat dry/annual composites, S2 monthly, S1 monthly, terrain | S2-Landsat radiometric offset discovered; monsoon season unobserved; district only at 240 m |
| 2 | Baseline modelling: L1 classification, temporal reconstruction, thematic analysis (35 experiments) | Completed (silver only) | L1 classifier (macro-F1 0.98 blocks, 0.66–0.81 unseen), temporal LULC maps, water/urban/stress analysis | All accuracy against silver/products only; gold not interpreted; crop types unresolved |
| 3 | Systematic evaluation: 63 experiments, gate framework, transferability, uncertainty | Completed | Gate framework (G1-G6); SAR+terrain improve transfer; temporal context reduces false built-up | Gates 1, 5, 6 NOT PASSED; 1990 built-up implausible; calibration fails on unseen landscapes |
| 4 | District scaling design: frozen evaluation protocol, gold sample design, product benchmark | Completed (design) | Protocol v2, 641-point gold design, product benchmark, 6 confirmatory pre-registrations | NOT_READY: needs human gold + district cube |
| 5 | Pilot cube + infrastructure: district 30 m pilot composites, observation support analysis | Completed (partial) | 4 dry-season district epochs validated; observation-support table; product benchmark extended | NOT_READY: only 4/72 registered products built |
| 6 | Gold workflow + decisions: D1 (addendum review), D2 (sensor correction), D3 (compute route) | Completed (decisions) | Decisions D1/D2/D3; T1 v2 training sample; interpreter kits; gold workflow | NOT_READY: 0 labels; compute boundary |
| 7 | Runner + restoration: confirmatory runner implemented/tested, container restored | Completed (code) | C1/C2 runners validated; spatial context restored byte-identical; all freeze reports | NOT_READY: 0 gold, 0 T1, no cube, no confirmatory runs |

## 8. Current Status

**Phase 7: NOT_READY.** The project is blocked at two boundaries:
1. **Human-label boundary:** 0 Tier-A gold labels, 0 T1 training labels
2. **Compute boundary:** the full district feature cube requires a machine larger than the sandbox

The next scientific action is: two independent human interpreters label the 641 gold points using the Phase 6 interpreter kits.

## 9. Important Scientific Decisions

| Decision | Record | Location |
|----------|--------|----------|
| D1: P4-C1@v2 addendum review | APPROVED WITH CORRECTION | phase6/reports/phase6_D1_review.md |
| D2: Cross-sensor TM correction | LOCAL PIF RMA (not Roy 2016) | phase6/reports/phase6_D2_sensor_correction_decision.md |
| D3: Compute route for district cube | Route A (repository engine on Planetary Computer) | phase6/reports/phase6_D3_compute_decision.md |
| S2→OLI transform | Fitted, corridor only; fails outside corridor (P3-I1) | phase2/reports/phase1_audit.md §7–8 |
| Protocol v2 freeze | Before any confirmatory run | phase4/reports/phase4_readiness_gate.md |

## 10. Important Rejected Hypotheses/Findings

- **P3-I1 (S2→OLI outside corridor):** NOT SUPPORTED — corridor-fitted transform fails on other landscapes
- **P3-J1 (domain probability predicts error):** NOT SUPPORTED — predicts in 1 of 6 directions only
- **P3-J2 (domain adaptation: CORAL/importance weighting/self-training):** NOT SUPPORTED — only few-shot target labels work
- **P3-K1/K2 (annual change timing):** NOT SUPPORTED — annual precision not observationally supported
- **Gate 6 (district reconstruction):** NOT PASSED — product not defensible for district scaling
- **Crop-type classification:** PENDING — silhouette < 0.25; clusters are a greenness gradient, not discrete types

## 11. Important Limitations

1. **No human gold labels exist** — all accuracy is against silver (product consensus) or AI interpretation
2. **District at 30 m covers only 5.8%** — three benchmark windows only
3. **Pre-2013 record sparse** — 2–11 clear dry-season observations per year
4. **Wet season unobserved** — optical blindness during monsoon
5. **S2-Landsat fusion limited** — S2→OLI transform fitted on one landscape only
6. **1990 built-up implausible** — corridor mapped 50–74% built-up vs WSF 21%
7. **No crop labels, no water gauge/storage data**
8. **Gate 4 of Phase 3 is not blind** — model designed after seeing SAR/terrain results

## 12. Where Each Important Artifact Is Located

| Artifact | Path |
|----------|------|
| Phase 2 results (35 experiments) | phase2/reports/phase2_results.md |
| Phase 3 results (63 experiments) | phase3/reports/phase3_results_1.md |
| Phase 4 final report | phase4/reports/phase4_final_report.md |
| Phase 4 audit (weakness matrix) | phase4/reports/phase4_audit.md |
| Phase 5 readiness gate | phase5/reports/phase5_readiness_gate.md |
| Phase 6 gold workflow | phase6/reports/phase6_human_gold_workflow.md |
| Phase 7 readiness gate | phase7/reports/phase7_readiness_gate.md |
| Phase 7 confirmatory freeze | phase7/reports/phase7_confirmatory_freeze.md |
| Gold interpreter kits | phase6/data/phase6_gold_kit_INTERPRETER_A.zip, _B.zip |
| T1 interpreter kits | phase6/data/phase6_T1v2_kit_INTERPRETER_A.zip, _B.zip |
| Repository snapshot | phase7/data/phase7_repository_snapshot.zip |
| Git bundle (full history) | phase7/data/phase7_repository.bundle |

## 13. Large File Storage

All files in this repository are under 30 MB and stored directly in Git. No Git LFS is used. The complete SHA256 manifest is at `provenance/SHA256_MANIFEST.csv`. The original source material is preserved at the researcher's local machine path: `yash renu maam /` (external backup).

## 14. How a New Claude Should Navigate the Repository

1. **Start here** → read this file
2. **Read** `CLAUDE_HANDOFF_MASTER.md` for safety rules and prohibited actions
3. **Read** `RESEARCH_HISTORY.md` for chronological context
4. **Read** the current phase README: `phase7/README.md`
5. **Inspect** `provenance/SHA256_MANIFEST.csv` for file integrity
6. **Inspect** phase reports in `phaseN/reports/`
7. **Check** `RESEARCH_STATUS.md` for current blockers
8. **Perform** a read-only audit before modifying anything
9. **Ask** for authorization before any modification
