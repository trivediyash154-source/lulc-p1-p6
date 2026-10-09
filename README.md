# Pune District Earth Observation Research

### Multi-decadal land-use/land-cover change in Pune District, Maharashtra (1990–2026): a harmonised multi-sensor framework with pre-registered, design-based validation

| | |
| --- | --- |
| **Investigators** | Yash Trivedi, Darshil Zade |
| **Supervisor** | Renu Vaidya |
| **Institute** | Vishwakarma Institute of Technology, Pune |
| **Discipline** | Earth Observation and Remote Sensing / Geoinformatics |
| **Repository role** | Frozen research archive (Phases 1–6), Phase 7 reference material, and project documentation |
| **Status (9 October 2026)** | District data foundation complete and validated · human reference labelling is the next stage · confirmatory experiments not yet run |

> **Read this first.** This README is the complete guide to the project: what it is, why it exists, how every component works, where every file lives, how to reproduce the work, and what remains to be done. It is long on purpose. Anyone joining the project is expected to read it in full before touching the data, the code or the labelling kits. Section 19 lists what a new team member must read next and the rules that apply to everyone.

---

## Table of contents

1. [The project at a glance](#1-the-project-at-a-glance)
2. [Why this project exists](#2-why-this-project-exists)
3. [Research question, hypotheses and objectives](#3-research-question-hypotheses-and-objectives)
4. [Study area](#4-study-area)
5. [Data sources](#5-data-sources)
6. [Repository map](#6-repository-map)
7. [The seven research phases](#7-the-seven-research-phases)
8. [The processing pipeline, step by step](#8-the-processing-pipeline-step-by-step)
9. [Data product reference](#9-data-product-reference)
10. [Reference labels: gold, T1 and silver](#10-reference-labels-gold-t1-and-silver)
11. [Models and uncertainty](#11-models-and-uncertainty)
12. [Pre-registered confirmatory evaluation (C1–C6)](#12-pre-registered-confirmatory-evaluation-c1c6)
13. [Readiness gates](#13-readiness-gates)
14. [How to reproduce and run the work](#14-how-to-reproduce-and-run-the-work)
15. [Quality assurance and reproducibility evidence](#15-quality-assurance-and-reproducibility-evidence)
16. [Exploratory findings so far](#16-exploratory-findings-so-far)
17. [Limitations](#17-limitations)
18. [Roadmap](#18-roadmap)
19. [Guide for new team members](#19-guide-for-new-team-members)
20. [Frequently asked questions](#20-frequently-asked-questions)
21. [Glossary](#21-glossary)
22. [Citation, data credits and licence](#22-citation-data-credits-and-licence)

---

## 1. The project at a glance

This project builds a carefully tested, 30-metre record of how land in Pune District changed between 1990 and the present: cities, farmland, forest, water and bare land. It uses free satellite archives, machine learning and a strict evaluation method, so that the area and change figures it produces can be trusted for planning and decision-making.

### 1.1 One-paragraph summary

Pune District has transformed since 1990, yet freely available global land-cover products disagree by up to four times on its built-up area, provide no reliable 1990 baseline and rarely report local error bars. This project harmonises Landsat 5, 7, 8 and 9 surface reflectance with Sentinel-1 radar, a 30 m elevation model and CHIRPS rainfall on a single fixed 30 m grid covering the whole district. Older Landsat data are corrected to the modern sensor using locally fitted pseudo-invariant-feature regression. Random Forest and temporal convolutional networks classify five land-cover classes, with calibrated probabilities and conformal prediction sets. Six confirmatory experiments, whose rules were frozen before any reference data existed, test training-label quality, the old-sensor correction, era effects, transfer across rainfall regions, calibration and the value of radar. Accuracy is judged against a blind, double-interpreted human reference sample of 641 points in 125 independent blocks.

### 1.2 Status in numbers

| Component | Status |
| --- | --- |
| Study area | Whole Pune District: 17,380,091 cells of 30 m (≈ 15,640 km²) |
| Years in the district build | 18 (1990–92, 1998–2002, 2008–12, 2018–22) around four epochs: 1990, 2000, 2010, 2020 |
| Landsat composites (standard family) | 72 / 72 built and validated |
| Landsat composites (Landsat-5 corrected family) | 44 / 44 built and validated |
| Sentinel-1 radar composites (2020) | 16 / 16 built from 107 scenes |
| Terrain layers | 12 / 12 built from Copernicus GLO-30 |
| Feature cubes | 2 / 2 built and validated (≈ 24.7 GB each) |
| Files fingerprinted with SHA-256 | 34,965 files, 95.3 GB |
| Automated tests (live build) | 217 passing |
| Human gold reference labels | 0 / 641 (next stage) |
| Human training labels (T1) | 0 / 699 (next stage) |
| Confirmatory experiments C1–C6 | 0 / 6 run (blocked by design until labels are frozen) |

### 1.3 What is finished and what is not

**Finished:** the complete district-wide satellite archive, its quality layers, the sensor-corrected version, radar, terrain and the analysis-ready feature cubes; the sampling designs for the human reference and training samples; the blind interpreter kits; the frozen evaluation protocol; and the code for all six confirmatory experiments, tested on synthetic data.

**Not finished:** the human interpretation of reference points, the six confirmatory experiments, the validated district maps, the area and change estimates, the thematic analyses and the scenario work. No final accuracy figure exists yet, and none should be quoted. The reason is deliberate: an accuracy figure is only meaningful when it is measured against independent, human-checked reference data, and that labelling is the next stage.

---

## 2. Why this project exists

### 2.1 The place

Since 1990 Pune has grown from a mid-sized city into one of India's major metropolitan regions. Industry, information technology and population growth have physically rearranged the land:

- **The city has spread** outward from its core along the Mutha–Mula river corridor and the highway corridors, into farmland and the foothills.
- **Water** for the city comes from reservoirs such as Khadakwasla, Mulshi, Panshet and Varasgaon, whose extent swings with the south-west monsoon.
- **Farming** ranges from intensively irrigated sugarcane plains around Baramati to rain-fed land in the eastern rain-shadow talukas that suffers in drought years.
- **Forest** on the Western Ghats escarpment around Mulshi is ecologically sensitive and under development pressure.
- **Flooding** recurs along river corridors where built-up land meets low-lying terrain.

Planning authorities, water managers, disaster agencies, insurers and environmental regulators all need to know how much land changed, where, when, and how sure we are.

### 2.2 Why nobody can answer that question reliably today

| Obstacle | Evidence gathered in this project | Consequence |
| --- | --- | --- |
| Global land-cover products disagree | Built-up area in 2020 differs by up to 4× between products; three products agree on only 60 % of district cells; 1990 built-up agreement between products is only F1 0.32–0.43 | A planner must choose between incompatible numbers |
| No reliable historical baseline | Labels derived from 2020–21 maps and projected backwards mapped the 1990 city corridor as 50–74 % built-up, against 21 % in an independent settlement product | Change since 1990 is overstated or unknown |
| Satellite generations differ | On stable targets, Landsat-5 years read higher red and lower NDVI than Landsat-8/9 years; Sentinel-2's standard product is 20–50 % brighter than Landsat in the blue band | A change of camera can masquerade as a change of land |
| Monsoon cloud | June–September optical composites covered less than half of the district in several years | Monsoon-season processes are invisible to optical sensors |
| Models fail in new places | Macro-F1 of about 0.98 inside a familiar test area fell to 0.66–0.81 in unseen areas | Accuracy measured in one place does not hold district-wide |

### 2.3 What this project contributes

The scientific contribution is not a new algorithm. It is a **pre-registered, confirmatory test of choices that multi-decadal land-cover studies usually assume**: whether automatic training labels are good enough, whether a local correction makes old satellites comparable with new ones, whether models need era-specific handling, whether a model trained in one rainfall region works in another, whether predicted probabilities are honest, and whether radar adds value. These tests are run on a locally harmonised 30-year record and graded against a blind, statistically designed human sample.

The practical contribution is the first defensible, uncertainty-quantified baseline of land change for Pune District since 1990, together with a reproducible open pipeline that can be applied to other districts.

---

## 3. Research question, hypotheses and objectives

### 3.1 Central research question

> To what extent can multi-sensor Earth-observation time series (1990–2026), harmonised across heterogeneous satellite instruments and evaluated against design-based human reference data, quantify multi-decadal land-cover transformation in Pune District, distinguish climatic fluctuation from irreversible human land conversion, and provide defensible baselines for flood hazard and environmental exposure?

In plain words: can we build a 30-year record of how Pune's land changed that is accurate enough, and honest enough about its own uncertainty, to be trusted for planning, water, flood and environmental decisions?

### 3.2 Hypotheses

| No. | Hypothesis | Answered by |
| --- | --- | --- |
| H1 | Local cross-sensor harmonisation using pseudo-invariant features (PIF) and reduced-major-axis (RMA) regression provides enough radiometric stability to detect real multi-decadal change without sensor-transition artefacts. | C2, C3 |
| H2 | Spatial context and multi-temporal trajectory features improve class separation in complex peri-urban and semi-arid agricultural landscapes, compared with single-date spectral classification. | C1, C6 |
| H3 | Spatial cross-validation and conformal prediction reveal substantial uncertainty in rapidly changing and unfamiliar regions; deterministic point predictions overstate reliability under spatial transfer. | C4, C5 |

### 3.3 Objectives

1. Reconstruct a continuous, seasonally harmonised 30 m satellite record of Pune District across Landsat 5 TM, Landsat 7 ETM+, Landsat 8 OLI and Landsat 9 OLI-2, with Sentinel-1 radar for the recent era.
2. Design and collect an independent, stratified, double-interpreted human reference sample for accuracy and area estimation at four epochs (1990, 2000, 2010 and 2020).
3. Test, under pre-registered rules, which training-label sources, harmonisation choices and model designs give accurate and transferable land-cover maps.
4. Produce validated district land-use/land-cover maps and design-based area and change estimates with confidence intervals.
5. Quantify the trajectory, speed and form of urban expansion along the Mutha–Mula corridor and peripheral growth centres.
6. Characterise reservoir and surface-water dynamics in relation to monsoon rainfall, agricultural stability and Western Ghats forest persistence.
7. Derive flood-exposure proxies from radar backscatter and terrain (height above nearest drainage, slope) for riparian built-up areas.
8. Provide a validated empirical basis for future scenario modelling and a digital-twin foundation for the Pune region.

### 3.4 Land-cover classes

The project maps five Level-1 classes, with "other" reserved for clearly identifiable surfaces outside them:

| Class | Includes |
| --- | --- |
| Built-up | Roofs, paved surfaces, compounds, built structures |
| Agriculture | Field parcels, cropped or fallow, with visible parcel structure; orchards |
| Natural vegetation | Forest, scrub and grassland without parcel structure |
| Water | Open water visible at the observation date, permanent or seasonal |
| Bare / sparse | Rock, quarries, construction sites, bare soil without parcels |

---

## 4. Study area

### 4.1 The district and the analysis grid

| Property | Value |
| --- | --- |
| Area | Whole Pune District (administrative level 2), never a rectangle or only the city |
| Boundary source | geoBoundaries gbOpen, India ADM2, release commit `9469f09` (GADM 4.1 used only as a cross-check) |
| Official area (cross-check) | 15,643 km² (Census of India 2011) |
| Cells inside the district | 17,380,091 cells of 30 m × 30 m (≈ 15,640 km²) |
| Grid name | G30 |
| Grid size | 6,546 columns × 5,624 rows (≈ 196 km × 169 km bounding rectangle) |
| Map projection | UTM zone 43N (EPSG:32643), metres |
| Grid origin | x = 322,065 m, y = 2,146,035 m (upper-left corner) |
| Lattice | Native Landsat Collection 2 lattice: pixel corners at 15 m modulo 30 m, so Landsat is never resampled |
| Equal-area projection for area statistics | Asia South Albers Equal Area Conic (ESRI:102028) |

Other grids exist for specific purposes: **G10** and **G20** follow the native Sentinel-2/Sentinel-1 lattices, and **G240** (eight G30 cells) is a district-wide diagnostic grid used in Phase 1. G240 is never used for area statistics.

### 4.2 Landscapes and rainfall regions

The district runs from the high-rainfall Western Ghats escarpment in the west, across the Pune metropolitan corridor, to the semi-arid Deccan plateau and the Bhima basin in the east. Three rainfall regions derived from CHIRPS climatology structure the regional transfer tests:

| Region | Character |
| --- | --- |
| Wet west | Western Ghats escarpment: heavy monsoon, forest, hydropower reservoirs, steep terrain |
| Transition | Pune metropolitan corridor and surrounding plateau |
| Dry east | Rain-shadow plateau: rain-fed and canal-irrigated agriculture, drought exposure |

### 4.3 The three benchmark windows

The exploratory phases (1–3) worked intensively in three 30 m benchmark windows covering 5.8 % of the district:

| Window identifier | Landscape | Main themes |
| --- | --- | --- |
| `pune_G30_khadakwasla_mutha` | Urban and peri-urban corridor with reservoirs | Urban expansion, water, flood exposure |
| `pune_G30_baramati_agri` | Intensively cultivated semi-arid plain: sugarcane, canal-irrigated and rain-fed crops | Agriculture, drought, bare-soil confusion |
| `pune_G30_mulshi_ghats` | Steep forested escarpment with heavy monsoon and hydropower reservoirs | Forest persistence, terrain, cloud and radar shadow |

The final validation sample lies outside these windows plus a 2 km buffer, so the confirmatory evaluation is independent of everything learned in the exploratory phases.

---

## 5. Data sources

All satellite and elevation data are open and were accessed through the **Microsoft Planetary Computer** STAC API (`https://planetarycomputer.microsoft.com/api/stac/v1`). The exact list of scenes used for every product is recorded in that product's `metadata.json`.

### 5.1 Satellite and elevation data

| Data | Source | Collection | Original producer |
| --- | --- | --- | --- |
| Landsat 5 TM, 7 ETM+, 8 OLI, 9 OLI-2, Collection 2 Level-2 surface reflectance (Tier 1 only) | https://planetarycomputer.microsoft.com/dataset/landsat-c2-l2 | `landsat-c2-l2` | USGS / NASA (https://earthexplorer.usgs.gov) |
| Sentinel-1 radiometrically terrain-corrected backscatter γ⁰ (VV, VH) | https://planetarycomputer.microsoft.com/dataset/sentinel-1-rtc | `sentinel-1-rtc` | ESA Copernicus (https://dataspace.copernicus.eu) |
| Sentinel-2 Level-2A (exploratory phases) | https://planetarycomputer.microsoft.com/dataset/sentinel-2-l2a | `sentinel-2-l2a` | ESA Copernicus |
| Copernicus DEM GLO-30 | https://planetarycomputer.microsoft.com/dataset/cop-dem-glo-30 | `cop-dem-glo-30` | ESA / Airbus |

### 5.2 Global land-cover and reference products

| Data | Source | Use in this project |
| --- | --- | --- |
| ESA WorldCover 2020, 2021 | https://planetarycomputer.microsoft.com/dataset/esa-worldcover (`esa-worldcover`) | Silver training labels (with Esri) |
| Esri / Impact Observatory annual land cover | https://planetarycomputer.microsoft.com/dataset/io-lulc-annual-v02 (`io-lulc-annual-v02`) | Silver training labels (with WorldCover) |
| JRC Global Surface Water v1.4 (1984–2021) | https://planetarycomputer.microsoft.com/dataset/jrc-gsw (`jrc-gsw`) | Water benchmark |
| GLC_FCS30D (1985–2022) | https://zenodo.org/records/15063683 | Training-label filter (C1 arm C), benchmark, independent built-up envelope |
| GISA impervious surface (1972–2021) | https://zenodo.org/records/14848113 | Built-up benchmark |
| GHSL GHS-BUILT-S R2023A | https://jeodpp.jrc.ec.europa.eu/ftp/jrc-opendata/GHSL/ | Built-up benchmark |
| WSF-Evolution (1985–2015) | https://download.geoservice.dlr.de/WSF_EVO/ | Settlement-history benchmark |

### 5.3 Climate and boundaries

| Data | Source | Use |
| --- | --- | --- |
| CHIRPS v2.0 monthly rainfall (0.05°) | https://data.chc.ucsb.edu/products/CHIRPS-2.0/global_monthly/ | Rainfall regions, drought context |
| District boundary | https://github.com/wmgeolab/geoBoundaries (gbOpen IND ADM2, commit `9469f09`) | Study-area mask |
| Boundary cross-check | https://gadm.org (GADM 4.1, India level 2) | Comparison only (not redistributable) |

### 5.4 Human reference imagery

Human interpreters use the historical very-high-resolution imagery in **Google Earth Pro** (https://www.google.com/earth/versions/). Nothing is downloaded from it: interpreters look at the imagery and record their judgement in a form.

---

## 6. Repository map

### 6.1 Top level

```text
lulc-p1-p6/
├── README.md                    ← this guide
├── START_HERE.md                ← rules and reading order for anyone joining the project
├── HANDOFF_MASTER.md            ← mission, prohibitions, decision records, safety rules
├── PROJECT_MASTER_INDEX.md      ← master map of every artefact
├── CURRENT_RESEARCH_VISION.md   ← research vision, objectives, what is and is not established
├── RESEARCH_HISTORY.md          ← chronological narrative, Phase 1 → Phase 7
├── RESEARCH_STATUS.md           ← readiness gates and blockers (archive view)
├── LIVE_PHASE7_STATUS.md        ← status of the live Phase 7 execution environment
├── DATA_PROVENANCE.md           ← sensors, reference products, label tiers, lineage
├── REPRODUCIBILITY.md           ← what can and cannot be reproduced, and how
├── ARCHIVE_POLICY.md            ← how the archive is organised and preserved
├── CHANGELOG.md                 ← repository change history
├── phase1/ … phase7/            ← one folder per research phase
├── manifests/                   ← file inventories (master + per phase)
├── provenance/                  ← SHA-256 manifest, artefact registry, data lineage
├── shared/                      ← cross-phase resources
└── archive/                     ← top-level archive navigation
```

### 6.2 Inside every phase folder

Every `phaseN/` folder follows the same layout:

| Sub-folder | Contents |
| --- | --- |
| `README.md` | Purpose, research questions, inputs, methods, experiments, results, supported and rejected findings, limitations, decisions and key artefacts of the phase |
| `reports/` | The phase's reports and audits, in Markdown |
| `protocols/` | Frozen protocols, readiness gates, audits and decision records |
| `figures/` | Figures produced in the phase |
| `data/` | Compressed data packages (`.zip`), interpreter kits and repository snapshots |
| `archive/original_structure/` | Byte-identical copies of the original files, in their original layout, pinned by SHA-256 |

### 6.3 Important data packages

| Package | What it contains |
| --- | --- |
| `phase4/data/phase4_core.zip` | Phase 4 core outputs: protocol, sampling design, product benchmark |
| `phase5/data/phase5_core.zip` | Phase 5 outputs: pilot cube validation, execution plan, checklists |
| `phase5/data/phase5_pilot_cube_QA_layers_*.zip` | QA layers of the four pilot epochs |
| `phase6/data/phase6_gold_kit_INTERPRETER_A.zip` / `_B.zip` | Blind gold interpreter kits (form, cell outlines, protocol, metadata template) |
| `phase6/data/phase6_T1v2_kit_INTERPRETER_A.zip` / `_B.zip` | Blind T1 interpreter kits |
| `phase6/data/phase6_repository_snapshot.zip` | Code and configuration snapshot at the end of Phase 6 |
| `phase7/data/phase7_repository.bundle` | Full git bundle of the Phase 7 code repository |
| `phase7/data/phase7_repository_snapshot.zip` | Phase 7 code and configuration snapshot |
| `phase7/data/phase7_manifests.zip` | Phase 7 manifests |
| `*_KEY_evaluator_only.zip`, `phase6_KEYS_evaluator_only.zip` | Evaluator-only answer keys for the sampling designs. **Never give these to an interpreter.** |

The superseded Phase 4 and Phase 5 interpreter kits are kept for the record but must not be used: the Phase 6 kits replace them.

### 6.4 What is deliberately not in this repository

The full 95 GB district archive (composites, terrain, feature cubes) is too large for GitHub. It lives in the live Phase 7 build workspace and is pinned by a SHA-256 manifest of all 34,965 files. Everything in it can be regenerated from the code and the open data (Section 14), and the fingerprints prove that a regenerated file is identical to the original.

---
## 7. The seven research phases

The project advanced in seven phases. Phases 1–6 are **frozen**: their records are the authoritative scientific history and are never rewritten, including their failed experiments and negative results. Phase 7 is the active phase.

| Phase | Theme | Status |
| --- | --- | --- |
| 1 | Data foundation: compositing, cross-sensor comparison, QA | Frozen |
| 2 | Baseline land-cover modelling and thematic analyses (35 experiments) | Frozen |
| 3 | Systematic evaluation (63 experiments) and quality gates | Frozen |
| 4 | District design: frozen protocol, gold sample, pre-registration | Frozen |
| 5 | District pilot cube and infrastructure | Frozen |
| 6 | Decisions D1–D3, T1 design, blind interpreter kits | Frozen |
| 7 | District build, confirmatory runners, labelling preparation | Active |

### 7.1 Phase 1: data foundation

**Goal.** Build a multi-sensor, multi-decadal, analysis-ready archive for Pune.

**What was done.** Compositing pipelines for Landsat (1990–2026), Sentinel-2 (2018–2026) and Sentinel-1 (2017–2025) were built for the three 30 m benchmark windows, and district-wide at 240 m.

**What was learned.**
- 37-year Landsat dry-season and annual composites are reproducible and co-registered to better than 0.1 pixel.
- Sentinel-2 L2A (Sen2Cor) is systematically brighter than Landsat Collection 2 L2 (LaSRC) in every band, by 20–50 % in the blue band. The difference is a gain-type difference between processors, not a spatial effect, and which processor is closer to truth is unknown.
- Monsoon-season optical composites are not usable.
- District-wide products existed only at 240 m in this phase.

**What failed and was fixed.** An invalid water-shift registration test was replaced by image cross-correlation; a decoding bug in WSF-Evolution (valid 0 values bumped to 1 during warping) was fixed with an explicit no-data sentinel; a Sentinel-2 scene-classification no-data bug that overstated counts was fixed; QA was moved inside the compositing jobs.

### 7.2 Phase 2: baseline modelling

**Goal.** Train Level-1 classifiers, reconstruct historical maps and run thematic analyses (35 experiments).

**What was learned (exploratory, against silver labels).**
- Within-window macro-F1 ≈ 0.98, but leave-one-window-out macro-F1 only 0.66–0.81: a major transfer gap.
- Sentinel-1 radar adds information for unseen windows (macro-F1 0.70–0.88).
- Temporal smoothing with a hidden Markov model reduces false built-up.
- Drought creates false built-up in bare agricultural landscapes.
- Reservoir surface area correlates with monsoon rainfall at a lag of 0–1 year.
- Urban expansion modes (infill, edge, outlying) can be characterised.
- Crop types cannot be separated at 30 m (cluster silhouette < 0.25): agriculture forms a continuous greenness gradient.

**What failed.** Crop-type classification; 30 m district maps (only 240 m existed); a climate-data access route; flood-inundation maps (no radar acquisition on the cited event dates); river-width measurement at 30 m.

### 7.3 Phase 3: systematic evaluation

**Goal.** A comprehensive, gated evaluation: true baseline, historical failure, temporal context, radar and terrain, transferability, uncertainty, domain adaptation, agriculture, flood and model selection (63 experiments, gates G1–G6 in Phase 3 numbering).

**What was learned.**
- The 2021 baseline reached 0.979 agreement with silver labels, about 0.88 against provisional machine-proposed labels, and only 0.23–0.72 for built-up against GHSL. The 0.979 figure is therefore agreement with automatic labels, not accuracy.
- Radar and terrain improve transfer to unseen windows; multi-year labels and temporal context reduce false built-up.
- Conformal coverage fails when calibrated in one landscape and applied in another.
- Annual change-timing precision is not supported by pre-2013 observation densities.

**Rejected hypotheses.** A Sentinel-2→Landsat transform fitted in the city corridor failed in Mulshi and Baramati (P3-I1); domain classifiers did not predict where models would fail (P3-J1, 1 of 6 directions); unsupervised domain adaptation did not recover transfer losses, while a few local labels did (P3-J2); annual change timing (P3-K1, P3-K2).

**Gates not passed.** Gate 1 (no human reference labels), Gate 5 (calibration on unseen landscapes) and Gate 6 (reconstruction not defensible for district scaling). One model (P3-E5) was registered after its results were seen, so it was not a blind test; that lesson led directly to pre-registration in Phase 4.

### 7.4 Phase 4: district design

**Goal.** Design the infrastructure for district-wide inference before touching any validation data.

**What was done.**
- Evaluation protocol v1 → **v2**, frozen before any confirmatory run.
- Gold sample design v1 (672 points) → **v2 (641 points)** in independent blocks outside the Phase-3 windows.
- Six confirmatory experiments pre-registered (P4-C1@v2 … P4-C6@v2).
- Existing products benchmarked: up to 4× disagreement on 2020 built-up area, 60 % three-way agreement, disagreement highest on 3–8° slopes, in the transition rainfall zone and near water.
- Sensor-era analysis on pseudo-invariant targets: Landsat-5-dominated years read higher red and lower NDVI (confounded with time, few years per regime).

### 7.5 Phase 5: pilot cube

**Goal.** Build pilot district composites at 30 m for the four epochs and validate them.

**What was learned.** District pilot composites are bit-identical to the window composites where they overlap (3,846,463 cell-epochs, maximum difference 0); valid coverage ≥ 0.999999; pixels with ≥ 3 clear observations 0.958–1.000 across epochs; the Planetary Computer route works without Earth Engine credentials. A full district build and district terrain were not possible in the constrained environment of the time.

### 7.6 Phase 6: decisions and kits

**Decisions taken before any data existed.**
- **D1:** the training-label options for C1 were corrected. A hidden model-selection advantage was removed by scoring every option on the same human tuning labels, and a circularity was removed: GLC_FCS30D cannot both filter training labels and act as an independent benchmark.
- **D2:** the locally fitted PIF-RMA correction was adopted over global coefficients (Roy et al., 2016) because of Pune's distinctive soils and vegetation and the need for internal consistency within mixed-sensor epochs.
- **D3:** the compute route was fixed as the repository's own engine reading Planetary Computer data.

**Also done.** The T1 training sample was redesigned (v2) with a 2 km separation between training and tuning points; the interpreter kits were rebuilt as fully blind kits containing only the form, the cell outlines, a short protocol and a metadata template.

### 7.7 Phase 7: district build and confirmatory infrastructure (active)

**Done.**
- Confirmatory runner code for all six records, with fail-closed guards, tested on synthetic and known-answer data.
- Runner specifications adopted and countersigned before any label exists.
- The complete district build (Section 9), on a single Apple M4 laptop (10 cores, 16 GB memory): 146 products, 34,965 files, 95.3 GB, every file fingerprinted.
- Two definitional validator findings resolved by an additive validator version 2 that leaves the original validator untouched (Section 8.9).
- Interpreter kits verified clean and ready for hand-out.

**Next.** Human labelling of the gold and T1 samples, freezing and ingestion, then C1 followed by C2–C6.

> **Note on the `phase7/` folder.** The `phase7/` folder holds an earlier working snapshot made in a constrained environment (about 18 GB of free disk). It is reference material only. The live Phase 7 build described in this README was carried out afterwards in a dedicated environment and supersedes it.

---

## 8. The processing pipeline, step by step

Every stage of the pipeline records its inputs, its code version and a SHA-256 fingerprint of each output. Re-running a stage on the same inputs produces byte-identical files.

```text
 STAC catalogue ──► scene list (pinned) ──► read district pixels onto the 30 m grid
        │                                              │
        │                                   cloud / shadow / gap masking (QA_PIXEL)
        │                                              │
        │                         per-pixel median composites, 4 periods per year
        │                                              │
        │                     harmonisation: standard family + Landsat-5 corrected family
        │                                              │
        ├──► Sentinel-1 RTC composites (2020) ─────────┤
        └──► Copernicus DEM → 12 terrain layers ───────┤
                                                       │
                           product validators (completeness, ranges, fingerprints)
                                                       │
                               features → two validated Zarr feature cubes
                                                       │
                           SHA-256 manifest of every file (34,965 files, 95.3 GB)
```

### 8.1 Finding the scenes

The code queries the Planetary Computer STAC API for every Landsat scene that intersects the district in each year and period. Only Collection 2 **Tier 1** scenes are used; Tier 2 is excluded because its geometric accuracy is not suitable for time series. The resulting scene list is written to a manifest, so every later run uses exactly the same scenes. For scale: the 2022 annual composite drew on 115 selected scenes, the dry season on 65, post-monsoon on 37 and the monsoon on 13.

### 8.2 Reading the district onto the fixed grid ("cropping", done properly)

Satellite scenes are about 185 km × 180 km and Pune falls across several of them. The pipeline does not download whole scenes and crop them. It reads only the part of each Cloud-Optimised GeoTIFF that falls inside the district grid, in horizontal row strips (eight strips in the district build) so that memory use stays bounded, and places the pixels directly onto the G30 grid. Because G30 is the native Landsat lattice, Landsat pixels are never resampled. Cells outside the district boundary are set to no-data. The original scene files are never modified.

Why this matters: every product shares exactly the same grid, so cell number *n* is the same patch of ground in 1990 and 2020, in optical and radar, and in the terrain layers. A shift of even half a pixel would turn the edge of a road into apparent change.

### 8.3 Masking clouds, shadows and gaps

Each Landsat scene carries a quality band, `QA_PIXEL`, whose bits flag cloud, cloud shadow and fill. Flagged pixels are removed before any statistic is computed. The stripes of missing data in Landsat 7 imagery after May 2003 (the scan-line-corrector failure) are fill and are removed the same way. Nothing is ever interpolated: a missing pixel stays missing, and the observation count records how many clear looks each pixel received.

### 8.4 Compositing

For every pixel, the median of all clear observations within a period is computed. Four periods are produced every year:

| Period | Months | Purpose | Used by the models |
| --- | --- | --- | --- |
| Dry | January–May | Fewest clouds; the most reliable optical period every year | Yes (main input) |
| Monsoon (wet) | June–September | For completeness; optical is mostly cloud-blind, radar is the primary source | No |
| Post-monsoon | October–December | Crop state after the rains; maximum reservoir extent | Yes |
| Annual | January–December | Whole-year summary | Available |

A single date can be cloudy, hazy, post-harvest, drought-affected or striped. The per-pixel median over a fixed seasonal window gives a representative value that is comparable from year to year. Phase 1 showed that before 2013 there were often only 2–11 clear dry-season passes per year, so the seasonal composite, not the individual date, is the reliable unit of analysis.

### 8.5 Quality layers

Every composite is accompanied by layers that say how far it can be trusted: the number of clear observations per pixel, a valid mask with four codes, an uncertainty layer (the spread of the clear observations, computed only where at least three exist) and a multi-band QA stack. Their exact formats are listed in Section 9.

### 8.6 Cross-sensor harmonisation

Landsat 5 TM (1984–2011), Landsat 7 ETM+ (from 1999), Landsat 8 OLI (from 2013) and Landsat 9 OLI-2 (from 2021) are different instruments. Their spectral bands have slightly different widths and their detectors and calibration differ. Without correction, a change of instrument can look like a change of land.

Two harmonisation families are built:

| Family | Identifier | What it does | Products |
| --- | --- | --- | --- |
| Standard | `local_pune_g30_v2` | Landsat 7 ETM+ mapped to Landsat 8 OLI with a locally fitted reduced-major-axis regression; Landsat 5 and Landsat 9 left as delivered | 72 |
| Landsat-5 corrected | `c2_tm_pif_v1` | Additionally maps Landsat 5 TM to OLI with a pseudo-invariant-feature (PIF) regression fitted in Pune; frozen parameters, SHA-256 `782eaef3e95c9dbe4b79868d936fd1e96ff1aa681d93ad609b5463ddd119db47` | 44 (11 Landsat-5 years × 4 periods) |

**How the correction is found.**
1. Identify targets that did not change over the years: deep reservoir water and long-standing dense forest. These are pseudo-invariant features.
2. Compare what the older and newer sensors read on those targets.
3. Fit a straight line per band with reduced-major-axis regression, which treats both sensors as noisy.
4. Apply that line to every Landsat-5 pixel.

The correction is a pure linear map, deliberately without clipping. In two monsoon composites (1998 and 2011) it maps a few very bright pixels to 1.010 and 1.003, slightly above a reflectance of 1.0. These values were recorded, not clipped, and the monsoon layers are not used by the models.

**Why a local correction?** Published global coefficients average over the whole world, while Pune's red Deccan soils and its vegetation are spectrally distinctive. Phase 3 also showed that a correction fitted in one Pune landscape failed in another. Whether the local correction actually improves historical accuracy is not assumed: it is tested by experiment C2.

### 8.7 Sentinel-1 radar

Sentinel-1 is a C-band synthetic aperture radar. It sends microwave pulses and measures the echo, so it works through cloud and at night. The project uses radiometrically terrain-corrected γ⁰ backscatter in two polarisations: VV (sent and received vertically) and VH (sent vertically, received horizontally). Smooth open water reflects the pulse away and appears dark, while vegetation volume scrambles the polarisation and raises VH.

For 2020, 107 descending-orbit scenes produced 16 composites: annual, three seasonal (dry, monsoon, post-monsoon) and twelve monthly. One orbit direction is kept per year, and no incidence-angle normalisation is applied beyond the terrain correction. Six radar features enter the models:

| Feature | Definition | Purpose |
| --- | --- | --- |
| `s1_vv_dry` | VV backscatter, dry season (dB) | Surface structure |
| `s1_vh_dry` | VH backscatter, dry season (dB) | Vegetation volume |
| `s1_vh_wet` | VH backscatter, June–September (dB) | Kharif crop biomass while optical is blind |
| `s1_ratio` | VV − VH (dB) | Separates surfaces with similar brightness |
| `s1_vv_min` | Minimum monthly VV (dB) | Open water and flooded surfaces |
| `s1_vh_sd` | Standard deviation of monthly VH (dB) | Dynamic crops versus stable forest and built-up |

**Limitations.** Radar exists only from 2014, so it informs the 2020 epoch only. Steep Ghats slopes cause radar shadow, which is also dark. Soil moisture changes the backscatter.

### 8.8 Terrain

The Copernicus GLO-30 elevation model (nine tiles cover the district) is processed on a grid buffered by 3 km around the district, then cropped back to G30. Slope and aspect use Horn's method, curvature uses Zevenbergen–Thorne, and hydrology uses pit filling, flat resolution, D8 flow directions and flow accumulation. Twelve terrain bands are produced:

| Band | Meaning |
| --- | --- |
| `elevation_m` | Height above sea level |
| `slope_deg` | Steepness |
| `aspect_sin`, `aspect_cos` | Direction the slope faces, encoded without a wrap-around |
| `curv_profile`, `curv_plan` | Curvature along and across the slope |
| `tpi_150m`, `tpi_1050m` | Topographic position index: ridge, slope or valley at two scales |
| `flow_acc_km2` | Upstream area draining through the cell |
| `hand_m` | Height above the nearest drainage: a flood-exposure indicator |
| `dist_drainage_m` | Distance to the nearest stream |
| `twi` | Topographic wetness index |

A stream map and a metadata file accompany the terrain stack. **Limitation:** upstream area outside the 3 km buffer is not routed, so flow accumulation and HAND are underestimated where rivers enter the district.

### 8.9 Product validators

Before any feature is computed, automatic validators check every product: grid, completeness of the required years and periods, value ranges, metadata, harmonisation identifiers, transform fingerprints and byte-identity rules.

The original validator (`validate_district_cube.py`) is frozen and unchanged. On the district build it raised two definitional findings:

1. **Identical layers.** Some layers are byte-identical across products by construction: for example, uncertainty layers that are entirely empty where fewer than three clear observations exist, and valid masks equal to the district mask. The frozen rule counted every such group as an error.
2. **Values slightly above 1.0.** The two monsoon composites in the Landsat-5 corrected family described in Section 8.6.

Instead of editing the frozen validator or altering any data, an **additive validator version 2** was written in the live build. It refuses to run if the original validator's fingerprint has changed. It exempts only layers that are proven identical by construction, checks values against the bound implied by the frozen transform (with a warning above 1.0), and is otherwise as strict as the original. It has its own tests, and the pre-run checklists accept a version-2 report only if every pinned fingerprint matches. With it, both product families pass.

### 8.10 Features

A feature is a number that describes a cell and helps a model decide its class. For every cell and every year the feature cube stores 23 Landsat features:

| Feature | Formula or definition | What it captures |
| --- | --- | --- |
| `dry_blue`, `dry_green`, `dry_red`, `dry_nir`, `dry_swir1`, `dry_swir2` | Dry-season median surface reflectance | Raw spectral signal |
| `dry_ndvi` | (NIR − red) / (NIR + red) | Green vegetation; irrigated versus fallow land in the dry season |
| `dry_evi` | 2.5 (NIR − red) / (NIR + 6 red − 7.5 blue + 1) | Dense canopy without NDVI saturation |
| `dry_savi` | 1.5 (NIR − red) / (NIR + red + 0.5) | Sparse vegetation on bright soils |
| `dry_mndwi` | (green − SWIR1) / (green + SWIR1) | Open water |
| `dry_ndwi` | (green − NIR) / (green + NIR) | Water in vegetated settings |
| `dry_ndmi` | (NIR − SWIR1) / (NIR + SWIR1) | Canopy and soil moisture |
| `dry_ndbi` | (SWIR1 − NIR) / (SWIR1 + NIR) | Built-up and bare signal |
| `dry_bsi` | ((SWIR1 + red) − (NIR + blue)) / ((SWIR1 + red) + (NIR + blue)) | Bare soil, fallow, construction |
| `dry_nbr` | (NIR − SWIR2) / (NIR + SWIR2) | Disturbance, burning |
| `post_ndvi`, `post_mndwi`, `post_ndmi` | The same indices, October–December | Post-monsoon crop and water state |
| `d_ndvi`, `d_mndwi` | Post-monsoon minus dry-season value | Seasonal swing: rain-fed crops and seasonal water |
| `ann_ndvi` | Annual NDVI | Whole-year greenness |
| `sd_ndvi_5`, `sd_nir_5` | Standard deviation in a 5 × 5 (150 m) window | Texture: field mosaics and urban fabric versus uniform forest or water |

The confirmatory models use a pre-registered subset of 16 per year (called B3 in the code): five dry-season bands (blue, red, NIR, SWIR1, SWIR2), dry NDVI, MNDWI, NDMI and NBR, the two texture features, post-monsoon NDVI, MNDWI and NDMI, and the two seasonal differences. The Random Forest additionally receives a five-year window (t−2 … t+2), nine five-year summary statistics and the twelve terrain features: 101 inputs in total. Correlated features such as NDBI (exactly the negative of NDMI) are kept in the cube for transparency but excluded from the models.

### 8.11 Feature cubes

All features are stored in two Zarr feature cubes, one per harmonisation family:

| Variable | Dimensions | Contents |
| --- | --- | --- |
| `landsat` | year (18) × feature (23) × y (5,624) × x (6,546) | Landsat features, float32 |
| `quality` | year (18) × qfeature (3) × y × x | `n_dry`, `n_post`, `se_dry_nir` |
| `terrain` | tfeature (12) × y × x | The twelve terrain bands |
| `sentinel` | syear × sfeature (7) × y × x | Six Sentinel-1 features (2020) and one Sentinel-2 column that is empty because Sentinel-2 is not part of the district build |

Data are stored in 256 × 256-pixel chunks, which is why each cube holds 17,045 files. The standard cube (`pune_G30.zarr`) is about 24.7 GB; the corrected cube (`pune_G30_landsat_c2tm.zarr`) uses the corrected products for the Landsat-5 years and the standard products for all other years. Both pass their feature-cube validators (13 and 14 checks respectively), and every source product recorded in each cube matches the hash manifest.

### 8.12 Fingerprinting

Every output file carries a SHA-256 hash, a 64-character fingerprint computed from its exact bytes. Change one bit and the fingerprint changes completely. The complete district build (34,965 files, 95.3 GB) is listed in a hash manifest, and completeness was checked against two independent file listings (0 missing, 0 extra). The confirmatory runner refuses to start if any pinned fingerprint does not match.

---

## 9. Data product reference

### 9.1 Folder layout of the district build

```text
data/
├── composites/pune_G30/
│   └── <year>/
│       ├── landsat/          annual/ · seasonal/dry/ · seasonal/wet/ · seasonal/post_monsoon/
│       ├── landsat_c2tm/     same periods (Landsat-5 years only)
│       └── sentinel1/        annual/ · seasonal/<period>/ · monthly/<YYYY-MM>/   (2020)
├── features/
│   ├── terrain/pune_G30/     terrain.tif · streams.tif · metadata.json
│   └── cube/                 pune_G30.zarr · pune_G30_landsat_c2tm.zarr
└── composites_quarantine/    products quarantined during the 2022 provider defect (kept for the record)
```

### 9.2 Files in one Landsat product folder

Every Landsat product folder (for example `2000/landsat/seasonal/dry/`) contains six files. Every raster is 6,546 × 5,624 pixels on the G30 grid.

| File | Bands | Data type | Contents |
| --- | --- | --- | --- |
| `optical_composite.tif` | 6: blue, green, red, nir08, swir16, swir22 | int16 | Median surface reflectance × 10,000; no-data −32768 |
| `uncertainty.tif` | 6 | int16 | Spread of the clear observations × 10,000, where at least 3 exist; no-data −32768 |
| `observation_count.tif` | 1 | uint16 | Number of clear observations per pixel |
| `valid_mask.tif` | 1 | uint8 | 0 = outside district · 1 = no clear observation · 2 = one or two clear observations · 3 = three or more |
| `qa.tif` | several | uint16 | Observations seen, clear observations, percentage clear, clear observations per satellite, median day-of-year of the clear observations |
| `metadata.json` | – | JSON | Scenes used, sensors, harmonisation family, code version, QA summary and fingerprints |

**Reading a value.** A stored value of 2,800 in the NIR band means a surface reflectance of 0.28: the ground reflected 28 % of the incoming near-infrared light.

### 9.3 Files in one Sentinel-1 product folder

Each Sentinel-1 product folder contains eight files: mean and median backscatter for VV and VH (float32, dB, no-data −9999), the VV − VH difference, a temporal standard deviation layer, an observation count, a valid mask, a QA stack and `metadata.json`.

### 9.4 Sizes on disk

| Part | Size | Files |
| --- | --- | --- |
| Landsat composites, standard family | 21.39 GB | 432 |
| Landsat composites, Landsat-5 corrected family | 12.25 GB | 264 |
| Sentinel-1 composites (2020) | 8.46 GB | 128 |
| 2020 benchmark composites | 0.74 GB | 12 |
| Terrain | 1.95 GB | 3 |
| Feature cube, standard | 24.74 GB | 17,045 |
| Feature cube, corrected | 24.78 GB | 17,045 |
| **Total in these folders** | **94.3 GB** | **34,931** |

The fingerprint manifest covers 34,965 files (95.3 GB): the folders above plus 24 quarantined 2022 files and 10 external reference-product files.

### 9.5 Product-level quality notes

- Monsoon composites for 1990, 1991, 1992, 1999 and 2011 fail the product QA check (valid coverage below 50 % because of cloud). They are faithful outputs, nothing was filled, and the models do not use monsoon optical layers.
- In 2022 one Landsat 8 scene (5 November 2022) had a corrupt quality file at the provider: the server returned an error page instead of the image. The affected products were quarantined, the defect was recorded with its evidence, and the products were rebuilt without that scene exactly as the pre-written rules require; the rebuild was byte-identical across 20 of 20 files. Only the 2022 annual and post-monsoon composites lost that single observation (114 of 115 and 36 of 37 scenes used).

---
## 10. Reference labels: gold, T1 and silver

A machine-learning model learns from labelled examples, and its quality is measured against labelled examples. Labels therefore do three different jobs, and the project keeps them strictly apart:

| Job | Analogy | Labels used |
| --- | --- | --- |
| Training | The textbook | Silver labels and/or T1-train (525 points) |
| Model selection | The practice test | T1-tuning (174 points) |
| Accuracy and area estimation | The final exam, set by an independent examiner | Gold (641 points) |

If final-exam questions leak into the textbook, a high score means nothing. That is why gold points are never used for training or selection, and why training data are kept at least 2 km away from them.

### 10.1 Label tiers

| Tier | What it is | Size | Admissible for |
| --- | --- | --- | --- |
| **Tier A gold** | Blind human interpretation of very-high-resolution historical imagery | 641 points × 4 epochs; 160 points interpreted twice | Accuracy and area estimation only |
| **T1** | Human interpretation with the same protocol, in training-eligible areas | 699 points (525 train, 174 tuning); 140 interpreted twice | Training option and model selection |
| **Silver** | Automatic: cells where WorldCover 2020, WorldCover 2021, Esri 2020 and Esri 2021 all agree and the class covers ≥ 78 % of the cell | 8,155,297 cells (46.9 % of the district) | Training option only |
| **Provisional machine-proposed labels** | Labels proposed automatically for 437 points in Phase 3 | 437 points | Exploratory only; never a reference |

### 10.2 Why silver labels are not truth

- **They favour easy cells.** Unanimous agreement between four maps happens mostly in the interior of large uniform areas. Only 7–25 % of silver cells lie near edges, compared with 65–72 % in a properly drawn sample. A model can score very highly on silver cells and still fail on real, mixed edges.
- **They look backwards from 2020.** Applying 2020–21 labels to 1990 assumes nothing changed. Phase 3 showed the consequence: a 1990 city corridor mapped at 50–74 % built-up against 21 % in an independent product.
- **Some classes are almost absent.** Bare/sparse land has only 102 silver cells in the whole district. The silver class counts are: natural vegetation 3,706,418; agriculture 3,622,320; water 442,783; built-up 383,674; bare/sparse 102.

### 10.3 The gold sample design

| Property | Value |
| --- | --- |
| Points | 641 |
| Spatial blocks | 125 independent blocks of 6 km × 6 km |
| Strata | 13, over-sampling areas where existing products disagree |
| Rainfall regions | All three covered (about 190–260 points each) |
| Double interpretation | 160 points (25 %), labelled by two people who do not communicate |
| Separation | Outside the Phase-3 windows plus 2 km; at least 2 km from every training cell (minimum measured: 2,010 m) |
| Epochs | 2020, 2000, 2010, 1990, labelled in that order |
| Estimator | Design-based stratified estimators (Olofsson et al., 2014) with weights = stratum area / stratum sample size |

Because the sample is stratified and weighted, 641 points support district-wide accuracy and area estimates with confidence intervals, in the same way that a well-designed exit poll supports a national estimate with a margin of error.

### 10.4 The T1 training sample

T1 v2 has 699 points: 525 in the training population and 174 in tuning blocks. Tuning blocks are 6 km training-eligible blocks selected by a fixed hash rule, so selection cannot be influenced by the data. Every training point is at least 2 km from every tuning cell, and every T1 point is outside the validation blocks and at least 2 km from every gold point. 140 points are interpreted twice. The confirmatory checks require T1 labels for 2020 and 2000 at ≥ 80 % labelled in both splits; the forms also contain 2010 and 1990 columns, which nothing uses.

### 10.5 The blind interpreter kits

Each kit contains exactly four files: a form (point identifier, latitude, longitude and empty label columns for each epoch), a KML file of the 30 m cells, a two-page interpreter protocol, and a metadata template. Kits contain no answer key, strata, blocks, regions, product names, model outputs or the other interpreter's form. The B kits contain only the double-interpretation points, reshuffled, and their KML holds only those cells. Gold and T1 kits share no identifiers. The current kits are the Phase 6 kits; the earlier Phase 4 and Phase 5 kits are superseded.

### 10.6 How a point is interpreted

1. Open the KML in Google Earth Pro and double-click the point to fly to its 30 m cell.
2. Use the historical-imagery slider to choose an image within **±2 calendar years** of the epoch, preferring the closest date and, for ties, the dry season (January–May).
3. Record the image date and its source: `GEP_historical` (very-high-resolution imagery), `Landsat_context` (only coarse imagery available; maximum confidence 2), `field`, or `other_vhr`.
4. Record the dominant class, its share of the cell (≥ 75, 50–75 or < 50 %), a second class, a confidence from 1 to 3, a status (`done`, `unavailable` or `skipped`) and notes.
5. If no usable image exists, or the cell is hidden by cloud in every image, the status is `unavailable` and the class is left empty. **Never guess.** Many 1990 cells will legitimately be unavailable.
6. Work alone. Do not consult maps, products, models or the other interpreter.

`unavailable` counts as labelled for the quality gate; `uncertain` and `ambiguous` are recorded but do not count as labelled.

### 10.7 From returned forms to frozen labels

1. Returned forms and metadata files are copied unchanged into `raw_interpreter_A/` or `raw_interpreter_B/`.
2. When a pass is complete it is frozen: every raw, metadata and adjudication file is fingerprinted, and a freeze is immutable.
3. Ingestion validates everything and fails loudly on any violation: provenance (frozen, human, blind declaration, no declared product or automated source), identity (every identifier present, no duplicates, correct A/B subsets), geometry and leakage (coordinates equal to the design, cells inside validation blocks, ≥ 2 km from training cells, outside the Phase-3 windows), values (vocabularies, date window, confidence rules) and adjudication completeness. The ingestion code has 28 dedicated tests.
4. Every disagreement where at least one interpreter gave a definite class is adjudicated by a third person or by joint review, and the adjudication file is frozen like the raw files.
5. Raw files are never edited after hand-in. A genuine recording error is corrected only through a signed correction record, and the corrected file is frozen again under a new pass identifier; ingestion rejects any re-frozen file without such a record.

### 10.8 Quality gate G2

| Criterion | Requirement, for 2020 **and** 2000 |
| --- | --- |
| Labelled | ≥ 80 % of the 641 points have a resolved definite class or `unavailable` |
| Double-interpreted | ≥ 20 % of the 641 points have records from both interpreters |
| Agreement | Cohen's κ ≥ 0.6 over at least 30 double points where both gave a definite class |
| Adjudication | No unadjudicated point-epoch remains |

### 10.9 Labelling effort (planning estimate)

| Pass | Interpretations | Effort at 1–2 minutes each |
| --- | --- | --- |
| Gold 2020 + 2000 (A: 641 × 2, B: 160 × 2) | 1,602 | 27–53 hours |
| T1 2020 + 2000 (A: 699 × 2, B: 140 × 2) | 1,678 | 28–56 hours |
| Gold 2010 + 1990 | 1,602 | 27–53 hours |

---

## 11. Models and uncertainty

Machine learning is used here as a measuring instrument, not as a novelty. The models are well understood and testable, and the effort goes into honest evaluation.

### 11.1 Random Forest

An ensemble of decision trees, each trained on a random subset of the data and features, that votes on the class. Settings: 300 trees, `min_samples_leaf = 2`, `max_features = sqrt`; missing values are handled natively. Input: the 101-feature five-year window described in Section 8.10.

### 11.2 Temporal convolutional network (TempCNN)

A compact deep-learning model that reads each cell's 16 optical features as a five-year sequence (t−2 … t+2) and learns temporal patterns, for example "green every post-monsoon, bare every dry season" for rain-fed farmland. Inputs are standardised with training statistics; missing years are zero-filled with observation-mask channels. Training uses Adam (learning rate 10⁻³), batch size 512, at most 40 epochs and early stopping with patience 6. The early-stopping set is a fixed 10 % of training blocks chosen by a hash rule, never tuning or gold data.

### 11.3 Model selection

The training-label option and model family carried forward from C1 are chosen on the T1 tuning split only, using macro-F1 over the classes present at 2020 and 2000. Ties within 0.005 go to Random Forest before TempCNN, then to the earlier option. The choice is written to the run manifest before any gold metric is computed.

### 11.4 Calibration and conformal prediction

- **Calibration.** Predicted probabilities should be honest: when the model says 72 %, it should be right about 72 % of the time. Temperature scaling corrects over- or under-confidence; the expected calibration error (ECE, 15 equal-width bins) measures what remains.
- **Conformal prediction.** Instead of one class, the model returns a set of classes (for example {agriculture, natural vegetation}) that contains the true class at a chosen rate, here 90 %, provided new data resemble the calibration data. The primary method is LAC; APS is reported.
- **Abstention.** The model is allowed to decline its least confident 20 % of cases, and accuracy on the rest must actually rise.

Calibration and conformal thresholds are fitted on T1 tuning labels located outside the region being evaluated, then applied to that region's gold points.

### 11.5 Models explored earlier

Phases 2 and 3 also explored XGBoost, sequence transformers, LSTMs, hidden Markov smoothing and PELT changepoint detection. These results are exploratory. The confirmatory experiments use Random Forest and TempCNN as registered.

---

## 12. Pre-registered confirmatory evaluation (C1–C6)

### 12.1 Exploratory versus confirmatory

- **Exploratory** work tries many things, looks at results and adjusts. It is how ideas are discovered, and its results are real, but because choices were made after seeing results, they suggest rather than prove. Phases 1–3 are exploratory.
- **Confirmatory** work writes down the question, data, method, metric and pass/fail rule **before** any result exists, then runs exactly once. Only then can a hypothesis be called supported. C1–C6 are confirmatory.

### 12.2 Protocol v2 (frozen 2 October 2026)

| Rule | Meaning |
| --- | --- |
| Frozen before data | Protocol code and settings are fingerprinted and checked before every run (pre-run audit: 64 checks match) |
| Five seeds | Every model is trained with seeds 20261201–20261205; a result must hold in at least 4 of 5 |
| Paired block bootstrap | Treatment and comparator are compared on the same resampled gold units: stratified by gold stratum, resampling whole 6 km blocks, 2,000 resamples (seed 20261110) |
| Interval excludes zero | The improvement must be clearly different from zero |
| Multiple-testing control | Benjamini–Hochberg, q = 0.05, over each family of tests together |
| Design-based metrics | Accuracy and macro-F1 weighted by stratum area / stratum sample, recomputed on every resample |
| Outcomes | SUPPORTED, NOT SUPPORTED or INCONCLUSIVE only; INCONCLUSIVE means the evidence is incomplete, never "partly supported" |

### 12.3 The six experiments

| Record | Question | Comparison | Decisive evidence | Gold epochs |
| --- | --- | --- | --- | --- |
| **P4-C1@v2** | Does the training-label source change map accuracy? | Five arms: A silver labels for all epochs; B silver only inside each cell's change-free segment (PELT); C silver confirmed by GLC_FCS30D in 2020 and 2021; D human T1 labels; E = D ∪ C. Random Forest and TempCNN, five seeds | 64 paired tests (B−A, C−A, D−A, E−A × 4 epochs × 2 metrics × 2 model families); an epoch with < 100 usable gold points is not testable and enters the correction with p = 1 | 1990, 2000, 2010, 2020 |
| **P4-C2@v2** | Does the local Landsat-5 correction improve historical accuracy? | Same arm, model, rows, seeds and settings; only the feature cube differs (standard vs corrected) | Gold accuracy at 1990 and 2000 **and** district built-up share within the envelope of independent products (both decisive) | 1990, 2000 |
| **P4-C3@v2** | Does an era-aware model beat a single model in older sensor eras? | Era-aware approaches (per-era normalisation of features; per-era models where labels allow) versus one unified model | Gold accuracy per era: early historical (1990) and Landsat-dominant (2000 and 2010 pooled) | 1990, 2000, 2010 |
| **P4-C4@v2** | Does a model trained in two rainfall regions work in the third? | Leave-one-region-out versus within-region training | In every region: lower 95 % bound of macro-F1 ≥ 0.70 **and** gap ≤ 0.10; decisive for 2020 and 2000 | 2020, 2000 |
| **P4-C5@v2** | Are probabilities and prediction sets honest in new regions? | Calibration and conformal sets fitted outside the target region | Per region and epoch: ECE ≤ 0.05, 90 % set coverage ≥ 0.85, abstention raises accuracy; evaluated on all four epochs (a recorded scope deviation from the registered 2020 and 2000) | All four |
| **P4-C6@v2** | Do radar and terrain add value to optical data? | Random Forest, optical only versus optical + Sentinel-1 + terrain, leave-one-region-out | One paired macro-F1 test, 2020 | 2020 |

**Order.** C1 runs first and fixes the training option and model family used by C2–C6. The runner refuses C2 before C1 completes, C3 before C2, and so on.

### 12.4 Why each question matters outside academia

| Record | Practical question it answers |
| --- | --- |
| C1 | Are cheap automatic labels good enough, or must a mapping project pay for human labelling? |
| C2, C3 | Can 1990s data be trusted next to today's, for historical baselines and land-conversion audits? |
| C4 | Can a model built in one region be used in the next without relabelling? |
| C5 | Can a user trust the stated confidence of each prediction? |
| C6 | Is the extra cost of radar processing justified? |

### 12.5 Safeguards built into the runner

The confirmatory runner refuses to start unless: the protocol fingerprint matches; the gold and T1 labels are frozen; the pre-run checklist passes; the code tree is clean; the transform chain for C2 verifies; the runner registration matches the registered code; and the previous records in the order are complete. After the first label freeze, the registered runner code cannot be amended. Nobody, including the research team, can change the rules after seeing the data.

---

## 13. Readiness gates

Phase 7 has 20 readiness gates. The table shows the status recorded at the computational handoff on 7 October 2026. On 8 October the validator finding behind G4 was resolved with validator version 2; the pre-run checklist item for district-cube validation now passes for all six records.

| Gate | Condition | Status |
| --- | --- | --- |
| G1 | Protocol frozen | Pass |
| G2 | Human gold available and frozen | Not yet (no forms returned) |
| G3 | T1 available and frozen | Not yet |
| G4 | District feature cube complete | Resolved 8 October (see above) |
| G5 | Sentinel-1 complete where required | Pass |
| G6 | Automated labels validated and frozen | Pass |
| G7 | Decisions D1/D2 validated | Pass |
| G8 | Runner specification frozen | Pass |
| G9 | C1–C6 runners validated | Pass (synthetic, known-answer and fail-closed tests) |
| G10 | No leakage | Partial: design level passes; the run-time audit executes inside each run |
| G11 | Confirmatory configurations frozen before execution | Pass |
| G12–G17 | C1 … C6 completed validly | Not yet (blocked by G2, G3 and order) |
| G18 | Results frozen | Not yet |
| G19 | Reproducibility manifest complete | Not yet (needs labels and runs) |
| G20 | Final scientific report | Not yet |

Every gate that is not yet passed depends on one thing: independent human reference labels.

---
## 14. How to reproduce and run the work

This section explains how to obtain the code, set up the environment, run the tests, rebuild the district archive, and run the labelling and confirmatory workflows. Read Section 19.2 (ground rules) before running anything.

### 14.1 Requirements

| Item | Requirement |
| --- | --- |
| Operating system | macOS (Apple Silicon or Intel) or Linux |
| Python | 3.12, via the conda environment in `environment.yml` |
| Geospatial libraries | GDAL ≥ 3.8, rasterio, pyproj, shapely, geopandas |
| Array and data libraries | NumPy, SciPy, pandas, xarray, dask, Zarr, pyarrow |
| Data access | pystac-client, planetary-computer, odc-stac (internet access to the Planetary Computer; no special credentials) |
| Machine learning (optional extra `ml`) | scikit-learn, XGBoost, PyTorch |
| Memory | 16 GB was sufficient for the full district build (peak 6.64 GB with 8 workers) |
| Disk | ≈ 100 GB for the outputs, plus working space; keep at least 50 GB free during the build |

### 14.2 Getting the code

The code repository is preserved inside this archive as a git bundle:

```bash
git clone phase7/data/phase7_repository.bundle pune-eo
cd pune-eo
```

A plain snapshot of the same code is in `phase7/data/phase7_repository_snapshot.zip`, and the Phase 6 state is in `phase6/data/phase6_repository_snapshot.zip`. The live build workspace contains later commits (validator version 2, its tests and the updated runner registration); those are recorded in the live workspace's decision files and manifests.

### 14.3 Setting up the environment

```bash
conda env create -f environment.yml
conda activate pune-eo
pip install -e ".[ml,dev]"
```

### 14.4 Running the tests

```bash
python -m pytest -q
```

The live build workspace runs 217 tests (2 skipped for documented reasons). The tests cover the compositing engine, harmonisation, validators, feature cubes, label ingestion (28 tests), the confirmatory runners (synthetic data, known answers and fail-closed guards) and the evaluation statistics.

### 14.5 Rebuilding the district archive

These commands rebuild every product from the open data. They are the documented build sequence; run them in this order.

```bash
# 1. External reference products (needed for C1 arm C and the benchmarks)
python3 scripts/phase4_products_acquire.py

# 2. Spatial context: validation blocks and training eligibility, verified against frozen fingerprints
python3 scripts/phase7_restore_spatial_context.py

# 3. Landsat standard family: 18 years x 4 periods
Y=1990,1991,1992,1998,1999,2000,2001,2002,2008,2009,2010,2011,2012,2018,2019,2020,2021,2022
python3 scripts/phase5_district_cube_pilot.py --years $Y --periods annual,dry,post_monsoon,wet --threads 8 --min-strips 8

# 4. Landsat-5 corrected family: 11 Landsat-5 years x 4 periods
python3 scripts/phase5_district_cube_pilot.py \
  --years 1990,1991,1992,1998,1999,2000,2001,2008,2009,2010,2011 \
  --periods annual,dry,post_monsoon,wet \
  --transform c2_tm_pif_v1 --out-family landsat_c2tm --threads 8 --min-strips 8

# 5. Sentinel-1 (2020) and terrain
python3 scripts/phase6_district_s1.py --year 2020 --threads 8
python3 scripts/phase6_district_terrain.py

# 6. Product validation
python3 src/data/validate_district_cube.py --require min \
  --out results/phase5/cube_validation/validation_report_min.json
python3 src/data/validate_district_cube.py --family landsat_c2tm --require c2tm \
  --expect-harmonization c2_tm_pif_v1 \
  --expect-transform-sha256 782eaef3e95c9dbe4b79868d936fd1e96ff1aa681d93ad609b5463ddd119db47 \
  --out results/phase6/cube_validation_c2tm.json
python3 scripts/phase5_pilot_vs_windows.py

# 7. Feature cubes and their validation
python3 scripts/phase6_build_feature_cube.py && python3 src/data/validate_feature_cube.py
python3 scripts/phase6_build_feature_cube.py --family landsat_c2tm --fallback-family landsat && \
  python3 src/data/validate_feature_cube.py data/features/cube/pune_G30_landsat_c2tm.zarr --family landsat_c2tm

# 8. Pre-run checklists and runner preflight
python3 scripts/phase5_prerun_checklists.py
python3 scripts/phase7_run_confirmatory.py --preflight
```

The district silver labels are already built and frozen; they do not need to be rebuilt. A rebuild has been proven byte-identical to the frozen original (SHA-256 `32e1ddb0…`).

After a rebuild, compare every output with the SHA-256 manifest of the original build. Identical fingerprints prove an identical rebuild.

### 14.6 The labelling workflow, as commands

```bash
# Freeze a completed interpretation pass (gold), recording who froze it
python3 src/validation/ingest_gold.py --kind gold freeze <pass_id> --by <evaluator>

# Validate and assemble everything; exits non-zero with a list of every violation
python3 src/validation/ingest_gold.py --kind gold ingest

# Agreement, labelled shares and gate G2
python3 src/validation/audit_gold.py

# The same for the T1 training sample
python3 src/validation/ingest_gold.py --kind training freeze <pass_id> --by <evaluator>
python3 src/validation/ingest_gold.py --kind training ingest
python3 src/validation/audit_gold.py --t1

# Dataset freeze manifests for the confirmatory runs
python3 scripts/phase7_label_freeze.py --kind gold --frozen-by <evaluator>
python3 scripts/phase7_label_freeze.py --kind training --frozen-by <evaluator>
```

Label folders: gold in `data/labels/gold/phase4/` (`raw_interpreter_A/`, `raw_interpreter_B/`, `adjudicated/`, `metadata/`), T1 in `data/labels/training/phase6_T1v2/`.

### 14.7 Running the confirmatory experiments

```bash
python3 scripts/phase7_run_confirmatory.py --preflight   # shows what still blocks each record
python3 scripts/phase7_run_confirmatory.py P4-C1@v2      # runs first
python3 scripts/phase7_run_confirmatory.py P4-C2@v2      # then C2 ... C6, in order
```

Each run writes a manifest that records the protocol fingerprint, the label-freeze versions, the code version, the selected arm and model, every test result and the outcome.

### 14.8 Commands that must never be run

| Command | Why |
| --- | --- |
| `scripts/phase4_validation_design.py` | It rewrites the pinned gold key, forms and design. Use `phase7_restore_spatial_context.py` instead, which regenerates only the rasters and proves them against the frozen values. |
| Anything that writes into a frozen phase folder | Phases 1–6 are the frozen scientific record |
| Any edit to a returned interpreter form | Corrections go through the signed correction procedure only |

### 14.9 The exploratory pipeline (Phase 1, for reference)

The code also contains the original exploratory pipeline, driven by `make` targets: `setup`, `boundary`, `grid`, `discover` (scene discovery, coverage and timeline), `harmonize`, `landsat`, `sentinel`, `qa`, `figures`, `status` and `test`. These targets produce the 240 m district diagnostic products and are not part of the confirmatory path.

---

## 15. Quality assurance and reproducibility evidence

| Check | Evidence |
| --- | --- |
| Byte-identical rebuilds | District products bit-identical with 4, 6, 8, 9 and 10 workers, and identical to an x86 cloud reference run |
| Pilot versus windows | District pilot composites bit-identical to the window composites over 3,846,463 overlapping cell-epochs (maximum difference 0) |
| Complete fingerprinting | 34,965 files (95.3 GB) in the SHA-256 manifest; completeness checked against two independent listings (0 missing, 0 extra); 61 randomly chosen files re-hashed with 61 matches |
| Protocol integrity | Protocol v2 code (`3bb40f9c…`) and settings (`fe1fad6f…`) unchanged; pre-run audit 64 match, 0 mismatch |
| Silver labels | Rebuilt byte-identical to the frozen original; overlap agreement with the window silver 0.9968 / 0.9965 / 0.9993 |
| Spatial context | Validation blocks byte-identical to the frozen fingerprint; training eligibility pixel-exact |
| Defect handling | 2022 provider defect quarantined, documented and rebuilt by rule; nothing filled, interpolated or substituted |
| Tests | 217 passing in the live build; 28 for label ingestion; synthetic, known-answer and fail-closed tests for every runner |
| Frozen record | Every Phase 1–3 experiment kept, including failed and invalid ones (35 + 63 records) |

**What cannot be reproduced exactly.** The original git history of Phases 1–6 was lost when an early working container was reclaimed; the archive was restored from the Phase 6 snapshot, and the restoration was verified byte-for-byte where possible. The Planetary Computer catalogue can change over time, which is why every product's scene list is pinned. Earth Engine product versions used in the earliest phases are not pinned.

---

## 16. Exploratory findings so far

These findings come from Phases 1–4. They were measured against automatic or product references, not human gold, so they are exploratory: they guide the design but are not confirmed results.

| Finding | Value | Implication |
| --- | --- | --- |
| Within-area agreement with silver labels | Macro-F1 ≈ 0.98; 0.979 overall agreement in 2021 | High agreement with easy automatic labels, not accuracy |
| Transfer to unseen areas | Macro-F1 0.66–0.81 | Models lose a fifth to a third of their skill in new landscapes |
| Radar in unseen areas | Macro-F1 0.70–0.88 | Radar narrows the transfer gap; tested by C6 |
| Global product disagreement | Up to 4× on 2020 built-up area; 60 % three-way agreement | Existing maps cannot be used as truth |
| 1990 built-up between products | F1 0.32–0.43 | The historical baseline is the weakest point of existing products |
| Backward-projection bias | 1990 corridor 50–74 % built-up from silver vs 21 % in WSF | 2020 labels cannot simply be applied to 1990 |
| Sensor-era effect | Higher red, lower NDVI in Landsat-5 years on stable targets | Harmonisation needed; tested by C2 and C3 |
| Sentinel-2 vs Landsat | Sentinel-2 L2A 20–50 % brighter in blue; transform fails across landscapes | Sensor fusion needs landscape-specific checks |
| Clear observations before 2013 | 2–11 dry-season passes per year | Change dated by epoch, not by year |
| Crop types | Silhouette < 0.25 | Agriculture mapped as one class |
| Reservoir dynamics | Surface area tracks monsoon rainfall at 0–1 year lag | Basis for the water analysis |
| Calibration across landscapes | Conformal coverage fails when calibrated elsewhere | Region-wise calibration tested by C5 |

---

## 17. Limitations

- **Spatial resolution.** A 30 m pixel often mixes covers. Labels refer to the dominant class, and small buildings or narrow channels are not resolved.
- **Historical data density.** Before 2013 there were often only 2–11 clear dry-season observations per year, so change is dated by epoch, not by exact year.
- **Radar history.** Sentinel-1 exists only from 2014; radar informs the 2020 epoch only. Steep slopes cause radar shadow.
- **Crop types.** Not separable at 30 m with the available observations.
- **Flood work.** The project derives flood-exposure proxies (terrain and radar); it does not map specific flood events or water depth.
- **Causality.** Links between roads, infrastructure and urban growth will be reported as associations, not causes.
- **Terrain at the boundary.** Upstream area beyond the 3 km buffer is not routed, so flow accumulation and HAND are underestimated where rivers enter the district.
- **Reference data.** Very-high-resolution historical imagery is sparse before about 2003, so many 1990 and some 2000 gold points will legitimately be unavailable; epochs with fewer than 100 usable gold points are reported as not testable.

---

## 18. Roadmap

| Stage | Main tasks | Gate to pass | Indicative duration |
| --- | --- | --- | --- |
| 1. Human labelling | Gold 2020 + 2000 first, then 2010 + 1990; T1 2020 + 2000 | G2 and G3 | 2–3 months, depending on interpreter availability |
| 2. Freeze and checks | Freeze forms, ingestion checks, adjudication, agreement | Labels frozen | 2–4 weeks |
| 3. Confirmatory experiments | C1, then C2–C6; family-wise correction | All runner guards | About 1 month |
| 4. Maps and areas | District maps for four epochs; design-based area and change estimates | C1–C6 complete | 1–2 months |
| 5. Thematic analyses | Urban growth modes, water and rainfall, agriculture, forest, flood exposure | – | 3–4 months |
| 6. Synthesis | Thesis, journal articles, data and code release | – | 3–6 months |

**Planned outputs.**
1. Answers to the six pre-registered questions, with confidence intervals.
2. Validated 30 m land-cover maps for 1990, 2000, 2010 and 2020, with per-pixel probabilities and prediction sets.
3. Area and change estimates with 95 % confidence intervals, including a 1990 → 2020 transition matrix.
4. Thematic findings on urban growth, water, agriculture, Western Ghats forest and flood exposure.
5. A reusable human-validated reference dataset and an open, reproducible pipeline.
6. Three papers: the harmonised archive, the confirmatory evaluation, and Pune's land-change trajectories.

**Further horizon.** The validated historical change rates are the empirical foundation for scenario modelling (trend continuation, conservation zoning, transit-oriented growth) and for a digital twin of the Pune region. Neither exists yet.

---

## 19. Guide for new team members

### 19.1 Reading order

Read these documents in order before doing any work. Expect the full set to take several hours; that is normal.

1. This README, in full.
2. [START_HERE.md](START_HERE.md): rules and the authority hierarchy of documents.
3. [CURRENT_RESEARCH_VISION.md](CURRENT_RESEARCH_VISION.md): what the project investigates, what is established and what may not be claimed.
4. [HANDOFF_MASTER.md](HANDOFF_MASTER.md): prohibitions, decision records and safety rules.
5. [RESEARCH_HISTORY.md](RESEARCH_HISTORY.md): the chronological story of Phases 1–7.
6. [RESEARCH_STATUS.md](RESEARCH_STATUS.md) and [LIVE_PHASE7_STATUS.md](LIVE_PHASE7_STATUS.md).
7. [DATA_PROVENANCE.md](DATA_PROVENANCE.md) and [REPRODUCIBILITY.md](REPRODUCIBILITY.md).
8. Every `phaseN/README.md`, then the protocols in `phase4/protocols/` and `phase6/protocols/` (in particular the human gold workflow, the interpreter protocol and the T1 training protocol).
9. [PROJECT_MASTER_INDEX.md](PROJECT_MASTER_INDEX.md) and the manifests.

### 19.2 Ground rules

1. **Phases 1–6 are frozen.** Never edit, rename or delete anything in them, including failed experiments and negative results.
2. **Never invent data or labels.** No field, value or label is ever filled, interpolated or guessed.
3. **Automatic labels are never ground truth.** Silver labels, product maps and machine-proposed labels must never be used as gold.
4. **Exploratory is not confirmatory.** Never present a Phase 1–3 number as a validated accuracy. In particular, 0.979 is agreement with silver labels.
5. **Evaluator keys stay with the evaluator.** Never give a key file, a design file, product maps or this repository to an interpreter.
6. **Interpreters work blind and alone.** They receive only their own kit and never discuss cells with each other.
7. **The protocol is frozen.** Any change after data exist is a recorded deviation, decided by the lead researcher before the data it could affect.
8. **Every change is recorded.** Decisions go in decision files, repository changes in the changelog, and changed files are re-hashed in the manifests.

### 19.3 Roles

| Role | Responsibilities | Restrictions |
| --- | --- | --- |
| Lead researcher and evaluator | Design, decisions, freezing, ingestion, running experiments, interpretation of results | Does not interpret gold points |
| Gold interpreter A | All 641 gold points, four epochs | Must not see keys, products, model outputs or interpreter B's form |
| Gold interpreter B | The 160 double points, four epochs | Different person from A; same restrictions |
| T1 interpreters A and B | 699 and 140 T1 points, 2020 and 2000 | Preferably different people from the gold interpreters; otherwise separate sessions |
| Adjudicator | Resolves disagreements between A and B | A third person, or joint review by A and B |
| Code reviewer | Reviews changes to code outside the frozen record | Never changes registered runner code after the first label freeze |

### 19.4 Self-check before contributing

A new team member should be able to answer each of these without looking:

1. What are the four epochs, and why does the district build contain 18 years?
2. Why is the dry season January–May, and why are monsoon optical composites not used?
3. What is the G30 grid, and why must every product share it?
4. What does a stored value of 2,800 in `optical_composite.tif` mean?
5. What do the four valid-mask codes mean?
6. What is a pseudo-invariant feature, and why is the Landsat-5 correction tested rather than assumed?
7. What are the six radar features, and which one targets water?
8. What is HAND, and what can it not tell us?
9. What is the difference between gold, T1 and silver labels?
10. Why is 0.979 not an accuracy figure?
11. Why are training data kept at least 2 km from gold points?
12. What must an interpreter do when no usable image exists?
13. What are the three requirements of gate G2?
14. Why must C1 run before C2–C6?
15. What does INCONCLUSIVE mean under protocol v2?

### 19.5 Contribution workflow

1. Work on a branch; never commit directly into the frozen phase folders.
2. Write clear commit messages that say what changed and why.
3. Update [CHANGELOG.md](CHANGELOG.md) for every change to the repository.
4. Re-hash every changed file in `manifests/` and `provenance/` so the fingerprints stay true.
5. Record any research decision in a dated decision file before acting on it.
6. Ask the lead researcher before changing anything that a frozen record, a manifest or the runner depends on.

---

## 20. Frequently asked questions

**Why Landsat?** It is the only free, consistent 30 m archive that reaches back to 1990.

**Why 30 m and not sharper imagery?** 30 m is the finest resolution available consistently since 1990. Sharper imagery is used where it matters: by human interpreters for the reference labels.

**Why not just use WorldCover, Dynamic World or Esri?** They start around 2015–2020, are not validated specifically for Pune, and disagree with each other by up to 4× on Pune's built-up area.

**Why radar?** The monsoon blinds optical satellites for four months a year; radar sees through cloud and measures structure and wetness.

**Why not Google Earth Engine?** The same public data are available on the Planetary Computer, which worked without special credentials and lets every scene be pinned and fingerprinted. The method does not depend on the platform.

**Did you just crop the images?** No. Each scene is read onto one fixed 30 m grid aligned to the native Landsat lattice, masked for cloud and gaps, composited per season, harmonised and validated, and every output is fingerprinted.

**What does 0.979 mean?** Agreement with automatic silver labels inside familiar test areas in 2021. It is not accuracy; in unseen areas performance fell to 0.66–0.81.

**Where is the validation?** Designed and frozen: 641 points in 125 blocks, double-interpreted and blind. Human labelling is the next stage.

**Why haven't the final experiments been run?** They require independent human labels, and the runner refuses to start without frozen labels. That lock is deliberate.

**What if the experiments come out "not supported"?** That is a valid, publishable result, because the rules were fixed in advance.

**Can this work for another district?** The pipeline, yes: change the boundary and grid. Trust requires some local reference labels, and C4 measures how much a model loses when moved to a new region.

**How much does it cost?** The data are free and the build ran on one laptop. The main cost is expert labelling time.

**Can it detect the exact year of change?** No. Before 2013 there are too few clear images per year; change is dated by epoch.

**Can it predict floods?** No. It provides flood-exposure proxies from terrain and radar, not event maps or water depth.

**Why is the archive 95 GB?** About 17.4 million cells × 18 years × 4 seasons × 6 bands plus quality layers, radar, terrain and two complete feature cubes.

**Is anything in the repository machine-generated ground truth?** No. Every reference label used for accuracy will come from blind human interpretation.

---

## 21. Glossary

| Term | Meaning |
| --- | --- |
| Adjudication | Resolution of disagreements between two interpreters by a third person or joint review |
| Backscatter | Strength of the radar echo returning to the satellite |
| Band | One slice of the light spectrum recorded by a sensor |
| Benjamini–Hochberg | A correction that limits false discoveries when many tests are run |
| Block bootstrap | Re-drawing the sample by whole spatial blocks many times to estimate uncertainty |
| Calibration | Agreement between stated probabilities and observed frequencies |
| CHIRPS | Satellite-plus-gauge rainfall dataset at about 5 km |
| COG | Cloud-Optimised GeoTIFF, an image file that can be read in parts over the internet |
| Cohen's κ | Agreement between two labellers beyond chance |
| Composite | One image made from many dates; here the per-pixel median of clear observations |
| Confirmatory test | A test whose rules were fixed before the data existed |
| Conformal prediction | A method that outputs a set of classes containing the truth at a chosen rate |
| Co-registration | Alignment of images pixel for pixel |
| DEM | Digital elevation model: a height map |
| Design-based estimation | Estimates weighted by the sampling design, giving unbiased areas with confidence intervals |
| ECE | Expected calibration error |
| Epoch | A target year of analysis: 1990, 2000, 2010 or 2020 |
| Exploratory result | A real finding from flexible analysis that suggests rather than proves |
| Feature | A number describing a cell, used by a model |
| Feature cube | A store of every feature for every cell and year |
| G30 | The project's 30 m analysis grid |
| Gold label | Independent human-interpreted reference label used only for evaluation |
| HAND | Height above nearest drainage, a flood-exposure terrain indicator |
| Harmonisation | Making measurements from different sensors comparable |
| Leakage | Test information reaching the training data and inflating scores |
| LULC | Land use / land cover |
| Macro-F1 | An accuracy score averaging over classes so that rare classes count equally |
| MNDWI, NDVI, NDMI, NBR | Band-ratio indices for water, greenness, moisture and disturbance |
| NIR, SWIR | Near-infrared and shortwave-infrared light, invisible to the eye |
| PELT | A changepoint-detection method used to find change-free segments |
| PIF | Pseudo-invariant feature: a target that did not change, used to compare sensors |
| Pre-registration | Writing down and freezing a test before running it |
| QA_PIXEL | Landsat's per-pixel quality band |
| Random Forest | An ensemble of decision trees that vote on the class |
| Reflectance | Fraction of incoming light reflected by the surface (0–1) |
| RMA regression | Line fitting that treats both variables as noisy |
| RTC γ⁰ | Radar backscatter corrected for terrain |
| SAR | Synthetic aperture radar |
| Seed | Starting value for random processes |
| SHA-256 | A cryptographic fingerprint of a file |
| Silver label | Automatic label from the agreement of existing maps |
| Spatial cross-validation | Testing on whole held-out areas rather than random pixels |
| STAC | A standard catalogue format for searching satellite data |
| Stratified sampling | Sampling within defined groups, then weighting |
| T1 | The human-labelled training and tuning sample |
| TempCNN | A neural network that reads a cell's multi-year sequence |
| Transfer gap | The drop in performance when a model is used in a new landscape |
| VV, VH | Radar polarisations: vertical-vertical and vertical-horizontal |
| Zarr | A chunked storage format for very large arrays |

---

## 22. Citation, data credits and licence

### 22.1 Suggested citation

> Trivedi, Y., and Zade, D. (2026). *Pune District Earth Observation Research: multi-decadal land-use/land-cover change in Pune District, Maharashtra (1990–2026), a harmonised multi-sensor framework with pre-registered, design-based validation.* Vishwakarma Institute of Technology, Pune. Research archive: https://github.com/trivediyash154-source/lulc-p1-p6

### 22.2 Data credits

- Landsat data courtesy of the U.S. Geological Survey.
- Contains modified Copernicus Sentinel-1 and Sentinel-2 data, processed through the Microsoft Planetary Computer.
- Copernicus DEM GLO-30, provided under the Copernicus programme by the European Union and ESA.
- CHIRPS: Funk et al. (2015), Climate Hazards Center, UC Santa Barbara.
- ESA WorldCover: Zanaga et al. (2022), ESA WorldCover project.
- Esri / Impact Observatory annual land cover (Karra et al., 2021).
- GLC_FCS30D: Zhang et al. (2024).
- JRC Global Surface Water: Pekel et al. (2016).
- GHSL: European Commission, Joint Research Centre.
- WSF-Evolution: German Aerospace Center (DLR).
- District boundary: geoBoundaries.

### 22.3 Key references

- Olofsson, P., et al. (2014). Good practices for estimating area and assessing accuracy of land change. *Remote Sensing of Environment*, 148, 42–57.
- Stehman, S. V., and Foody, G. M. (2019). Key issues in rigorous accuracy assessment of land cover products. *Remote Sensing of Environment*, 231, 111199.
- Roy, D. P., et al. (2016). Characterization of Landsat-7 to Landsat-8 reflective wavelength and NDVI continuity. *Remote Sensing of Environment*, 185, 57–70.
- Ploton, P., et al. (2020). Spatial validation reveals poor predictive performance of large-scale ecological mapping models. *Nature Communications*, 11, 4540.
- Pelletier, C., Webb, G. I., and Petitjean, F. (2019). Temporal convolutional neural network for the classification of satellite image time series. *Remote Sensing*, 11(5), 523.
- Breiman, L. (2001). Random forests. *Machine Learning*, 45, 5–32.
- Angelopoulos, A. N., and Bates, S. (2023). Conformal prediction: a gentle introduction. *Foundations and Trends in Machine Learning*, 16(4), 494–591.
- Nobre, A. D., et al. (2011). Height Above the Nearest Drainage: a hydrologically relevant new terrain model. *Journal of Hydrology*, 404, 13–29.

### 22.4 Licence

No open-source licence has been granted for this repository. All rights are reserved by the investigators unless stated otherwise. Third-party data remain under their providers' licences, and the GADM boundary used for cross-checking may not be redistributed. For permission to reuse any part of this work, contact the investigators through Vishwakarma Institute of Technology, Pune.
