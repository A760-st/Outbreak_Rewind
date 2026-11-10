# OUTBREAK REWIND --- Website & System Architecture

**Product:** Evidence-Centred Outbreak Investigation and Reconstruction
Workspace\
**Document type:** Proposed architecture baseline\
**Prepared:** 10 November 2026\
**Status:** Architecture proposal for synthetic-data MVP

## 1. Architecture goals

Build a web workspace that preserves the source, chronology,
uncertainty, contradictions, and revision history of an investigation.

The core architecture concept is **Evidence Rewind**: - ingest a new or
corrected record; - determine which derived views and working hypotheses
may be affected; - show a proposed change set; - require a human
investigator to review and approve changes; - preserve the previous
interpretation and audit trail.

## 2. Product boundary and safety

The MVP is a training/research demonstrator using synthetic data. It is
not a production public-health surveillance system, clinical
decision-support tool, diagnostic system, or outbreak prediction engine.

No real patient data should be used unless a formal institutional,
legal, privacy, and security review authorizes it.

## 3. Website information architecture

  ------------------------------------------------------------------------------------
  Area                    Route                                Purpose
  ----------------------- ------------------------------------ -----------------------
  Landing                 `/`                                  Product explanation and
                                                               demo

  Login                   `/login`                             Authentication

  Workspace               `/workspace`                         Investigation list

  Investigation overview  `/investigations/:id`                Summary and current
                                                               status

  Data import             `/investigations/:id/import`         Upload and field
                                                               mapping

  Cases                   `/investigations/:id/cases`          Case records and
                                                               classification

  Contacts                `/investigations/:id/contacts`       Contact records and
                                                               follow-up metadata

  Timeline                `/investigations/:id/timeline`       Chronological event
                                                               view

  Network                 `/investigations/:id/network`        Case/contact/exposure
                                                               relationships

  Evidence                `/investigations/:id/evidence`       Source records and
                                                               verification

  Contradictions          `/investigations/:id/review-flags`   Inconsistency and
                                                               missing-data review

  Hypotheses              `/investigations/:id/hypotheses`     Working hypotheses and
                                                               evidence

  Rewind                  `/investigations/:id/rewind`         Change-impact preview
                                                               and approval

  History                 `/investigations/:id/history`        Version and audit
                                                               timeline

  Reports                 `/investigations/:id/reports`        Export summary and
                                                               audit record

  Settings                `/settings`                          Members, roles, data
                                                               retention
  ------------------------------------------------------------------------------------

## 4. Primary user journey

``` mermaid
flowchart TD
  A[Create investigation] --> B[Import synthetic or approved data]
  B --> C[Map fields to canonical schema]
  C --> D[Validate and preserve raw records]
  D --> E[Build timeline and relationship views]
  E --> F[Flag contradictions and missing information]
  F --> G[Create working hypotheses]
  G --> H[Add corrected or newly verified evidence]
  H --> I[Calculate affected derived records]
  I --> J[Present Evidence Rewind change set]
  J --> K{Investigator review}
  K -- Reject / request changes --> H
  K -- Approve --> L[Create new investigation revision]
  L --> M[Preserve prior state and audit event]
  M --> N[Export updated investigation report]
```

## 5. Main page requirements

### 5.1 Investigation overview

Show investigation title, scenario, dataset version, team members/roles,
case count, evidence count, unresolved flags, active hypotheses, latest
rewind event, and last updated time.

### 5.2 Data import

-   CSV upload.
-   Field mapping to canonical entities.
-   Required-field and type validation.
-   Duplicate detection based on stable source IDs and configurable
    matching rules.
-   Preview rejected rows and warnings.
-   Preserve original source file and row references.
-   Record who imported the data and when.

### 5.3 Canonical case record

Suggested fields: - internal case ID; - source record ID; - case
classification (suspected/probable/confirmed/other, configurable); -
symptom onset time and precision; - report/record time; - location at
appropriate coarse granularity; - relevant exposure events; - laboratory
result references; - verification state; - source and notes.

Avoid collecting direct identifiers in the MVP.

### 5.4 Timeline

-   Display events by event time.
-   Make record/ingestion time available separately.
-   Show uncertainty or unknown time precision.
-   Filter by case, event type, source, and verification state.
-   Selecting an event opens source provenance.
-   Corrections create a new revision rather than erasing history.

### 5.5 Relationship network

-   Nodes: cases, contacts, exposure events, locations, and evidence
    records.
-   Edges: reported contact, shared exposure, temporal proximity, or
    investigator-confirmed relationship.
-   Each edge must include evidence/source, time interval, status, and
    uncertainty.
-   Do not label an edge "transmission" unless an authorized
    investigator explicitly records that interpretation with supporting
    evidence.
-   Graph layout is explanatory, not proof of causality.

### 5.6 Contradiction workbench

Flag examples: - conflicting symptom-onset dates for the same case; -
impossible or overlapping event chronology under configured rules; -
conflicting location or exposure descriptions; - lab result date
preceding specimen collection; - missing required fields; - records with
unclear source or verification state.

Each flag contains the compared records, rule triggered, severity, and
review decision. The system must not automatically decide which source
is correct.

### 5.7 Hypothesis workspace

Each hypothesis contains: - statement; - proposed
source/exposure/transmission explanation; - supporting evidence links; -
opposing or inconsistent evidence; - assumptions; - expected
observations; - current status; - author and revision history.

Use labels such as Working, Needs review, Weakened, Not supported by
current evidence, and Inconclusive. Avoid "proven."

### 5.8 Evidence Rewind

When evidence is added or corrected: 1. identify entities and events
directly affected; 2. identify timeline events and graph edges derived
from them; 3. identify hypotheses linked to those records; 4. create a
proposed change set; 5. display before/after values and reason; 6.
require investigator approval; 7. write a new revision and immutable
audit event.

The first version should use deterministic dependency links and rules,
not an LLM deciding the impact.

## 6. System architecture

``` mermaid
flowchart TB
  U[Investigator / Reviewer] --> FE[React + TypeScript Web App]
  FE --> API[FastAPI Backend]
  API --> AUTH[Authentication + RBAC]
  API --> PG[(PostgreSQL)]
  API --> OBJ[(Object Storage)]
  API --> ING[Import & Validation Service]
  ING --> PG
  ING --> OBJ
  API --> DERIVE[Timeline / Relationship Derivation]
  DERIVE --> PG
  API --> RULES[Contradiction Rules Engine]
  RULES --> PG
  API --> REWIND[Evidence Rewind Impact Engine]
  REWIND --> PG
  API --> AI[Optional AI Summary Adapter]
  AI --> LLM[External LLM Provider]
  API --> AUDIT[Audit Event Service]
  AUDIT --> PG
```

### Component responsibilities

-   **Frontend:** investigation views, import mapping, timeline, network
    graph, review flags, hypothesis editor, rewind approval.
-   **FastAPI:** validation, access control, business logic, APIs.
-   **PostgreSQL:** normalized investigation records, temporal fields,
    relationships, hypotheses, revisions, flags, audit events.
-   **Object storage:** original imports, immutable source files, report
    exports.
-   **Import service:** parse, validate, map, and preserve source
    references.
-   **Derivation service:** create timeline events and graph edges from
    approved source records.
-   **Rules engine:** deterministic contradiction and missing-data
    checks.
-   **Rewind engine:** calculate affected dependent records and prepare
    a proposed change set.
-   **Optional AI adapter:** summarize selected evidence with source
    links; never invent facts or auto-approve revisions.
-   **Audit service:** preserve who did what, when, and why.

## 7. Suggested technology stack

  -----------------------------------------------------------------------
  Layer                   MVP choice              Notes
  ----------------------- ----------------------- -----------------------
  Frontend                React + TypeScript +    Web application
                          Vite                    

  UI                      Tailwind CSS or CSS     Choose one
                          modules                 

  API                     Python + FastAPI        Typed API

  Database                PostgreSQL              Relational data and
                                                  revisions

  ORM/migrations          SQLAlchemy + Alembic    Schema management

  Validation              Pydantic                API and import schemas

  Data processing         pandas                  CSV import and
                                                  normalization

  Graph UI                Cytoscape.js or React   Choose based on graph
                          Flow                    needs

  Timeline UI             React timeline          Prioritize clarity
                          component or custom     
                          list                    

  Graph analysis          NetworkX                Optional; do not infer
                                                  transmission

  AI                      Optional provider       Add only after
                          adapter                 deterministic workflow

  Packaging               Docker                  Repeatable local setup

  Testing                 pytest + frontend tests Rule and workflow
                                                  validation
  -----------------------------------------------------------------------

A graph database is not required for the first MVP. PostgreSQL edge
tables are sufficient for a small synthetic dataset.

## 8. Core database model

``` text
User
- id, email, identity_provider_id, created_at

Investigation
- id, title, scenario_description, status, created_by, created_at

InvestigationMember
- investigation_id, user_id, role

SourceFile
- id, investigation_id, filename, object_key, checksum, imported_by, imported_at

SourceRecord
- id, source_file_id, source_row_id, raw_payload_json, source_system, record_time

Case
- id, investigation_id, source_record_id, case_code, classification, verification_status

Contact
- id, investigation_id, source_record_id, contact_code, verification_status

LabResult
- id, case_id, specimen_time, result_time, result_type, result_value, source_record_id

ExposureEvent
- id, investigation_id, event_type, location_id, start_time, end_time, time_precision, source_record_id

Location
- id, investigation_id, label, admin_area, latitude_optional, longitude_optional

Relationship
- id, investigation_id, from_entity_type, from_entity_id, to_entity_type, to_entity_id,
  relationship_type, evidence_id, status, confidence_note

TimelineEvent
- id, investigation_id, entity_type, entity_id, event_type, event_time,
  time_precision, recorded_at, source_record_id, revision_id

EvidenceRecord
- id, investigation_id, source_record_id, evidence_type, verification_status, notes

Hypothesis
- id, investigation_id, statement, rationale, status, created_by

HypothesisEvidenceLink
- hypothesis_id, evidence_record_id, relation_type [supports / contradicts / contextualizes]

ContradictionFlag
- id, investigation_id, rule_id, record_a_id, record_b_id, details_json, status, reviewed_by

RewindEvent
- id, investigation_id, triggering_record_id, created_at, created_by, status

RewindImpact
- id, rewind_event_id, impacted_entity_type, impacted_entity_id, before_json, proposed_after_json, reason

ReviewDecision
- id, rewind_impact_id, decision, reviewer_id, comment, decided_at

InvestigationRevision
- id, investigation_id, revision_number, summary, created_by, created_at

AuditEvent
- id, investigation_id, actor_id, event_type, entity_type, entity_id, payload_json, created_at
```

### Temporal modelling rule

For every important event, distinguish: - **event time:** when the event
is believed to have happened; - **record time:** when the system
received or recorded the information; - **verification time/status:**
when and how it was reviewed.

Do not overwrite old values. Store revisions or source-record versions.

## 9. API design (initial)

  -----------------------------------------------------------------------------------------
  Method                  Endpoint                                  Purpose
  ----------------------- ----------------------------------------- -----------------------
  POST                    `/api/v1/investigations`                  Create investigation

  GET                     `/api/v1/investigations/:id`              Overview

  POST                    `/api/v1/investigations/:id/imports`      Upload source file

  POST                    `/api/v1/imports/:id/mapping`             Save field mapping

  POST                    `/api/v1/imports/:id/validate`            Validate import

  GET                     `/api/v1/investigations/:id/cases`        List cases

  GET                     `/api/v1/investigations/:id/timeline`     Timeline data

  GET                     `/api/v1/investigations/:id/network`      Relationship graph data

  GET                     `/api/v1/investigations/:id/flags`        Contradiction flags

  POST                    `/api/v1/investigations/:id/hypotheses`   Create hypothesis

  POST                    `/api/v1/investigations/:id/evidence`     Add evidence

  POST                    `/api/v1/investigations/:id/rewind`       Calculate impact
                                                                    preview

  GET                     `/api/v1/rewind/:id`                      View proposed changes

  POST                    `/api/v1/rewind/:id/decisions`            Approve/reject impacts

  GET                     `/api/v1/investigations/:id/history`      Revision/audit history

  POST                    `/api/v1/investigations/:id/reports`      Export report
  -----------------------------------------------------------------------------------------

Every route must check investigation membership and role. Use request
IDs, pagination, structured errors, and audit sensitive actions.

## 10. Evidence Rewind algorithm --- MVP

``` text
INPUT: corrected_or_new_source_record

1. Validate the incoming record and preserve its original payload.
2. Create a new source-record version; do not delete the prior version.
3. Identify directly affected case/contact/lab/exposure entities.
4. Find timeline events and relationship edges derived from those entities.
5. Find hypotheses linked to the affected evidence or derived records.
6. Compare previous and proposed derived values.
7. Create a RewindEvent and RewindImpact records.
8. Present the impact preview to an investigator.
9. For each impact, record approve / reject / needs-more-information.
10. Apply only approved changes as a new investigation revision.
11. Preserve previous state and write audit events.
```

This is dependency-based change impact, not an AI-generated
epidemiological conclusion.

## 11. Security, privacy, and governance

-   Use synthetic data for development and demonstrations.
-   Do not store names, phone numbers, addresses, or direct identifiers
    in the MVP.
-   Use role-based access and least privilege.
-   Encrypt network traffic and protect secrets.
-   Validate uploaded files and enforce file-size limits.
-   Keep original imports and audit records access-controlled.
-   Define deletion and retention policies.
-   Log exports and permission changes.
-   Do not send health data to an external LLM.
-   AI summaries, if added, must use approved de-identified content and
    cite source records.
-   Clearly label synthetic/demo data and non-operational status.
-   Obtain institutional and domain-expert review before any real-world
    deployment.

## 12. Deployment topology

### Local development

-   React frontend.
-   FastAPI backend.
-   PostgreSQL.
-   Local object storage adapter.
-   Optional Redis only if asynchronous import/report jobs are needed.

### Demo deployment

One frontend, one API service, managed PostgreSQL, and private object
storage. Keep the demo isolated from real health systems and real
patient data.

## 13. Implementation phases

1.  **Foundation:** auth, investigation, member roles, schema.
2.  **Import:** source file storage, field mapping, validation, raw
    record preservation.
3.  **Investigation views:** cases, timeline, relationship network.
4.  **Evidence quality:** verification states, contradiction rules,
    review workbench.
5.  **Hypotheses:** evidence links, status, revisions.
6.  **Evidence Rewind:** dependency graph, impact preview, approval,
    audit.
7.  **Reporting:** investigation summary and revision history.
8.  **Hardening:** privacy checks, permissions, tests, demo rehearsal.

## 14. Architecture acceptance criteria

-   Every displayed fact can be traced to a source record.
-   Event time and record time are distinct.
-   Conflicting records are flagged, not silently reconciled.
-   Relationship edges carry evidence and interpretation labels.
-   New evidence creates a reviewable impact preview.
-   No derived interpretation changes without human approval.
-   Previous states remain inspectable.
-   The system can demonstrate the complete synthetic scenario
    end-to-end.

## 15. Open architecture decisions

-   Final synthetic scenario and canonical data schema.
-   Whether coordinates are needed or administrative areas suffice.
-   Exact contradiction rules and severity levels.
-   Graph visualization library.
-   Whether optional AI summarization is needed for the first demo.
-   Hosting, data retention, and access model.
-   Team ownership for import, timeline, graph, evidence, rewind, and
    testing.

------------------------------------------------------------------------

This is a proposed architecture for a synthetic-data MVP, not a
production public-health system or security certification.
