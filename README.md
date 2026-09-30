# Multi-Authority Linked Data Reconciliation Pipeline
> Core Entity Verification, Link Correction, and Subject/Keyword Resolution

## Overview
This suite of Python scripts forms an analytical, verification, and audit pipeline to reconcile, replace, and enrich multi-authority linkages across historical datasets.

The pipeline operates in two primary phases:
1. **Core Entity Reconciliation & Correction:** Validates, normalizes, and inserts missing authority links (LCNAF, VIAF, Wikidata, GeoNames) for primary metadata attributes (`creator`, `receiver`, `sender_place`, `receiver_place`).
2. **Subject Authority & Keyword Resolution:** Parses unstructured terms from `keywords` and direct subject fields (`subject_person_600`, `subject_geo_651`, `subject_corporate_610`), maps them against authority lookups, and moves unmatched terms to uncontrolled fields or log sheets for cataloger review.

---

## 1. Core Entity Authority Scripts (Creator / Receiver / Place)
*Focused on `creator_linked_new`, `receiver_linked_new`, `sender_place_linked_new`, and `receiver_place_linked_new`.*

- **Wikidata Replacement (`replace_wikidata.ipynb`):** Normalizes QID URIs and updates labels.
- **LCNAF Replacement & Insertion (`replace_lcnaf.ipynb`):** Replaces existing LCNAF anchors and inserts missing links using name-matching with life-date stripping.
- **GeoNames Replacement & Insertion (`replace_geo.ipynb`):** Replaces place URIs (using canonical `sws.geonames.org`) and matches places using exact and fuzzy matching.
- **VIAF Replacement & Insertion (`replace_viaf.ipynb`):** Updates and inserts VIAF authority links for creators and receivers.

---

## 2. Subject Authority & Keyword Resolution Scripts
*Focused on `subject_person_600`, `subject_geo_651`, `subject_corporate_610`, and `keywords`.*

- **Subject & Organization Authority Updates (V1 / Module Scripts):**
  - `600_651_version1.ipynb`: Matches plain-text subject entries against LCNAF/GeoNames and fallback fields.
  - `organization.ipynb`: Reconciles organization subjects against Wikidata, LCNAF, and VIAF.
- **Integrated Keyword Pipeline (Main Updated Script - `600_610_651_from_keyword_updated.ipynb`):**
  - Parses unstructured terms from `keywords`.
  - Reconciles terms against multi-authority indices (LCNAF, VIAF, GeoNames, Wikidata, Corporate sources) and row-level fallbacks.
  - Automatically migrates verified entities to controlled MARC-aligned fields (`600`, `610`, `651`).
  - Retains unmatched terms in `keywords` and generates a detailed audit log (`keyword_authority_log`).

---

## Output Rules & Audit Reports
- **Controlled Fields:** Contain strictly validated HTML-formatted authority links.
- **Uncontrolled / Log Sheets:** Unmatched, ambiguous, or candidate terms are safely isolated into `sub_uncontrolled_*` or detailed log sheets (`*_ambig_log`, `keyword_authority_log`) for cataloger QA/review.
