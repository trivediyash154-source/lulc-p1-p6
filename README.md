# Pune Earth Observation PhD Research

> [!IMPORTANT]
> **MANDATORY STARTING POINT FOR AI AGENTS:**
> All new AI research assistants (Claude) must start at **[CLAUDE_START_HERE.md](CLAUDE_START_HERE.md)** before performing any operations or reading other documents.

---

## Project Overview

A long-running doctoral research project studying multi-decadal **Land Use / Land Cover (LULC) transformation** in the **Pune district, Maharashtra, India** using multi-sensor satellite Earth observation data (Landsat 5/7/8/9, Sentinel-1 SAR, Sentinel-2 MSI) spanning **1990–2026**.

The complete research agenda, scientific objectives, and hypotheses are defined in **[CURRENT_RESEARCH_VISION.md](CURRENT_RESEARCH_VISION.md)**.

---

## Research Scope

- **Geographic Scope:** Pune district, Maharashtra, India (~15,642 km²) — three 30 m benchmark windows (Khadakwasla–Mutha urban corridor, Baramati semi-arid agricultural plain, Mulshi–Western Ghats forested escarpment) plus district-wide multi-decadal analysis.
- **Temporal Scope:** 1990–2026 (37-year observational record; Sentinel-1/2 integrated for recent epochs).
- **Thematic Scope:** Level-1 (L1) classification (Built-up, Agriculture, Natural Vegetation, Water, Bare/Sparse), urban spatial morphology, surface water dynamics, agricultural phenology/stress, and flood exposure proxies.
- **Methodological Scope:** Cross-sensor radiometry (local PIF RMA), spatiotemporal machine learning (XGBoost, Temporal CNNs, Transformers), design-based probability sampling, spatial cross-validation (Leave-One-Window-Out), and conformal prediction sets.

---

## Phase Tiers & Repository Architecture

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

| Phase Tier | Focus | Status | Authority Level |
|:---|:---|:---|:---|
| **[Phase 1](phase1/)** | Satellite compositing, cross-sensor harmonisation, QA | **FROZEN** | Authoritative Scientific History |
| **[Phase 2](phase2/)** | Baseline L1 modelling, temporal reconstruction | **FROZEN** | Authoritative Scientific History (Silver standard) |
| **[Phase 3](phase3/)** | Systematic 63-experiment evaluation, uncertainty | **FROZEN** | Authoritative Scientific History (Gates 1, 5, 6 NOT PASSED) |
| **[Phase 4](phase4/)** | Protocol v2 pre-registration, gold sample design | **FROZEN** | Authoritative Scientific History |
| **[Phase 5](phase5/)** | District 30 m pilot cube (4 epochs), QA validation | **FROZEN** | Authoritative Scientific History |
| **[Phase 6](phase6/)** | Decisions D1/D2/D3, T1 sample design, evaluator keys | **FROZEN** | Authoritative Scientific History |
| **[Live Phase 7](LIVE_PHASE7_STATUS.md)** | Active execution in current environment (~198 GB free) | **ACTIVE** | **Current Live Research State** (NOT_READY) |
| **[Old Phase 7](phase7/)** | Working snapshot generated during low-storage (~18 GB) period | **REFERENCE** | Non-Authoritative Historical Working Archive |

*Note on Obsolete Phases:* Speculative or low-storage references to "Phase 7.1", "Phase 8", "Phase 9", or "Phase 10" have been completely removed from the authoritative repository structure.

---

## Essential Documents

- **[CLAUDE_START_HERE.md](CLAUDE_START_HERE.md)** — First stop for AI assistants (Rules, read-only audit workflow)
- **[CURRENT_RESEARCH_VISION.md](CURRENT_RESEARCH_VISION.md)** — Complete PhD research framework, hypotheses, and scope
- **[LIVE_PHASE7_STATUS.md](LIVE_PHASE7_STATUS.md)** — Verified status of current live execution environment
- **[PROJECT_MASTER_INDEX.md](PROJECT_MASTER_INDEX.md)** — Master map of the entire repository and artifact index
- **[CLAUDE_HANDOFF_MASTER.md](CLAUDE_HANDOFF_MASTER.md)** — Negative constraints, decision records, and safety protocols
- **[RESEARCH_HISTORY.md](RESEARCH_HISTORY.md)** — Chronological evolution from Phase 1 through Phase 6
- **[RESEARCH_STATUS.md](RESEARCH_STATUS.md)** — Formal readiness gate evaluation and blocker audit
- **[DATA_PROVENANCE.md](DATA_PROVENANCE.md)** — Sensor fleet specifications and data lineage
- **[REPRODUCIBILITY.md](REPRODUCIBILITY.md)** — Reproducibility assessment and software dependencies

---

## Strict Safety & Preservation Rules

1. **DO NOT** modify frozen protocols, historical experiment registries, or decision records in Phases 1–6.
2. **DO NOT** fabricate ground-truth labels, satellite datasets, or experimental findings.
3. **DO NOT** treat silver consensus labels or AI predictions as human Tier-A gold labels.
4. **DO NOT** treat exploratory findings as confirmatory proof.
5. **DO NOT** delete historical evidence, failed experiments, or negative results.
6. **DO NOT** assume the old Phase 7 working snapshot represents current live decisions.
7. **DO NOT** run confirmatory experiments until human gold labels are unblinded and the district cube is built.

---

## Large Data & Archival Policy

All repository artifacts are under 30 MB and stored directly in Git. Full cryptographic integrity is tracked via SHA-256 hashes in `provenance/SHA256_MANIFEST.csv`. The original historical source files remain preserved locally at `/Users/yashtrivedi/yash renu maam `.

---

## How a New Research Agent Should Start

1. Read **[CLAUDE_START_HERE.md](CLAUDE_START_HERE.md)**
2. Read **[CURRENT_RESEARCH_VISION.md](CURRENT_RESEARCH_VISION.md)**
3. Read **[CLAUDE_HANDOFF_MASTER.md](CLAUDE_HANDOFF_MASTER.md)**
4. Read **[PROJECT_MASTER_INDEX.md](PROJECT_MASTER_INDEX.md)**
5. Read **[LIVE_PHASE7_STATUS.md](LIVE_PHASE7_STATUS.md)**
6. Inspect **[provenance/SHA256_MANIFEST.csv](provenance/SHA256_MANIFEST.csv)**
7. Conduct a **READ-ONLY AUDIT** and await explicit researcher authorization.
