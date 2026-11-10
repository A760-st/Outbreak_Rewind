# Outbreak Rewind

**An evidence-centred workspace for outbreak investigation and reconstruction.**

Outbreak Rewind is a research-informed proposal for helping investigation teams assemble a time-ordered account from case, contact, laboratory, interview, and exposure records. It focuses on preserving where information came from, what is uncertain or contradictory, and how an investigation's working interpretation changes as evidence is added or corrected.

## Project Status

This repository currently contains product research and a proposed system architecture for a synthetic-data MVP. It is not a running application. The technology choices below are suggestions, not implemented dependencies.

## The Evidence Rewind Concept

When a record is added or corrected, Evidence Rewind is intended to identify affected timelines, relationships, and working hypotheses. It presents a proposed change set for an investigator to review, while preserving the previous interpretation and its audit history. Changes to interpretations require human review; the system is not intended to infer causation or make public-health decisions.

## Proposed MVP

- Import and validate synthetic case, contact, laboratory, and exposure records.
- Preserve source references, verification state, event time, and record time.
- Explore records through timelines and evidence-linked relationship views.
- Flag contradictions and missing information for review.
- Record hypotheses with supporting evidence and uncertainty.
- Preview the impact of corrected or newly verified evidence, then approve or reject revisions.
- Retain prior states and an audit trail.

## Proposed Technology

The architecture document suggests React and TypeScript for the web app, FastAPI for the API, and PostgreSQL for relational data. Python data tools and Docker are also proposed. These choices remain subject to implementation and validation.

## Documents

- [Product ideation and research](OUTBREAK_REWIND_IDEATION_AND_RESEARCH.md)
- [Website and system architecture](OUTBREAK_REWIND_WEBSITE_ARCHITECTURE.md)

## Safety and Data Scope

The MVP is intended as a training and research demonstrator using synthetic data. Do not use real patient data unless formal institutional, legal, privacy, and security reviews authorize it. Outbreak Rewind is not a production surveillance system, diagnostic or clinical decision-support tool, outbreak prediction engine, or substitute for public-health expertise.

Any real-world deployment would require domain-expert review and appropriate privacy, security, access-control, and governance safeguards.
