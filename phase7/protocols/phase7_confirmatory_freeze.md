# Phase 7 — confirmatory result freeze

_Generated 2026-10-03T01:22:45Z; git `73e2ebc`._

## No confirmatory experiment was executed, so there is no confirmatory result to freeze. This document freezes the STATE.

| # | item | value |
|---|---|---|
| 1 | protocol hash | code `3bb40f9c982d601db88f75913f7d00acb4944136a3f9771079b9413ec31a14ab`, YAML `fe1fad6f8b8eda1618302d6886ab8d90fbb65aefdf6da17f961aee93144cbdbe` (unchanged) |
| 2 | repository commit | `73e2ebc71f0848cf43b3221871bbecf930c3b5b9` |
| 3 | dataset hashes | district silver `32e1ddb07982a19832af9180a16643abe8c37f7ac3915e262c335a1c6297ad53`; T1 design `4058193bc6da90d81c42604a4834c0ef049d3f61c33b91236adb955e1d7d953e`; gold design `acc8692a3203944d7d791802b00c799dba75d82768db68d71a4c642608b48dd6` |
| 4 | gold hash | NOT AVAILABLE (NOT_AVAILABLE) |
| 5 | T1 hash | NOT AVAILABLE (NOT_AVAILABLE) |
| 6 | cube hash | NOT BUILT (pilot only; `results/phase7/manifests/cube/manifest.json`) |
| 7 | Sentinel-1 hash | NOT BUILT |
| 8 | D2 correction hash | `782eaef3e95c9dbe4b79868d936fd1e96ff1aa681d93ad609b5463ddd119db47` |
| 9 | experiment manifests | `results/phase7/manifests/P4-C{1..6}/manifest.json` (status DO NOT RUN); no run manifest exists |
| 10 | C1-C6 results | none |
| 11 | statistical corrections | none applied (no test was run) |
| 12 | failed / invalid runs | none (no run was attempted) |
| 13 | deviations | none from protocol v2. Phase-7 notes: (a) district-silver validation criterion revised before any label (see cube report); (b) runner specifications PROPOSED, not adopted; (c) the D1/D2 countersignature was given in the session and recorded by the lead; (d) the session container was reclaimed on 2026-10-03: the repository was restored from the Phase-6 snapshot, the Phase-7 code re-applied from the transcript, the spatial context regenerated (validation blocks byte-identical; eligibility pixel-exact on the T1 footprint + exact statistics) and the district silver rebuilt byte-identical (docs/phase7_entry_audit.md Part B), the countersignature re-materialised with its original signing time |
| 14 | preregistered hypotheses | below (verbatim) |
| 15 | hypothesis status | NOT TESTED for all six — 'not supported' and 'inconclusive' are reserved for executed tests |

| record | hypothesis (verbatim) | status |
|---|---|---|
| P4-C1@v2 | training-label quality and temporal design change gold accuracy of historical maps: arms C (independently validated), D (human gold-training), E (gold + filtered silver) each differ from A (single-year silver); B (multi-year silver) differs from A | NOT TESTED (not run: prerequisites missing) |
| P4-C2@v2 | replacing the identity TM transform with a cross-sensor TM/ETM+->OLI transform (published coefficients or PIF-fitted on non-validation blocks) reduces early-era false built-up and improves 1990/2000 gold accuracy | NOT TESTED (not run: prerequisites missing) |
| P4-C3@v2 | an era-aware approach (era-specific normalisation or era-specific models) beats one unified model on gold accuracy in the early-historical and Landsat-dominant eras | NOT TESTED (not run: prerequisites missing) |
| P4-C4@v2 | leave-one-region-out gold macro-F1 of the selected model is >= 0.70 (CI lower bound) with a within-minus-held-out gap <= 0.10 in every rainfall region (Gate G6) | NOT TESTED (not run: prerequisites missing) |
| P4-C5@v2 | on gold, calibration fitted on non-target regions reaches ECE <= 0.05 and 90 % conformal coverage >= 0.85 in every region and era; abstaining on the least confident 20 % raises accuracy (Gate G7) | NOT TESTED (not run: prerequisites missing) |
| P4-C6@v2 | optical + SAR + terrain beats optical-only on gold macro-F1 in leave-one-region-out (S1 era, 2020) | NOT TESTED (not run: prerequisites missing) |
