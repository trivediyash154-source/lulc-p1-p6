# Pune Earth Observation PhD Research

## Project Overview

A long-running PhD research project studying **Land Use / Land Cover (LULC) change** in the **Pune district, Maharashtra, India** using multi-sensor satellite Earth observation data (Landsat, Sentinel-1, Sentinel-2) spanning **1990–2026**.

## Research Scope

- **Geographic scope:** Pune district, Maharashtra, India — three 30 m benchmark windows (Khadakwasla–Mutha corridor, Baramati agricultural plain, Mulshi–Western Ghats) plus district-wide analysis at 240 m
- **Temporal scope:** 1990–2026 (37-year Landsat record; Sentinel-1/2 from 2015/2017)
- **Thematic scope:** L1 land-cover classification (built-up, agriculture, natural vegetation, water, bare/sparse), urban expansion, water body dynamics, vegetation stress, land-change trajectories, flood hazard proxies
- **Methodological scope:** Compositing, harmonisation, XGBoost/TempCNN/Transformer classification, HMM temporal smoothing, uncertainty quantification, spatial transferability (leave-one-window-out), conformal prediction, exploratory change drivers

## Current Phase

**Phase 7 (most recent session) — Status: NOT_READY**

The project is blocked on two critical inputs:
1. **Human Tier-A gold labels** — 0 of 641 points interpreted by human interpreters
2. **District 30 m feature cube** — requires compute resources beyond the sandbox

No confirmatory experiment has been executed. All six pre-registered confirmatory records (P4-C1@v2 through P4-C6@v2) remain PENDING.

## Phase Navigation

| Phase | Purpose | Status |
|-------|---------|--------|
| [Phase 1](phase1/) | Data foundation — satellite compositing, harmonisation, QA | Completed |
| [Phase 2](phase2/) | Baseline modelling — L1 classification, temporal reconstruction, thematic analysis | Completed (silver-standard only) |
| [Phase 3](phase3/) | Systematic evaluation — 63 experiments, gate framework, transferability, uncertainty | Completed (Gates 1, 5, 6 NOT PASSED) |
| [Phase 4](phase4/) | District scaling design — evaluation protocol, gold sample design, product benchmark | Completed (design only; NOT_READY) |
| [Phase 5](phase5/) | Pilot cube + infrastructure — district pilot composites, readiness assessment | Completed (PARTIAL; NOT_READY) |
| [Phase 6](phase6/) | Gold workflow + decisions — D1/D2/D3 decisions, T1 training sample design, interpreter kits | Completed (decisions taken; NOT_READY) |
| [Phase 7](phase7/) | Runner implementation + restoration — confirmatory runner, container restoration, freeze reports | Completed (code ready; NOT_READY) |

## Important Documents

- [PROJECT_MASTER_INDEX.md](PROJECT_MASTER_INDEX.md) — Complete project map
- [CLAUDE_HANDOFF_MASTER.md](CLAUDE_HANDOFF_MASTER.md) — New AI agent onboarding
- [RESEARCH_HISTORY.md](RESEARCH_HISTORY.md) — Chronological evolution
- [RESEARCH_STATUS.md](RESEARCH_STATUS.md) — Current status snapshot
- [DATA_PROVENANCE.md](DATA_PROVENANCE.md) — Data lineage and sources
- [REPRODUCIBILITY.md](REPRODUCIBILITY.md) — Reproducibility assessment

## Repository Rules

1. **DO NOT** modify frozen protocols, experiment registries, or decision records
2. **DO NOT** fabricate labels, data, or experiment results
3. **DO NOT** treat silver/AI labels as human gold
4. **DO NOT** treat exploratory results as confirmatory
5. **DO NOT** delete historical evidence, including failed/rejected experiments
6. **DO NOT** silently resolve inconsistencies between phases
7. **DO NOT** rerun experiments under altered conditions using the same experiment ID

## Large Data Policy

Some datasets (ZIP archives, GeoTIFF composites) are stored directly in this repository. The largest file is ~26 MB. All files have SHA256 hashes recorded in `provenance/SHA256_MANIFEST.csv`. The source material is also preserved at the original external location (`yash renu maam` folder on the researcher's machine).

## How a New Research Agent Should Start

1. Read [CLAUDE_HANDOFF_MASTER.md](CLAUDE_HANDOFF_MASTER.md)
2. Read [PROJECT_MASTER_INDEX.md](PROJECT_MASTER_INDEX.md)
3. Read [RESEARCH_HISTORY.md](RESEARCH_HISTORY.md)
4. Read the current phase README ([Phase 7](phase7/README.md))
5. Inspect manifests in `manifests/`
6. Perform a **read-only audit** — verify hashes, check consistency
7. **Only then** perform authorized work after asking for permission
