# PUNE EO PhD — CLAUDE HANDOFF MASTER

## Mission

This is a PhD-level remote-sensing research project studying multi-decadal land use and land cover (LULC) change in Pune district, India, using Landsat (1990–2026), Sentinel-2, and Sentinel-1 satellite imagery. The goal is a defensible, uncertainty-quantified, human-validated 30 m land-cover change record and associated thematic analyses (urban expansion, water dynamics, vegetation stress, flood hazard).

## Current Phase

**Phase 7 — NOT_READY**

- 0 of 641 human gold labels interpreted
- 0 of 699 T1 training labels interpreted
- 4 of 72 registered district cube products built
- 0 of 6 confirmatory experiments executed
- All decisions (D1, D2, D3) taken and recorded
- Confirmatory runner code implemented and tested (C1, C2)
- Container lost and restored from Phase 6 snapshot

## Historical Phases

| Phase | What happened | Key finding |
|-------|---------------|-------------|
| 1 | Built 37-year Landsat composite archive for 3 benchmark windows; discovered S2-Landsat radiometric offset | S2 systematically brighter than Landsat in all bands; fusion requires fitted transform |
| 2 | Trained L1 classifier (35 experiments); temporal LULC reconstruction; water/urban/stress analysis | macro-F1 0.98 within-window but 0.66-0.81 on unseen landscapes; 1990 built-up implausible; all silver-standard |
| 3 | Systematic 63-experiment evaluation; gate framework; SAR+terrain; transferability; uncertainty | Gates 1,5,6 NOT PASSED; temporal context and SAR improve transfer; calibration fails cross-landscape |
| 4 | Designed district evaluation (protocol v2 frozen, 641-pt gold sample, product benchmark, 6 confirmatory pre-registrations) | Products disagree by up to 4x on built-up area; all NOT_READY without gold |
| 5 | Pilot district cube (4 dry-season epochs validated); observation-support analysis | Bit-identical to windows on overlap; sparse pre-2000 observations |
| 6 | Three decisions (D1/D2/D3); T1 v2 training sample; gold workflow; interpreter kits | Decisions recorded; kits ready; 0 labels |
| 7 | Runner implementation; container loss + restoration; all freeze reports | Code ready; spatial context restored byte-identical; all NOT_READY |

## Source of Truth Hierarchy

1. **Frozen protocol documents** — `experiments/registry4/`, protocol v2 YAML/code hashes
2. **Experiment registry** — immutable records (failed/invalid ones kept)
3. **Decision records** — D1, D2, D3 in phase6/reports/
4. **Original research reports** — phaseN/reports/*.md
5. **Artifact manifests** — provenance/SHA256_MANIFEST.csv
6. **Raw/large data** — phaseN/data/ (ZIP archives, GeoTIFFs)
7. **AI-generated summaries** — this document and navigation files

**The AI summary must NEVER override the original evidence.**

## Frozen Scientific Rules

1. Protocol v2 is frozen (code hash `3bb40f9c...`, YAML hash `fe1fad6f...`)
2. Gold sample design (641 points, 125 blocks) is frozen
3. T1 v2 training sample design (699 points) is frozen
4. District silver (SHA-256 `32e1ddb0...`) is frozen
5. D2 cross-sensor correction (SHA-256 `782eaef3...`) is frozen
6. No confirmatory experiment may run until gold and cube are available
7. Exploratory results must never be presented as confirmatory
8. Silver/AI labels must never substitute for human Tier-A gold
9. Experiment records are append-only; failed/invalid records are never deleted

## Important Decisions

| Decision | Status | Authoritative record |
|----------|--------|---------------------|
| D1: P4-C1@v2 addendum | Approved with correction; countersigned by Arush | phase6/reports/phase6_D1_review.md |
| D2: Cross-sensor TM correction | Local PIF RMA selected over Roy 2016 | phase6/reports/phase6_D2_sensor_correction_decision.md |
| D3: Compute route | Route A (repository engine, Planetary Computer) | phase6/reports/phase6_D3_compute_decision.md |

## Known Failures

- P3-I1: S2→OLI transform outside corridor — NOT SUPPORTED
- P3-J1: Domain probability predicts error — NOT SUPPORTED
- P3-J2: CORAL/importance weighting/self-training — NOT SUPPORTED (only few-shot labels work)
- P3-K1/K2: Annual change timing — NOT SUPPORTED
- Gate 5: Calibration coverage on unseen landscapes — NOT PASSED
- Gate 6: District reconstruction defensibility — NOT PASSED
- Crop-type classification: silhouette < 0.25, clusters not discrete types
- Phase 3 selected model P3-F3: 1990 built share implausible (corridor 0.50 vs WSF 0.21)
- TerraClimate water-balance access: Planetary Computer zarr async-loop error (EXP-STRESS-002)

## Known Limitations

1. 0 human gold labels — no absolute accuracy claim is valid
2. District at 30 m = 5.8% (3 windows only)
3. Pre-2013 observations sparse (2–11 per year, dry season only)
4. Wet season unobserved (optical blindness)
5. S2-Landsat fusion only tested on one landscape
6. Training labels biased toward interior cells (edge share 7–25% vs 65–72%)
7. Spatial autocorrelation extends beyond 3 km test blocks
8. Gate 4 of Phase 3 is not blind

## Data Provenance

- **Landsat:** Collection 2, Level 2, Planetary Computer (anonymous access)
- **Sentinel-2:** Level 2A (Sen2Cor), Planetary Computer
- **Sentinel-1:** RTC gamma0, Planetary Computer
- **SRTM DEM:** 30 m, Planetary Computer
- **CHIRPS:** Monthly precipitation, 5 km
- **Reference products:** GHSL, WSF-Evolution, JRC GSW, WorldCover, Esri, GLC_FCS30D — all from public sources
- **SHA256 hashes:** `provenance/SHA256_MANIFEST.csv`

## Large Files

All files in this repository are under 30 MB and stored directly. The largest file is `phase1/data/pune-eo-phd_samples_sentinel_2.zip` (~26 MB). The original source is preserved at the researcher's local storage (`yash renu maam /`).

## DO NOT

- Change protocols without authorization
- Modify frozen artifacts
- Fabricate labels
- Fabricate data
- Substitute unavailable data
- Rerun experiments under altered conditions and call them the same experiment
- Treat silver labels as human gold
- Treat exploratory results as confirmatory
- Change experiment definitions silently
- Delete historical evidence
- Resolve inconsistencies by rewriting earlier phase records
- Present Phase 3 Gate 4 as a blind test
- Use Phase 3 selected model P3-F3 for historical claims without disclosure

## FIRST ACTION FOR A NEW CLAUDE

**READ-ONLY AUDIT.** Do NOT immediately run experiments.

1. Clone the repository
2. Read this document (CLAUDE_HANDOFF_MASTER.md)
3. Read PROJECT_MASTER_INDEX.md
4. Read RESEARCH_HISTORY.md
5. Read phase READMEs (phase1–7)
6. Inspect manifests (provenance/SHA256_MANIFEST.csv)
7. Inspect protocol/decision records (phase6/protocols/)
8. Check repository status (RESEARCH_STATUS.md)
9. Verify hashes where possible
10. Report any inconsistencies found
11. **Ask for authorization before modifying anything**
