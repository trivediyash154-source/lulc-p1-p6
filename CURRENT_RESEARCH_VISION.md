# Current Research Vision: Pune Earth Observation PhD

This document defines the current research vision. It does not replace, rewrite, or reinterpret the frozen historical records of Phases 1–6. Historical records remain authoritative for what was actually tested, supported, rejected, or unavailable at each stage.

---

## SECTION A: WHAT THE PROJECT INTENDS TO INVESTIGATE

### 1. Project Identity & Scientific Scope
The project is a doctoral research inquiry in Earth Observation and Remote Sensing investigating multi-decadal landscape transformation in the Pune district, Maharashtra, India. The project integrates multi-sensor optical and radar satellite time series with rigorous machine learning and statistical uncertainty quantification to build a defensible, empirical spatial history of land-use/land-cover (LULC) dynamics.

### 2. Study Region & Geographic Domain
The spatial focus is **Pune District (~15,642 km²)**, characterized by sharp topographical, hydroclimatic, and anthropogenic gradients:
- **Khadakwasla–Mutha Corridor (`pune_G30_khadakwasla_mutha`):** The urban–peri-urban transition zone containing Pune city, expanding suburban fringes, and critical reservoir networks.
- **Baramati Agricultural Plain (`pune_G30_baramati_agri`):** Intensively cultivated semi-arid agricultural plain dominated by sugarcane, canal irrigation, and rainfed cropping.
- **Mulshi–Western Ghats (`pune_G30_mulshi_ghats`):** High-relief, ecologically sensitive Western Ghats escarpment characterized by dense moist/dry deciduous forests, heavy monsoon precipitation, and hydro-power reservoirs.
- **District-Wide Domain:** District-scale multi-decadal analysis bridging localized 30 m benchmark windows to regional 240 m and 30 m synthesis.

### 3. Research Motivation
Pune has undergone rapid economic, infrastructural, and demographic growth over the past four decades, causing substantial spatial restructuring: conversion of agricultural and natural land to built environments, pressure on catchment headwaters, hydrological modification of riparian zones, and recurring flood and drought vulnerabilities. Quantifying these changes with observational rigor has been historically impeded by cloud contamination during monsoon months, sensor discontinuities across satellite generations, and over-reliance on uncalibrated global products.

### 4. Central Research Question
*To what extent can multi-sensor Earth observation time series (1990–2026), harmonised across heterogeneous satellite instruments and evaluated against design-based human reference data, quantify multi-decadal land-cover transformations, distinguish climatic fluctuations from irreversible anthropogenic land conversion, and provide defensible empirical baselines for flood hazard and environmental exposure across the Pune district?*

### 5. Overarching Research Hypotheses
1. Cross-sensor surface reflectance harmonisation using localized pseudo-invariant features (PIF RMA) provides radiometric stability sufficient to detect true multi-decadal land-cover change without sensor-transition artifacts.
2. Incorporating spatial context and multitemporal trajectory features into gradient-boosted trees and deep sequence models significantly improves class separation in complex peri-urban and semi-arid agricultural interfaces compared to single-date spectral classification.
3. Conformal prediction sets and spatial cross-validation reveal substantial epistemic uncertainty in regions undergoing rapid transition, demonstrating that deterministic point predictions overestimate classification reliability in spatial transfer scenarios.

### 6. Specific Scientific Objectives
- To reconstruct a continuous, seasonally harmonised satellite record from 1990 to 2026 across Landsat 5 TM, Landsat 7 ETM+, Landsat 8 OLI, Landsat 9 OLI-2, Sentinel-2 MSI, and Sentinel-1 SAR.
- To quantify the spatial trajectory, velocity, and morphology of urban expansion along the Mutha river and surrounding development corridors.
- To characterize multi-decadal surface water dynamics and storage variability across major reservoirs (Khadakwasla, Mulshi, Panshet, Varasgaon).
- To evaluate agricultural land-use stability, cropping intensity patterns, and fallow-cycle responses to historical drought events.
- To measure forest canopy persistence, seasonal phenology, and degradation risk across the Western Ghats transition zone.
- To model inundation proxies and exposure zones along riparian corridors combining synthetic aperture radar backscatter and terrain descriptors.

### 7. Multi-Sensor Data Strategy
- **Landsat Series (1990–2026, 30 m):** Landsat 5 TM (1990–2011), Landsat 7 ETM+ (1999–2026, with SLC-off QA handling post-2003), Landsat 8 OLI (2013–2026), Landsat 9 OLI-2 (2021–2026).
- **Copernicus Sentinel-2 MSI (2018–2026, 10–20 m resampled to 30 m):** High-frequency multi-spectral observations for recent multi-seasonal phenology and cross-sensor validation.
- **Copernicus Sentinel-1 SAR (2017–2025, C-band, VV/VH):** Cloud-penetrating microwave backscatter used to observe surface roughness, moisture, and monsoon hydrology when optical sensors are blinded.
- **Ancillary Hydrometeorological & Topographic Data:** CHIRPS daily/monthly rainfall (1981–2026, 0.05°), SRTM/Copernicus 30 m DEM elevation and slope metrics.
- **Global Reference Layer Baselines:** GHSL (1975–2030), WSF-Evolution (1985–2015), JRC Global Surface Water (1984–2020), ESA WorldCover (2020/2021), Dynamic World, and GLC_FCS30D.

### 8. Multi-Decadal Radiometric Harmonisation
To investigate multi-decadal surface changes without false drift, the research applies rigorous radiometric calibration:
- Atmospheric correction using standardized surface reflectance archives.
- Empirical cross-sensor harmonisation utilizing local Pseudo-Invariant Features (PIF) with Reduced Major Axis (RMA) regression to align TM and ETM+ to the OLI radiometry.
- Explicit masking of clouds, cloud shadows, and Landsat 7 SLC-off gaps using QA band fusion.

### 9. Land-Use / Land-Cover Transformation Analysis
To classify and trace five canonical Level-1 (L1) land-cover categories across time:
1. Built-up / Impervious Surface
2. Agricultural Land (Cropped & Fallow)
3. Natural Vegetation (Forest & Shrubland)
4. Water Bodies (Permanent & Seasonal)
5. Bare Ground / Sparse Vegetation

### 10. Water Bodies, Rivers, and Reservoir Dynamics
To track surface water extent, seasonal expansion/contraction cycles, and multi-decadal storage area variability across the Mutha, Mula, and Bhima river networks and major reservoirs, evaluating correlations with annual monsoon precipitation deficits.

### 11. Agriculture & Crop Pattern Dynamics
To investigate seasonal greenness dynamics, double-cropping vs single-cropping prevalence, and fallow frequency in rainfed and irrigated zones, testing whether climatic anomalies produce temporary spectral confusion between barren farmland and cleared land.

### 12. Forest & Natural Vegetation Persistence
To measure vegetative stability in the Western Ghats escarpment, assessing moist deciduous versus evergreen forest boundaries, phenological green-up amplitude, and potential fragmentation along highway and development corridors.

### 13. Urban Expansion & Spatial Morphology
To map the spatial extent, density, and directional axes of urban outward growth from Pune's core into peripheral agricultural and forested panchayats, separating contiguous infill from leapfrog peri-urban sprawl.

### 14. Flood Inundation & Exposure Proxies
To evaluate radar backscatter anomalies (Sentinel-1 VV/VH ratio shifts and backscatter decrease) integrated with Height Above Nearest Drainage (HAND) and slope models to identify flood-prone reaches along the Mutha river corridor.

### 15. Environmental Stress & Drought Response
To assess the impact of historical meteorological droughts (e.g., 2002–2003, 2015–2016) on surface water retention, crop failure patterns, and vegetation health indices across Pune's rain-shadow talukas.

### 16. The Role of AI and Machine Learning
In this research, **AI and Machine Learning (Random Forests, XGBoost, Temporal CNNs, Sequence Transformers) are employed strictly as scientific measurement instruments**, not as standalone novelties. The scientific goal is not benchmark score chasing, but extracting robust, calibrated biophysical and land-cover signals while quantifying model epistemic uncertainty.

### 17. Uncertainty Quantification & Validation Framework
To reject reliance on unverified heuristic accuracy:
- Utilizing design-based probability sampling (stratified random sampling with spatial buffering).
- Formally distinguishing silver pseudo-labels (product consensus) from human-interpreted gold labels.
- Generating conformal prediction sets to provide mathematically guaranteed coverage under valid exchangeability assumptions.
- Rigorous spatial cross-validation (Leave-One-Window-Out) to test geographic generalization.

### 18. Driver and Interaction Analysis
To statistically model associations between observed land-cover transitions and candidate spatial drivers: distance to arterial roads, topographic slope, proximity to transit hubs, administrative boundaries, and water availability.

### 19. Future Scenario Analysis
To explore prospective land-allocation scenarios (trend extrapolation, conservation zoning, transit-oriented growth) using constrained spatial allocation models parameterized by historical change rates.

### 20. Digital Twin Foundation
To serve as an empirical, data-driven remote sensing core for a future urban-environmental Digital Twin of the Pune metropolitan region.

---

## SECTION B: WHAT HAS ALREADY BEEN ESTABLISHED (PHASES 1–6)

The following findings represent the **frozen, empirical scientific record** produced in Phases 1 through 6:

1. **Multi-Sensor Optical Foundation (Phase 1):**
   - Successfully constructed 37-year cloud-screened dry-season Landsat composite time series (1990–2026) for the 3 benchmark windows at 30 m and district-wide at 240 m.
   - Identified that pre-2013 observation frequency is sparse (2–11 clear dry-season passes per year), restricting reliable temporal analysis to seasonal composites rather than dense time steps.

2. **Baseline L1 Modelling & Thematic Analysis (Phase 2):**
   - Trained initial baseline classifiers on silver pseudo-labels (multi-product consensus from GHSL, WSF, JRC GSW).
   - Documented severe 1990 built-up overestimation in silver consensus data (corridor mapped 50–74% built-up vs WSF 21%), proving that global consensus products suffer severe historical backward-projection bias.

3. **Systematic 63-Experiment Evaluation & Rejection of Hypotheses (Phase 3):**
   - **Hypothesis P3-I1 REJECTED:** Sentinel-2 to Landsat OLI spectral transformation fitted on the Khadakwasla corridor completely failed when transferred to the Mulshi or Baramati landscapes. Cross-sensor transforms cannot be assumed spatially invariant.
   - **Hypothesis P3-J1 REJECTED:** Domain probability classifiers failed to predict spatial generalization errors across landscapes (predicted in only 1 of 6 directions).
   - **Hypothesis P3-J2 REJECTED:** Standard unsupervised domain adaptation techniques (CORAL, importance weighting, self-training) failed to recover spatial transfer losses; only few-shot local labels succeeded.
   - **Hypotheses P3-K1/K2 REJECTED:** Annual precision for land-change event timing was observationally unsupportable with pre-2013 Landsat observation densities.
   - **Readiness Gates 1, 5, and 6 NOT PASSED:** District-scale spatial extrapolation failed quality gates.

4. **Protocol v2 Pre-Registration & Gate Hardening (Phase 4):**
   - Formally locked all confirmatory evaluation protocols (P4-C1@v2 through P4-C6@v2) prior to touching validation data.
   - Designed a design-based, human-interpreted validation sample: 641 points across 125 spatial blocks, with two independent interpreters and a blind adjudicator.

5. **District Silver Pilot & Sensor Correction Decisions (Phases 5 & 6):**
   - Built a 4-epoch district pilot cube (1990, 2000, 2010, 2020) at 30 m, demonstrating pipeline scalability.
   - **Decision D1:** Confirmed P4-C1@v2 protocol rules.
   - **Decision D2:** Formally adopted localized PIF RMA cross-sensor correction over global Roy (2016) coefficients due to local soil and vegetative spectral distinctiveness.
   - **Decision D3:** Selected Route A (in-situ compute on Planetary Computer) for full district cube generation.

---

## SECTION C: WHAT REMAINS TO BE TESTED & CANNOT BE CLAIMED

The following items are **strictly unverified, pending, or explicitly barred from being claimed as established facts**:

1. **No Human Gold Accuracy Can Be Claimed:**
   - 0 of the 641 Tier-A validation points have been interpreted by human interpreters.
   - All performance figures in Phases 1–3 are measured against silver consensus or automated pseudo-labels. **Claiming verified real-world ground truth accuracy is strictly prohibited.**

2. **Confirmatory Hypotheses Remain Pending:**
   - Pre-registered confirmatory experiments P4-C1@v2 through P4-C6@v2 have NOT been executed on real gold data. Their status is **PENDING / DO NOT RUN**.

3. **District-Wide 30 m Wall-to-Wall Map Is Not Established:**
   - The verified 30 m analysis covers only the three benchmark windows (5.8% of the district). District-wide 30 m scaling is unexecuted.

4. **Discrete Crop Types Are Not Separable:**
   - Unsupervised clustering of agricultural phenology yielded silhouette scores < 0.25, reflecting a continuous vigor gradient rather than distinct crop taxonomy.

5. **Causal Urbanization Drivers Cannot Be Claimed:**
   - Observed spatial correlations between road corridors and built-up land cover do not constitute proven causal drivers; formal econometric or spatial econometric structural models have not been validated.

6. **Monsoon Cloud Penetration by Optical Sensors Is Impossible:**
   - Optical records during peak monsoon (July–August) are absent due to persistent cloud cover. Monsoon hydrological claims can only be explored using Sentinel-1 SAR (available only post-2017).
