# STOP — READ THIS BEFORE DOING ANYTHING

You are entering an existing PhD research archive.

This is **NOT** a blank project.

The scientific research has already been developed through multiple phases.

Your first task is **NOT** to improve the research.

Your first task is to **UNDERSTAND** the research.

- **DO NOT RUN EXPERIMENTS.**
- **DO NOT MODIFY FILES.**
- **DO NOT DELETE FILES.**
- **DO NOT GENERATE NEW DATA.**
- **DO NOT TRAIN MODELS.**
- **DO NOT RETRAIN MODELS.**
- **DO NOT CHANGE PROTOCOLS.**
- **DO NOT CHANGE EXPERIMENT IDs.**
- **DO NOT INVENT RESULTS.**
- **DO NOT FABRICATE DATA.**
- **DO NOT FABRICATE HUMAN LABELS.**
- **DO NOT TREAT SILVER LABELS AS HUMAN GOLD.**
- **DO NOT TREAT AI-GENERATED LABELS AS HUMAN GOLD.**
- **DO NOT ASSUME A DOCUMENT IS CURRENT JUST BECAUSE IT IS RECENTLY COMMITTED.**
- **DO NOT ASSUME THE OLD PHASE 7 ARCHIVE IS THE CURRENT PHASE 7.**
- **DO NOT ASSUME PHASE 8, 9, OR 10 EXIST.**

Those obsolete phases have been removed from the authoritative archive.

---

## RESEARCH AUTHORITY

The authority hierarchy is strictly ordered as follows:

1. [CURRENT_RESEARCH_VISION.md](CURRENT_RESEARCH_VISION.md)
2. **FROZEN PHASE 1–6 SCIENTIFIC RECORDS** (`phase1/` through `phase6/`)
3. **CURRENT LIVE PHASE 7 STATUS** ([LIVE_PHASE7_STATUS.md](LIVE_PHASE7_STATUS.md))
4. **FROZEN PROTOCOLS / REGISTRY / DECISION RECORDS** (in `phaseX/protocols/` and `phaseX/reports/`)
5. **ORIGINAL SOURCE ARTIFACTS** (in `phaseX/archive/original_structure/`)
6. **MANIFESTS / PROVENANCE** (`manifests/` and `provenance/`)
7. **HISTORICAL/ARCHIVED PHASE 7 REFERENCE MATERIAL** (in `phase7/`)

> [!WARNING]
> **IMPORTANT:**
> The old archived Phase 7 material is **NOT** authoritative. It was created in an earlier constrained computational environment (~18 GB free storage) before container loss. It may be useful for reference, but it must **NEVER** override the current research vision or current live Phase 7 status.

---

## FIRST DOCUMENTS TO READ

Read in this exact sequential order:

1. [START_HERE.md](START_HERE.md) (This document)
2. [CURRENT_RESEARCH_VISION.md](CURRENT_RESEARCH_VISION.md) (Defines overarching PhD research vision and questions)
3. [HANDOFF_MASTER.md](HANDOFF_MASTER.md) (Safety rules, prohibitions, decision logs)
4. [PROJECT_MASTER_INDEX.md](PROJECT_MASTER_INDEX.md) (Master navigation map and artifact directory)
5. [RESEARCH_HISTORY.md](RESEARCH_HISTORY.md) (Chronological evolution from Phase 1 through Phase 6)
6. [RESEARCH_STATUS.md](RESEARCH_STATUS.md) (Gate status, blockers, validation vs readiness)
7. [LIVE_PHASE7_STATUS.md](LIVE_PHASE7_STATUS.md) (Current verified live Phase 7 status)
8. `phase1/README.md`
9. `phase2/README.md`
10. `phase3/README.md`
11. `phase4/README.md`
12. `phase5/README.md`
13. `phase6/README.md`
14. **Frozen protocols** in each phase's `protocols/` subdirectory
15. **Experiment registries** and decision records (D1, D2, D3)
16. **Provenance and manifests** in `manifests/` and `provenance/`
17. **Old Phase 7 reference material** ONLY AFTER completing the above.

---

## PHASE STRUCTURE

| Phase Tier | Scope | Authority Status |
|:---|:---|:---|
| **PHASE 1–6** | Multi-sensor compositing, baseline modelling, 63 evaluation experiments, protocol freeze, silver cube pilot, human gold sample design | **FROZEN SCIENTIFIC HISTORY** (Unmodifiable) |
| **CURRENT PHASE 7** | Active computational execution in current environment | **ACTIVE RESEARCH** (Live status in `LIVE_PHASE7_STATUS.md`) |
| **OLD ARCHIVED PHASE 7** | Working snapshot generated during low-storage (~18 GB free) pre-current period | **REFERENCE ONLY** (Non-authoritative) |
| **PHASE 8+** | Conceptual / exploratory references in old notes | **NOT PART OF CURRENT ARCHIVE** (Completely removed) |

---

## FIRST ACTION: READ-ONLY AUDIT

Your very first action upon entering this repository must be a **READ-ONLY AUDIT**.

Do not modify anything. Do not run training or evaluation pipelines.

Produce a formal structured audit covering:
1. **Research Identity**
2. **Research Vision**
3. **Historical Phase Summary (Phases 1–6)**
4. **Current Phase 7 Status**
5. **Archived Phase 7 Status**
6. **Data Status (Sensor availability vs actual processing)**
7. **Human-Label Status (Gold vs Silver distinction)**
8. **Experiment Status (Exploratory vs Confirmatory)**
9. **Protocol Status (Protocol v2 freeze)**
10. **Provenance Status (Hashes and manifest check)**
11. **Known Limitations & Failure Points**
12. **Contradictions Requiring Researcher Review**
13. **Current Blockers**
14. **Recommended Next Actions**

### Then STOP.
### WAIT FOR RESEARCHER AUTHORIZATION BEFORE TAKING ANY FURTHER ACTION.
