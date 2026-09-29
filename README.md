# PRINT
A Python-based metadata audit and authority reconciliation pipeline that verifies existing labels and URIs for personal, organizational, and place names/subjects, while transforming plain-text entities and keywords into standardized, HTML-linked LCNAF, GeoNames, VIAF, and Wikidata authority records for datasets to be loaded into Digital Commons.

# Subject Authority Reconciliation & Keyword Resolution Pipeline

## About the Project
This repository contains a suite of Python/Jupyter Notebook scripts developed as an analytical, verification, and audit pipeline for library and archival metadata datasets. 

The dataset already contains existing labels and URIs. The pipeline performs dual-function authority management: it **audits and verifies existing name and subject linkages** while **reconciling and inserting newly matched authority records** to further enrich authority term lists. By scanning controlled subject fields (`subject_person_600`, `subject_geo_651`, `subject_corporate_610`) and unstructured terms in `keywords`, the system cross-references entries against multi-authority sources (LCNAF, GeoNames, VIAF, Wikidata) and row-level fallback links. Verified matches are formatted into standardized HTML anchor links, while unmatched or ambiguous entries are isolated into uncontrolled fields or audit logs for cataloger review.

## Key Features
* **Verification & Enrichment**: Audits existing labels and URIs in name and subject fields, resolves previously unlinked plain-text entities, and expands authority term lists.
* **Multi-Authority Integration**: Matches against LCNAF, VIAF, GeoNames, Wikidata authority files (term lists).
* **Automated Keyword Processing**: Extracts entities from uncontrolled `keywords` and maps newly verified matches to appropriate subject columns.
* **Fallback Linkage**: Performs row-level secondary lookups against existing linked fields for additional validation.
* **Audit Trail**: Generates comprehensive log sheets tracking source updates, assigned URIs, labels, and unmatched terms for quality control.
