# Subject Authority Reconciliation & Keyword Resolution Pipeline
A Python-based metadata audit and authority reconciliation pipeline that verifies existing labels and URIs for personal, organizational, and place names/subjects, while transforming plain-text entities and keywords into standardized, HTML-linked LCNAF, GeoNames, VIAF, and Wikidata authority records for datasets to be loaded into Digital Commons.

# Subject Authority Reconciliation & Keyword Resolution Pipeline

## About the Project
This repository contains a suite of Python/Jupyter Notebook scripts developed as an analytical, verification, and audit pipeline for library and archival metadata datasets. 

The pipeline performs a dual-function authority management workflow across MARC-aligned metadata fields (`subject_person_600`, `subject_geo_651`, and `subject_corporate_610`):
1. **Label & URI Audit/Verification (`replace*` / Verification Scripts):** Audits pre-existing labels and URIs in controlled fields, normalizes entity names (e.g., stripping life dates and bracketed text), corrects outdated/broken links, and updates records against authoritative lookup lists (LCNAF, GeoNames, VIAF, Wikidata).
2. **Unstructured Keyword Extraction & Resolution (`600_610_651_from_keyword_updated`):** Scans plain-text keywords, reconciles entities against multi-authority files and row-level relational links, transforms matched entries into standardized HTML anchor links within their corresponding subject columns, and removes successfully mapped terms from the keywords field.

---

## Pipeline Architecture & Script Versions

* **`600_651_version1` (Legacy Keyword Processor):** Initial iteration developed to extract personal names and geographic places from unstructured keywords and map them to LCNAF, VIAF, Wikidata, and GeoNames URIs.
* **`600_610_651_from_keyword_updated` (Current Keyword & Subject Pipeline):** Enhanced version expanded to include corporate/organization matching (`subject_corporate_610`). Accommodates updated metadata structures where pre-existing URIs were incorporated into keyword and subject fields. Implements a two-tiered lookup mechanism (Authority Master Lists → Row-Level Linked Metadata Fallback).
* **`replace*` / Verification Scripts:** Dedicated audit utility scripts that process existing linked columns, verify URIs against updated master sheets (`LCNAF_updated`, `GeoNames_updated`, `organization_updated`), update labels/URIs, and route ambiguous or unlinked entities to uncontrolled subject fields (`sub_uncontrolled_person`, `sub_uncontrolled_place`).

---

## Key Features

* **Multi-Authority Integration:** Reconciles entities across LCNAF, VIAF, GeoNames, and Wikidata authority sources, standardizing links into HTML `<p><a href="...">Label</a> [Authority]</p>` format.
* **Two-Tier Keyword Resolution Strategy:**
  * *Primary Match:* Direct normalized/alias cross-referencing against authority master files.
  * *Secondary Fallback:* Row-level context matching against record-specific metadata (e.g., `creator_linked_new`, `receiver_linked_new`, `sender_place_linked_new`, `receiver_place_linked_new`).
* **Controlled Field Migration & Keyword Cleaning:** Automatically strips successfully linked terms from the `keywords` column once appended to `subject_person_600`, `subject_geo_651`, or `subject_corporate_610`, leaving only unresolved terms for cataloger review.
* **Audit Logging & Uncontrolled Routing:** Generates a comprehensive execution log sheet (`keyword_authority_log`) tracking row-level source updates, authority selection, and leftover terms. Unmatched or low-confidence terms are isolated into uncontrolled fields for cataloger verification.
