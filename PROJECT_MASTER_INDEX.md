# PROJECT MASTER INDEX: PUNE EARTH OBSERVATION PhD

## 1. What the PhD Project Is

A doctoral research project in Remote Sensing and Earth Observation studying multi-decadal land-use and land-cover (LULC) transformation across the **Pune district, Maharashtra, India**, utilizing multi-sensor satellite observations (Landsat, Sentinel-1 SAR, Sentinel-2 MSI) spanning **1990–2026**.

See the comprehensive research framework in [CURRENT_RESEARCH_VISION.md](CURRENT_RESEARCH_VISION.md).

---

## 2. Research Authority & Architecture Hierarchy

```text
                     CURRENT RESEARCH VISION
                    (CURRENT_RESEARCH_VISION.md)
                                 │
                                 ▼
                     FROZEN SCIENTIFIC HISTORY
                        (PHASE 1 → PHASE 6)
                                 │
                                 ▼
                       CURRENT REAL PHASE 7
                      (LIVE_PHASE7_STATUS.md)
                                 │
                                 ▼
                        FUTURE RESEARCH WORK

                [OLD ARCHIVED PHASE 7 (phase7/)]
              = Historical Reference Material Only
```

### The Three Repository Tiers:
1. **FROZEN SCIENTIFIC HISTORY (Phases 1–6):**
   - Unmodifiable historical foundation. Contains data harmonisation, baseline models, 63 evaluation experiments, Protocol v2 pre-registration, silver pilot cube, and human gold sample design.
2. **CURRENT LIVE PHASE 7:**
   - Active, ongoing research work executed in the clean, current computational environment (~198 GB available storage). Documented in [LIVE_PHASE7_STATUS.md](LIVE_PHASE7_STATUS.md).
3. **ARCHIVED / REFERENCE PHASE 7 MATERIAL (`phase7/`):**
   - Retained working snapshot generated during an earlier low-storage (~18 GB free space) pre-current period prior to container loss. Strictly **non-authoritative**.
   - *Note on Obsolete Phases:* Conceptual or temporary references to "Phase 7.1", "Phase 8", "Phase 9", or "Phase 10" from the old environment have been completely removed from the authoritative repository structure.

---

## 3. Geographic Scope & Benchmark Windows

- **District-Wide Domain:** Entire Pune District (~15,642 km²), evaluated at 240 m overview and 30 m pilot epochs.
- **Three 30 m Benchmark Windows (covering 5.8% of district area):**
  1. `pune_G30_khadakwasla_mutha`: Urban–peri-urban corridor along the Mutha river, including Pune city core, peri-urban fringe, and Khadakwasla reservoir.
  2. `pune_G30_baramati_agri`: Semi-arid agricultural plain dominated by sugarcane, canal networks, and rainfed cropping systems.
  3. `pune_G30_mulshi_ghats`: High-relief Western Ghats forested escarpment containing moist/dry deciduous forests, heavy monsoon rainfall, and Mulshi reservoir.

---

## 4. Temporal Scope & Satellite Sensor Fleet

| Concept | Definition & Range |
|:---|:---|
| **Archive Availability** | Period during which satellite instruments collected data:<br>• Landsat 5 TM: 1984–2013<br>• Landsat 7 ETM+: 1999–present (SLC-off post-May 2003)<br>• Landsat 8 OLI: 2013–present<br>• Landsat 9 OLI-2: 2021–present<br>• Sentinel-2 MSI: 2015–present (operational S2A/S2B in study: 2018–2026)<br>• Sentinel-1 SAR: 2014–present (S1 C-band in study: 2017–2025)<br>• CHIRPS Rainfall: 1981–present |
| **Project Analysis Window** | **1990–2026** (37-year observational record) |
| **Actually Processed & Used** | • Landsat seasonal dry-season composites (1990–2026, 3 windows at 30 m, district at 240 m)<br>• Sentinel-2 optical composites (2018–2026)<br>• District 30 m silver pilot cubes (1990, 2000, 2010, 2020) |
| **Planned / Pending** | Full 37-year district-wide 30 m wall-to-wall feature cube (awaiting cloud compute execution) |

---

## 5. Phase Progression & Status Matrix

| Phase | Purpose | Status | Key Output | Important Limitation / Lesson |
|:---|:---|:---|:---|:---|
| [Phase 1](phase1/) | Satellite compositing, harmonization, QA | **FROZEN** | 37-yr Landsat dry-season composites (3 windows + district 240m) | Pre-2013 data sparse (2–11 clear passes/yr); monsoon optical blindness |
| [Phase 2](phase2/) | Baseline L1 modelling & thematic analysis | **FROZEN** | Initial L1 classification maps, change masks | Silver consensus severely overestimates 1990 built-up (50–74% vs WSF 21%) |
| [Phase 3](phase3/) | Systematic 63-experiment evaluation | **FROZEN** | Benchmark evaluations, spatial transfer tests | Gates 1, 5, 6 NOT PASSED; S2→OLI corridor transform fails in Mulshi/Baramati |
| [Phase 4](phase4/) | Protocol hardening & validation sample design | **FROZEN** | Protocol v2 pre-registration; 641-pt Tier-A gold design | No human gold labels interpreted; all metrics remained silver |
| [Phase 5](phase5/) | Pilot cube build & QA validation | **FROZEN** | 4-epoch (1990, 2000, 2010, 2020) district 30m pilot cube | Verified pipeline scalability, but full 37-yr district cube pending |
| [Phase 6](phase6/) | Key generation, decision reviews, T1 sample | **FROZEN** | Decisions D1, D2, D3; blind evaluator keys; T1 699-pt sample | D2 chose local PIF RMA; D3 chose Route A cloud compute; labels uninterpreted |
| [Live Phase 7](LIVE_PHASE7_STATUS.md) | Active execution in current environment | **ACTIVE** | Dedicated execution environment, verified 198 GiB disk space | Gate NOT_READY: strictly blocked on human Tier-A gold interpretation |
| [Old Phase 7](phase7/) | Historical container-loss recovery snapshot | **REFERENCE** | Working runner scripts (C1/C2 synthetically tested) | Non-authoritative snapshot; generated under low disk storage (~18 GB) |

---

## 6. Important Scientific Decisions (Frozen Records)

| Decision | Authority & Location | Core Determination |
|:---|:---|:---|
| **D1: P4-C1@v2 Review** | [phase6/reports/phase6_D1_review.md](phase6/reports/phase6_D1_review.md) | Approved Protocol v2 rules with explicit addendum constraints. |
| **D2: Cross-Sensor TM Correction** | [phase6/reports/phase6_D2_sensor_correction_decision.md](phase6/reports/phase6_D2_sensor_correction_decision.md) | Adopted **local PIF RMA regression** over global Roy (2016) coefficients due to local soil and canopy reflectance. |
| **D3: District Cube Compute Route** | [phase6/reports/phase6_D3_compute_decision.md](phase6/reports/phase6_D3_compute_decision.md) | Selected **Route A** (Planetary Computer in-situ compute) for full district 30 m feature cube generation. |
| **Protocol v2 Pre-registration** | [phase4/reports/phase4_readiness_gate.md](phase4/reports/phase4_readiness_gate.md) | Permanently froze confirmatory evaluation criteria prior to unblinding any validation data. |

---

## 7. Important Rejected Hypotheses & Negative Findings

- **P3-I1 (Spatial Invariance of Sensor Transform):** **REJECTED.** Spectral transforms fitted on the Khadakwasla corridor fail on other landscapes.
- **P3-J1 (Domain Probability Predicts Error):** **REJECTED.** Domain classifier probability failed to predict spatial transfer error in 5 out of 6 transfer directions.
- **P3-J2 (Unsupervised Domain Adaptation):** **REJECTED.** CORAL, importance weighting, and self-training failed to recover transfer performance.
- **P3-K1/K2 (Annual Land-Change Precision):** **REJECTED.** Annual timing of change events cannot be observationally supported with historical Landsat observation densities.
- **Crop-Type Separation:** **NOT SUPPORTED.** Clustering yielded silhouette scores < 0.25; clusters represent continuous greenness gradients rather than distinct botanical crops.

---

## 8. Where Important Artifacts Are Located

| Artifact | Authoritative Path |
|:---|:---|
| Master Entry Point for AI Assistants | [CLAUDE_START_HERE.md](CLAUDE_START_HERE.md) |
| Research Vision & Scope | [CURRENT_RESEARCH_VISION.md](CURRENT_RESEARCH_VISION.md) |
| Safety & Handoff Protocol | [CLAUDE_HANDOFF_MASTER.md](CLAUDE_HANDOFF_MASTER.md) |
| Current Live Phase 7 Status | [LIVE_PHASE7_STATUS.md](LIVE_PHASE7_STATUS.md) |
| Chronological Narrative | [RESEARCH_HISTORY.md](RESEARCH_HISTORY.md) |
| Research Status & Gating | [RESEARCH_STATUS.md](RESEARCH_STATUS.md) |
| Data Provenance & Sensor Tiers | [DATA_PROVENANCE.md](DATA_PROVENANCE.md) |
| Reproducibility Protocol | [REPRODUCIBILITY.md](REPRODUCIBILITY.md) |
| Cryptographic Hash Manifest | [provenance/SHA256_MANIFEST.csv](provenance/SHA256_MANIFEST.csv) |
| Master File Inventory | [manifests/MASTER_FILE_MANIFEST.csv](manifests/MASTER_FILE_MANIFEST.csv) |
| Human Gold Interpreter Kits | `phase6/data/phase6_gold_kit_INTERPRETER_A.zip`, `_B.zip` |
| T1 Training Kits | `phase6/data/phase6_T1v2_kit_INTERPRETER_A.zip`, `_B.zip` |
| Historical Evaluator Keys | `phase6/data/phase6_KEYS_evaluator_only.zip` |

---

## 9. How a New Research Agent (Claude) Must Navigate This Repository

1. **Start at [CLAUDE_START_HERE.md](CLAUDE_START_HERE.md)** — Do NOT take action until this is read.
2. **Read [CURRENT_RESEARCH_VISION.md](CURRENT_RESEARCH_VISION.md)** — Understand what the project is, what is established, and what remains to be tested.
3. **Read [CLAUDE_HANDOFF_MASTER.md](CLAUDE_HANDOFF_MASTER.md)** — Review mandatory negative constraints and safety rules.
4. **Read [LIVE_PHASE7_STATUS.md](LIVE_PHASE7_STATUS.md)** — Check the active execution status in the clean environment.
5. **Inspect [RESEARCH_STATUS.md](RESEARCH_STATUS.md)** and phase READMEs (`phase1/` through `phase6/`).
6. **Execute a READ-ONLY AUDIT.** Report findings to the researcher and await explicit authorization.
