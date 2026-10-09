# PUNE EO PhD — PROJECT HANDOFF MASTER

> [!IMPORTANT]
> **PRIMARY ENTRY POINT:**
> Every new contributor or research assistant **MUST START AT [START_HERE.md](START_HERE.md)** before reading this document or touching any files.

---

## Mission

This is a PhD-level remote-sensing research project studying multi-decadal land use and land cover (LULC) change in Pune district, India, using Landsat (1990–2026), Sentinel-2, and Sentinel-1 satellite imagery. The goal is a defensible, uncertainty-quantified, human-validated 30 m land-cover change record and associated thematic analyses (urban expansion, water dynamics, vegetation stress, flood hazard).

The comprehensive research objectives and scientific scope are defined in [CURRENT_RESEARCH_VISION.md](CURRENT_RESEARCH_VISION.md).

---

## Phase Tiers & Authority Structure

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

1. **PHASE 1–6 = FROZEN SCIENTIFIC HISTORY:** Unmodifiable empirical and methodological foundation. Historical failures, rejected hypotheses, and limitations are preserved exactly as documented.
2. **CURRENT REAL PHASE 7 = ACTIVE LIVE RESEARCH:** Executed separately in the current clean computational environment (~198 GB available disk space). Live status is tracked in [LIVE_PHASE7_STATUS.md](LIVE_PHASE7_STATUS.md).
3. **OLD PHASE 7 ARCHIVE (`phase7/`) = REFERENCE ONLY:** An earlier working snapshot created under severely constrained storage (~18 GB free space) prior to container loss. It is strictly non-authoritative.
4. **PHASE 8 / 9 / 10 = REMOVED:** Obsolete, speculative, or low-storage references to Phase 8, Phase 9, or Phase 10 have been removed from the authoritative repository structure.

---

## Historical Phases (Phases 1–6 Frozen)

| Phase | What happened | Key finding / Status |
|-------|---------------|----------------------|
| **Phase 1** | Built 37-year Landsat composite archive for 3 benchmark windows; discovered S2-Landsat radiometric offset | S2 systematically brighter than Landsat in all bands; fusion requires fitted transform |
| **Phase 2** | Trained L1 classifier (35 experiments); temporal LULC reconstruction; water/urban/stress analysis | macro-F1 0.98 within-window but 0.66-0.81 on unseen landscapes; 1990 built-up implausible; all silver-standard |
| **Phase 3** | Systematic 63-experiment evaluation; gate framework; SAR+terrain; transferability; uncertainty | Gates 1, 5, 6 NOT PASSED; temporal context and SAR improve transfer; calibration fails cross-landscape |
| **Phase 4** | Designed district evaluation (protocol v2 frozen, 641-pt gold sample, product benchmark, 6 confirmatory pre-registrations) | Products disagree by up to 4x on built-up area; all NOT_READY without gold |
| **Phase 5** | Pilot district cube (4 dry-season epochs validated); observation-support analysis | Bit-identical to windows on overlap; sparse pre-2000 observations |
| **Phase 6** | Three decisions (D1/D2/D3); T1 v2 training sample; gold workflow; interpreter kits | Decisions recorded; kits ready; 0 labels |

---

## Source of Truth Hierarchy

1. [CURRENT_RESEARCH_VISION.md](CURRENT_RESEARCH_VISION.md) — Overarching research direction and scientific hypotheses
2. **Frozen protocol documents** — `phase4/protocols/`, `phase6/protocols/`, protocol v2 YAML/code hashes
3. **Experiment registry** — immutable records (failed/invalid ones kept)
4. **Decision records** — D1, D2, D3 in `phase6/reports/`
5. **Original research reports** — `phaseN/reports/*.md`
6. **Artifact manifests & hashes** — `provenance/SHA256_MANIFEST.csv`, `manifests/`
7. **Raw/large data** — `phaseN/data/` (ZIP archives, GeoTIFFs)
8. **AI-generated summaries** — this document and navigation files

> [!CAUTION]
> **The AI summary must NEVER override the original evidence.**

---

## Frozen Scientific Rules

1. Protocol v2 is frozen (code hash `3bb40f9c...`, YAML hash `fe1fad6f...`)
2. Gold sample design (641 points, 125 blocks) is frozen
3. T1 v2 training sample design (699 points) is frozen
4. District silver (SHA-256 `32e1ddb0...`) is frozen
5. D2 cross-sensor correction (SHA-256 `782eaef3...`) is frozen
6. No confirmatory experiment may run until gold labels and district cube are available
7. Exploratory results must never be presented as confirmatory
8. Silver/AI labels must never substitute for human Tier-A gold
9. Experiment records are append-only; failed/invalid records are never deleted

---

## Important Decisions (Frozen Authority)

| Decision | Status | Authoritative record |
|----------|--------|---------------------|
| **D1: P4-C1@v2 addendum** | Approved with correction; countersigned by Arush | [phase6/reports/phase6_D1_review.md](phase6/reports/phase6_D1_review.md) |
| **D2: Cross-sensor TM correction** | Local PIF RMA selected over Roy 2016 | [phase6/reports/phase6_D2_sensor_correction_decision.md](phase6/reports/phase6_D2_sensor_correction_decision.md) |
| **D3: Compute route** | Route A (repository engine, Planetary Computer) | [phase6/reports/phase6_D3_compute_decision.md](phase6/reports/phase6_D3_compute_decision.md) |

---

## Known Failures & Rejected Hypotheses

- **P3-I1:** S2→OLI transform outside corridor — **NOT SUPPORTED** (spatially variant)
- **P3-J1:** Domain probability predicts error — **NOT SUPPORTED**
- **P3-J2:** CORAL / importance weighting / self-training — **NOT SUPPORTED** (only few-shot labels work)
- **P3-K1/K2:** Annual change timing — **NOT SUPPORTED**
- **Gate 5:** Calibration coverage on unseen landscapes — **NOT PASSED**
- **Gate 6:** District reconstruction defensibility — **NOT PASSED**
- **Crop-type classification:** silhouette < 0.25, clusters not discrete types
- **Phase 3 selected model P3-F3:** 1990 built share implausible (corridor 0.50 vs WSF 0.21)
- **TerraClimate water-balance access:** Planetary Computer zarr async-loop error (EXP-STRESS-002)

---

## Known Limitations

1. **0 human gold labels** — no absolute accuracy claim is valid
2. **District at 30 m = 5.8%** (3 benchmark windows only)
3. **Pre-2013 observations sparse** (2–11 per year, dry season only)
4. **Wet season unobserved** (optical blindness during monsoon)
5. **S2-Landsat fusion only tested on one landscape**
6. **Training labels biased toward interior cells** (edge share 7–25% vs 65–72%)
7. **Spatial autocorrelation extends beyond 3 km test blocks**
8. **Gate 4 of Phase 3 is not blind**

---

## Data Provenance & Sensor Fleet

- **Landsat:** Collection 2, Level 2, Planetary Computer
- **Sentinel-2:** Level 2A (Sen2Cor), Planetary Computer
- **Sentinel-1:** RTC gamma0, Planetary Computer
- **SRTM DEM:** 30 m, Planetary Computer
- **CHIRPS:** Monthly precipitation, 5 km
- **Reference products:** GHSL, WSF-Evolution, JRC GSW, WorldCover, Esri, GLC_FCS30D — all public sources
- **Cryptographic Hashes:** `provenance/SHA256_MANIFEST.csv`

---

## Large Files Policy

All files in this repository are under 30 MB and stored directly in Git. The largest file is `phase1/data/pune-eo-phd_samples_sentinel_2.zip` (~26 MB). The original source is preserved at the researcher's local backup storage (`/Users/yashtrivedi/yash renu maam `).

---

## DO NOT (Mandatory Negative Constraints)

- **DO NOT** change protocols without authorization
- **DO NOT** modify frozen artifacts in Phases 1–6
- **DO NOT** fabricate human labels
- **DO NOT** fabricate or synthesize dataset records
- **DO NOT** substitute unavailable data
- **DO NOT** rerun experiments under altered conditions and call them the same experiment
- **DO NOT** treat silver labels or AI classifications as human gold
- **DO NOT** treat exploratory results as confirmatory
- **DO NOT** change experiment definitions silently
- **DO NOT** delete historical evidence or negative results
- **DO NOT** resolve inconsistencies by rewriting earlier phase records
- **DO NOT** treat the old Phase 7 archive as the current live Phase 7
- **DO NOT** assume Phase 8, 9, or 10 exist

---

## FIRST ACTION FOR A NEW CONTRIBUTOR

**EXECUTE A READ-ONLY AUDIT.** Do NOT immediately run experiments.

1. Start at **[START_HERE.md](START_HERE.md)**
2. Read **[CURRENT_RESEARCH_VISION.md](CURRENT_RESEARCH_VISION.md)**
3. Read this document (**HANDOFF_MASTER.md**)
4. Read **[PROJECT_MASTER_INDEX.md](PROJECT_MASTER_INDEX.md)**
5. Read **[RESEARCH_HISTORY.md](RESEARCH_HISTORY.md)**
6. Read **[LIVE_PHASE7_STATUS.md](LIVE_PHASE7_STATUS.md)**
7. Read phase READMEs (`phase1/` through `phase6/`)
8. Inspect manifests (`provenance/SHA256_MANIFEST.csv`)
9. Inspect protocol/decision records (`phase6/protocols/`)
10. Check repository status (`RESEARCH_STATUS.md`)
11. Verify hashes where possible
12. Report any inconsistencies found to the researcher
13. **Ask for explicit authorization before modifying anything**
