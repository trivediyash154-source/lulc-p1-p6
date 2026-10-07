# Phase 7 — entry audit

## Part A — original Phase-7 entry audit (2026-10-02T18:48:24Z), transcribed verbatim from the session record

The original file was lost when the session container was reclaimed on 2026-10-03. The text below is the tool output that displayed it; the lines after the T1-design hash were not displayed and are not reproduced.

> # Phase 7 — entry audit (refreeze check)
> 
> _Generated 2026-10-02T18:48:24Z by `scripts/phase7_entry_audit.py` (read-only), before any Phase-7 change. Reference: the Phase-6 frozen state (`results/phase6/reproducibility_manifest.json`, commit `7b85b69`)._
> 
> ## RESULT: **OK - frozen Phase-6 state reproduced**
> 
> | check | result | detail |
> |---|---|---|
> | commit_is_phase6_final | PASS | {"head": "7b85b69a27c648d3883d3e792bfca0efb8cd3eb7"} |
> | working_tree_clean | PASS | {"status": ["?? results/phase7/", "?? scripts/phase7_entry_audit.py"]} |
> | protocol_v2_code_hash | PASS | {"value": "3bb40f9c982d601db88f75913f7d00acb4944136a3f9771079b9413ec31a14ab"} |
> | protocol_v2_yaml_hash | PASS | {"value": "fe1fad6f8b8eda1618302d6886ab8d90fbb65aefdf6da17f961aee93144cbdbe"} |
> | protocol_files_unchanged_since_phase4 | PASS | {} |
> | registry_G8_check_confirmatory_records | PASS | {"n": 6} |
> | confirmatory_records_all_PENDING | PASS | {"status": {"P4-C1_at_v2": "PENDING", "P4-C2_at_v2": "PENDING", "P4-C3_at_v2": "PENDING", "P4-C4_at_v2": "PENDING", "P4-C5_at_v2": "PENDING", "P4-C6_at_v2": "PENDING"}} |
> | phase6_manifest_files_unchanged | PASS | {"n": 46, "differences": {}} |
> | registry_records_unchanged | PASS | {"n": 21, "differences": {}} |
> | D1_addendum_and_erratum_match_record_pointers | PASS | {"addendum": "db13cfae510bb5b04bb748fbcd95729f14faf55fd844fb397599faf73cd4c0ea", "erratum": "6f13897f6e2128febb79b8bf46472a56dbeb352c0786d9beae6278a1bea41d4d"} |
> | D2_specification_and_erratum_match_record_pointers | PASS | {"specification": "c000e80c3c5beea0a446d8caa4eacd144124c485336478b51ab35be9b058cd82", "erratum": "d78feae7b97ccce3c6dd2ed050e08da4956fe0889ba3ce19c3fcbe678dfbb393"} |
> | D2_correction_file_frozen | PASS | {"value": "782eaef3e95c9dbe4b79868d936fd1e96ff1aa681d93ad609b5463ddd119db47"} |
> | D2_spec_committed_before_fit | PASS | {} |
> | T1_design_pinned_by_addendum | PASS | {} |
> | validation_design_rechecked (Phase-5 entry-check code, re-run) | PASS | {"failed": []} |
> | T1_partition_and_separation | PASS | {"n": 699, "split": {"train": 525, "tuning": 174}, "n_double": 140, "min_train_to_tuning_m": 2010.0, "min_T1_to_validation_block_m": 2010.0, "min_T1_to_gold_m": 2191.8485349129396} |
> | phase6_kits_present_and_unchanged | PASS | {"kits": {"gold_A": ["phase6_gold_kit_INTERPRETER_A.zip", 641], "gold_B": ["phase6_gold_kit_INTERPRETER_B.zip", 160], "T1v2_A": ["phase6_T1v2_kit_INTERPRETER_A.zip", 699], "T1v2_B": ["phase6_T1v2_kit_INTERPRETER_B.z
> | expected_phase6_code_present | PASS | {} |
> | phase6_docs_present | PASS | {} |
> | prerun_checklists_all_DO_NOT_RUN | PASS | {} |
> | tests_pass | PASS | {"result": "137 passed, 10 warnings in 28.69s", "collected": 137} |
> 
> ## Frozen hashes (SHA-256)
> 
> | item | SHA-256 |
> |---|---|
> | gold_design | `acc8692a3203944d7d791802b00c799dba75d82768db68d71a4c642608b48dd6` |
> | gold_key | `42aaf33cfcdd9f836d606c3d0d9f27379222b15d05d09780a49ce6616f65b95e` |
> | validation_blocks_raster | `66ceb88e422105899d81ebbe5cfa378b329577fb9d64a4576540371f607844bf` |
> | training_eligible_raster | `b0f929001919c76075ca5719d2a670832f651b3a3a246a9468cfe130b88baa2d` |
> | strata_raster | `ccd9f3177f998081020b8319a7a2931e02ac3c2136c35e22246f48d6c369cdd8` |
> | T1_design | `4058193bc6da90d81c42604a4834c0ef049d3f61c33b91236adb955e1d7d953e` |

## Part B — restoration audit (2026-10-03T00:50:34Z)

The repository was restored from `/mnt/user-data/outputs/phase6/phase6_repository_snapshot.zip` (git 7b85b69) + the evaluator keys, and the Phase-7 code was re-applied from the session transcript (`docs/phase7_execution_log.md`).

### RESULT: **OK - frozen state restored where verifiable**

| check | result | detail |
|---|---|---|
| protocol_v2_code_hash | PASS | {"value": "3bb40f9c982d601db88f75913f7d00acb4944136a3f9771079b9413ec31a14ab"} |
| protocol_v2_yaml_hash | PASS | {"value": "fe1fad6f8b8eda1618302d6886ab8d90fbb65aefdf6da17f961aee93144cbdbe"} |
| registry_G8_check_confirmatory_records | PASS | {"n": 6} |
| confirmatory_records_all_PENDING | PASS | {} |
| phase6_manifest_files_unchanged (restored snapshot) | PASS | {"n": 46, "differences": {}} |
| registry_records_unchanged | PASS | {"n": 21, "differences": {}} |
| frozen_hashes_recorded_in_the_original_phase7_audit | PASS | {"mismatches": []} |
| phase6_kits_unchanged (outputs/phase6/kits) | PASS | {} |
| spatial_context_restored_pixel_exact | PASS | {"files": {"data/products_ext/district/district_mask_30m.tif": null, "data/labels/gold/phase4/sampling_design/validation_blocks_30m.tif": true, "data/labels/gold/phase4/sampling_design/training_eligible_30m.tif": false}} |
| window_silver_restored_with_original_counts | PASS | {} |
| gold_design_rechecked | PASS | {"n": 641, "blocks": 125, "min_gold_to_training_eligible_m": 2010.0} |
| T1_partition_and_separation | PASS | {"n": 699, "split": {"train": 525, "tuning": 174}, "n_double": 140, "min_train_to_tuning_m": 2010.0, "min_T1_to_validation_block_m": 2010.0, "min_T1_to_gold_m": 2191.8485349129396} |
| no_human_label_files (as at the original entry) | PASS | {"files": {"data/labels/gold/phase4/raw_interpreter_A": 0, "data/labels/gold/phase4/raw_interpreter_B": 0, "data/labels/training/phase6_T1v2/raw_interpreter_A": 0}} |
| tests_pass | PASS | {"result": "146 passed, 5 skipped, 11 warnings in 101.78s (0:01:41)", "note": "the original 151 = 146 passed + 5 skipped here (skips need gitignored data: Landsat manifest parquet, district products, window fraction files)"} |

### Not re-verifiable after the restoration

* **git history of Phases 1-6**: not part of the Phase-6 snapshot (snapshot of git 7b85b69)
* **strata_30m.tif**: needs every district product (not restored)
* **Phase-5 entry-check items on products/cube pilot**: gitignored products not restored (pilot composites, district products, Landsat manifest parquet)
* **byte identity of training_eligible_30m.tif / district_mask_30m.tif**: pixel identity proven instead (COG bytes depend on the GDAL version)
