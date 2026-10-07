# Phase 6 — human gold workflow (Tier A), end to end

Human gold is the binding dependency of every confirmatory record. **Status: 0 labels** (`raw_interpreter_A/`, `raw_interpreter_B/`, `adjudicated/` and `metadata/` are all empty).

* No substitute may enter Tier A: no AI, no Dynamic World, GLC_FCS30D, WorldCover, Esri, GHSL or WSF, and no Phase-3 provisional labels. Those remain references (Tier B) or silver labels (Tier C).
* This workflow uses only the existing, tested code: `src/validation/ingest_gold.py`, `src/validation/audit_gold.py` and `tests/test_gold_ingestion.py` (28 tests).

## 0. Design (unchanged; evaluator only)

641 points in 125 independent 6 km validation blocks, 13 strata, outside the Phase-3 windows + 2 km. 160 double-interpretation points (25 %). Key: `data/labels/gold/phase4/_key/phase4_gold_key.csv`. **Never** give the key, the repository or any design file to an interpreter.

## 1. What Interpreter A receives

`phase6_gold_kit_INTERPRETER_A.zip`, containing:

* the form (641 rows);
* a KML of those 641 cells;
* `INTERPRETER_PROTOCOL.md` (= `docs/phase6_interpreter_protocol.md`);
* a metadata template.

## 2. What Interpreter B receives

`phase6_gold_kit_INTERPRETER_B.zip`: the B form (the 160 double points, re-shuffled), a KML of **only those 160 cells**, the same protocol and a metadata template. Interpreter B must be a different person from A, working independently.

## 3. What they must NOT receive

* the key;
* strata, blocks or rainfall regions;
* the sampling design or the full gold protocol (`docs/phase4_gold_protocol.md` §1 names strata and products);
* any product layer or product name tied to a point;
* model predictions, confidences or maps;
* the other interpreter's form;
* the repository.

**The Phase-4/5 kits are superseded.** They shipped the full gold protocol, and the B kit carried a KML of all 641 points. If they were already handed out, record this in `metadata/kit_history.json`; the design information they contained was general, not point-specific.

## 4. Interpretation instructions and 5. Class definitions

See `docs/phase6_interpreter_protocol.md` §2-§5:

* the unit is the 30 m cell;
* epochs are done in the order 2020, 2000, 2010, 1990;
* the classes are built_up, agriculture, natural_vegetation, water, bare_sparse, other, uncertain and ambiguous, with the decision rules given there;
* each label also records the dominant share, second class, confidence, imagery source, imagery date, status and notes.

## 6. Historical imagery rules

* Google Earth Pro historical imagery, with an image date in calendar years epoch−2 … epoch+2.
* Use the closest date; when dates tie, prefer the dry season.
* Record the actual image date and source.
* A `Landsat_context` label (coarse imagery only) may carry at most confidence 2. Ingestion enforces this (`LANDSAT_CONFIDENCE`).

## 7. Unavailable-data rules

* No usable image within ±2 years, or the cell is obscured in every image: status `unavailable`, class empty.
* `unavailable` counts as **labelled** for G2. Guessing is a protocol violation.
* `uncertain` / `ambiguous` are recorded and reported, but do **not** count as labelled (G2 revision 2).

## 8. Double-interpretation procedure

1. A labels all 641 points; B labels the 160 points independently, at the same time or later, without contact.
2. Agreement is computed per epoch on the double points: Cohen's κ over points where both gave a definite class (needs n ≥ 30), plus κ and Krippendorff's α with abstentions (`audit_gold.py`).

## 9. Correction procedure

* Raw files are never edited after hand-in.
* If an interpreter corrects a form, they send a correction statement (file, cells/years, reason, date, pseudonym) together with the corrected form.
* The evaluator records `data/labels/gold/phase4/metadata/correction_<file>.json`:

  ```json
  {"file": "raw_interpreter_A/<name>.csv", "previous_sha256": "<old>", "new_sha256": "<new>", "interpreter_id": "<pseudonym>", "reason": "<text>", "date": "YYYY-MM-DD", "signed_by_interpreter": true}
  ```

* The file is then frozen again under a new pass id. Ingestion refuses any path frozen with different contents without such a record (`RAW_REFROZEN`), and any file that is not equal to its latest freeze (`HASH_MISMATCH`, `STALE_RAW`).

## 10. Freeze procedure

1. Copy the returned CSV and `.meta.json` unchanged into `raw_interpreter_A/` or `raw_interpreter_B/`.
2. When a pass is complete:

   ```bash
   python3 src/validation/ingest_gold.py freeze <pass_id> --by <evaluator>
   ```

   This writes `metadata/freeze_<pass_id>.json` with the SHA-256 of every raw, meta and adjudication file. A freeze is immutable.

## 11. Ingestion procedure

```bash
python3 src/validation/ingest_gold.py ingest      # exit 1 + list of every violation, or writes data/labels/gold/phase4/ingested/
```

Ingestion checks:

* provenance: frozen, human, blind declaration, no declared product or AI source;
* identity: every ID present, no duplicates, correct A/B subsets;
* geometry and leakage: coordinates equal to the design, cells in validation blocks, ≥ 2 km from training cells, outside the Phase-3 windows;
* values: vocabularies, date window, confidence rules;
* adjudication completeness.

Fix violations at the source with the correction procedure (§9), never by editing.

## 12. Adjudication procedure

* Every A/B disagreement where **at least one** interpreter gave a definite class needs one row in `adjudicated/<name>.csv`: `anon_id, epoch, label_A, label_B, adjudicated_label, rule, reviewer, date`.
* The reviewer is a third person, or the rule `joint_review` is used.
* The label_A / label_B quoted must equal the raw forms.
* Adjudication files are frozen like raw files.
* Where both interpreters abstained differently, the reference becomes `uncertain`: an abstention, never a class.

## 13. G2 pass criteria (protocol v2 text; operational definitions fixed before any gold)

| criterion | 2020 **and** 2000 |
|---|---|
| labelled | ≥ 80 % of the 641 points have a resolved definite class or `unavailable` |
| double interpreted | ≥ 20 % of the 641 points have records from both A and B |
| agreement | Cohen's κ ≥ 0.6 over ≥ 30 double points where both gave a definite class |
| adjudication logged | no `unadjudicated` point-epoch |

Command: `python3 src/validation/audit_gold.py` → `results/phase5/gold/gold_audit.json`. G2 passes only if every criterion holds for both epochs. The confirmatory checklist reads this file and is never overridden.

## 14. Order of work and effort (planning estimate, not a result)

| pass | interpretations | effort at 1-2 min each |
|---|---|---|
| 2020 + 2000: A (641 × 2) + B (160 × 2) | 1 602 | 27-53 h |
| 2010 + 1990, later | 1 602 | 27-53 h |
