# Outbreak Rewind

**An evidence-centred workspace for outbreak investigation and reconstruction.**

Outbreak Rewind is a proposed workspace for exploring outbreak and disease-surveillance data while preserving source provenance, uncertainty, data-quality limitations, and the history of an investigation's working interpretations.

## Project Status

This repository contains a revised proposed architecture and team work-allocation documents. It is not a running application; architecture and technology choices are proposals, not implemented dependencies.

## Product Concept

**Evidence Rewind** tracks how corrected or newly added source records affect derived views and working hypotheses. It is intended to present proposed changes for human review and preserve prior revisions and an audit trail.

The revised architecture describes two investigation modes:

- **Aggregate surveillance:** Explore compatible disease, geography, time-series, and laboratory-surveillance data while making source, reporting grain, estimates, and missingness visible.
- **Outbreak/event investigation:** Review outbreak-level records, their provenance, supplied context and outcomes, and evidence-linked working hypotheses.

The initial dataset collection covers nine aggregate or outbreak-level datasets. These sources do not generally contain person-level histories or contact chains, so the product must not invent individual case, contact, or transmission records from aggregate totals.

## Project Documents

- [Revised website and system architecture](OUTBREAK_REWIND_REVISED_ARCHITECTURE.md) — proposed product boundaries, dataset inventory and cautions, investigation modes, information architecture, and technical design.
- [Adithya's work allocation](OUTBREAK_REWIND_Adithya_Work_Allocation.pdf)
- [Anjali Kumari's work allocation](OUTBREAK_REWIND_Anjali_Kumari_Work_Allocation.pdf)
- [Anjali Sagar's work allocation](OUTBREAK_REWIND_Anjali_Sagar_Work_Allocation.pdf)

## Safety and Data Scope

Use synthetic, public, or otherwise approved research data only. Do not use real patient data unless formal institutional, legal, privacy, and security reviews authorize it. Outbreak Rewind is not a diagnostic or clinical decision-support tool, automated outbreak-prediction engine, operational public-health surveillance service, or substitute for public-health expertise.

Any real-world deployment would require domain-expert review and appropriate privacy, security, access-control, and governance safeguards.
