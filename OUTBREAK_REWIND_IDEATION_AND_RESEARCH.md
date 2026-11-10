# OUTBREAK REWIND --- Product Ideation & Research

**Working product definition:** Evidence-Centred Outbreak Investigation
and Reconstruction Workspace\
**Prepared:** 10 November 2026\
**Purpose:** Axiom Review 1 preparation\
**Status:** Research-informed proposal; validate with public-health
domain experts

## 1. Executive recommendation

Do not position Outbreak Rewind as an outbreak prediction engine,
diagnostic system, or replacement for public-health surveillance
platforms.

Position it as a **case-investigation evidence reconstruction
workspace** for authorized investigation teams. It helps users assemble
a time-ordered account from case, contact, laboratory, interview, and
location records; inspect conflicting or incomplete information; compare
investigation hypotheses; and see how new verified evidence changes the
current reconstruction.

The central concept is **Evidence Rewind**: when a new record is added
or corrected, the system identifies which earlier timeline events,
links, and hypotheses may need review, preserves the previous
interpretation, and explains what changed. A human investigator decides
whether to accept the revision.

## 2. Problem definition

### 2.1 Problem statement

Outbreak investigations combine information from multiple sources and
people. Case definitions can change as understanding improves; records
may arrive at different times; dates, locations, and exposure histories
can be incomplete or inconsistent. Investigators must analyze cases by
person, place, and time, develop hypotheses about source and
transmission, and test those hypotheses using additional epidemiological
or laboratory evidence.

The operational challenge is not simply displaying a timeline or contact
graph. It is preserving the **provenance, chronology, uncertainty,
contradictions, and revision history** of the investigation as evidence
changes.

### 2.2 Intended users

-   Public-health investigation teams and field epidemiologists.
-   Epidemiology students and training teams using synthetic or
    de-identified data.
-   Data managers supporting case investigation and contact tracing.
-   Supervisors reviewing how an investigation's working interpretation
    changed.

For the student MVP, use synthetic or properly de-identified data only.
Do not collect real patient records.

### 2.3 Why it matters

WHO's outbreak investigation workflow includes confirming an event,
defining cases, collecting standardized case and contact data,
descriptive analysis by time/place/person, hypothesis development, and
further investigation to evaluate hypotheses. WHO also says changes to
case definitions should be evidence-based, communicated, and logged
\[R1\]. CDC guidance describes iterative hypothesis generation and
testing and cautions that statistical association alone is not
sufficient to establish causation \[R2\].

A tool that records how evidence changed the working account may support
transparency and review. It must not make public-health decisions
autonomously.

## 3. Research landscape and existing alternatives

### 3.1 WHO Go.Data

WHO describes Go.Data as a field data collection tool focused on case
and contact data, contact follow-up, case-case and case-contact
relationships, and chains of transmission. It supports multiple users,
role permissions, configurable questionnaires, laboratory data,
imports/exports, and mobile use \[R3\]. WHO also describes it as
supporting case investigation and visualization of disease transmission
\[R4\].

**Implication:** Outbreak Rewind cannot claim that case/contact
management, contact tracing, transmission-chain visualization, or
multi-user collaboration are new.

### 3.2 WHO Outbreak Toolkit

WHO's Outbreak Toolkit provides standardized tools and guidance for
field investigators. Its workflow includes case definitions, line lists,
epidemic curves, maps, hypothesis development, and further investigation
\[R1, R5\]. WHO's generic case investigation form is designed to support
collection of core variables and describe outbreaks by time, place, and
person \[R6\].

**Implication:** Outbreak Rewind should complement established
investigation practice rather than invent a replacement workflow.

### 3.3 CDC Field Epidemiology Manual

CDC guidance describes organizing line-list data by time, place, and
person; developing hypotheses from descriptive findings, interviews,
expert knowledge, and outlier cases; and evaluating hypotheses using
appropriate epidemiological methods. It notes that the process may
involve multiple iterations and that statistical significance alone does
not establish a valid causal conclusion \[R2\].

**Implication:** The product must represent uncertainty and
investigative judgment, not turn graph connections or statistical
associations into causal claims.

### 3.4 Research opportunity to validate

The proposed opportunity is a focused **evidence provenance and revision
layer** across the investigation record: - retain source and
confidence/verification state for each fact; - distinguish event time
from record-entry time; - detect conflicting values without silently
choosing one; - show which derived timelines, links, and hypotheses
depend on a changed record; - preserve previous and current
interpretations with reasons; - require investigator review before an
updated interpretation is accepted.

This is a product hypothesis. The current research reviewed here does
not prove that existing systems lack these capabilities. A hands-on
Go.Data feature review and interviews with epidemiology users are
required before claiming a gap.

## 4. Proposed product

### 4.1 Product proposition

Outbreak Rewind is an investigation workspace that turns a set of case
and evidence records into a traceable, time-aware investigation view. It
helps authorized users understand: - what is currently known; - where
each fact came from; - which records disagree or remain uncertain; - how
cases and possible exposures relate; - which hypotheses are currently
being considered; and - how new evidence changes the investigation
record.

### 4.2 Proposed differentiators to validate

1.  **Evidence provenance:** every fact has source, record ID,
    collection time, event time, author/role, and verification status.
2.  **Bitemporal records:** preserve both when an event reportedly
    occurred and when the system learned/recorded it.
3.  **Contradiction workbench:** flag inconsistent dates, locations,
    exposure details, and case attributes for review; never auto-resolve
    them.
4.  **Evidence Rewind:** when a record is corrected or verified,
    identify dependent timeline entries, graph edges, and hypotheses for
    reassessment.
5.  **Versioned investigation narrative:** preserve prior interpretation
    and the reason for each revision.
6.  **Human decision controls:** investigators approve case
    classification, relationship interpretation, and hypothesis status
    changes.

## 5. Core user journey

1.  Create an investigation using a synthetic or approved de-identified
    dataset.
2.  Import case, contact, laboratory, interview, and exposure records.
3.  Map incoming fields to a canonical schema and validate them.
4.  Review source, timestamps, and verification status.
5.  Generate a timeline and case/contact relationship graph.
6.  Inspect flagged contradictions and missing information.
7.  Create competing working hypotheses with supporting and opposing
    evidence.
8.  Add a new or corrected laboratory/interview record.
9.  Run Evidence Rewind to identify affected derived records and
    hypotheses.
10. Review the change set and approve or reject each proposed update.
11. Export an investigation summary and audit trail.

## 6. MVP definition

### Required for first demonstrator

-   Secure user login and role-based access for a small demo team.
-   Create investigation and import CSV records.
-   Canonical case/contact/lab/exposure schema.
-   Source and verification metadata.
-   Event-time and record-time fields.
-   Timeline view.
-   Case/contact relationship graph with edge provenance.
-   Rule-based contradiction and missing-data flags.
-   Hypothesis records with evidence links.
-   Evidence Rewind change-impact preview.
-   Human approval and revision history.
-   Synthetic data and a scripted demo scenario.

### Defer

-   Live feeds from hospitals or government surveillance systems.
-   Real patient data.
-   Automated diagnosis or pathogen identification.
-   Predictive outbreak forecasting.
-   Automated public-health recommendations.
-   Mobile field collection.
-   GIS-grade spatial analytics.
-   Claims of causal inference from graph structure.
-   Interoperability with production health systems.

## 7. Demonstration scenario

Use a fully synthetic outbreak scenario with fictional case IDs and
locations.

1.  Import 20--30 synthetic cases, contacts, and lab records.
2.  Display cases by symptom-onset time and location.
3.  Show a possible contact/exposure network with source references.
4.  Flag a record where an interview date conflicts with a laboratory
    timeline.
5.  Create two working hypotheses, clearly marked as unverified.
6.  Add a newly verified laboratory result that changes the
    interpretation for one case.
7.  Run Evidence Rewind.
8.  Show affected timeline items, graph relationships, and hypotheses.
9.  Have the investigator approve a revision and inspect the audit
    trail.

The demo must explicitly say that the synthetic data and hypotheses are
illustrative and not epidemiological conclusions.

## 8. Feasibility and proposed technical approach

Suggested stack: - Frontend: React + TypeScript. - Backend/API: Python +
FastAPI. - Database: PostgreSQL. - Graph representation: relational edge
tables initially; NetworkX for analysis. A graph database is not
required for the MVP. - Timeline: event records queried by event time,
with record-time history. - Geospatial: basic coordinates or
administrative-area labels; defer advanced GIS. - Data processing:
pandas and Pydantic validation. - AI/NLP: optional
extraction/summarization from unstructured text only after provenance
and human review are implemented. - Deployment: Docker-based services.

Start with deterministic rules for contradictions and change-impact
analysis. AI-generated summaries can be added later, but the product's
core evidence logic should not depend on an LLM.

## 9. Core data model

-   User
-   Investigation
-   InvestigationMember
-   Case
-   Contact
-   CaseContactLink
-   EvidenceRecord
-   LabResult
-   ExposureEvent
-   TimelineEvent
-   Hypothesis
-   HypothesisEvidenceLink
-   ContradictionFlag
-   RewindEvent
-   RewindImpact
-   ReviewDecision
-   AuditEvent

Every source record should retain original source reference, ingestion
timestamp, event timestamp if known, verification status, and a checksum
or immutable raw-record reference.

## 10. Risks and mitigations

  --------------------------------------------------------------------------
  Risk                    Why it matters           Mitigation
  ----------------------- ------------------------ -------------------------
  Sensitive health data   Real patient data is     Synthetic/de-identified
  exposure                highly sensitive         data only; strict access
                                                   controls

  False causal            A graph edge may be      Label edges by evidence
  interpretation          mistaken for             type and uncertainty; no
                          transmission             causal claims by default

  Incorrect contradiction Valid records can differ Flag for human review;
  flags                   for legitimate reasons   never auto-correct

  Time ambiguity          Event time and reporting Store both timestamps,
                          time differ              timezone, precision, and
                                                   source

  Bad or incomplete       Derived views may        Show missingness,
  source records          mislead                  provenance, and
                                                   verification state

  Scope creep into        Production integration   Keep MVP offline and
  surveillance            is complex and regulated synthetic

  AI hallucinations       Could invent facts or    Deterministic core;
                          links                    source-linked AI
                                                   summaries; human approval

  Operational misuse      Product could be         Clear non-operational
                          mistaken for             demo boundary and expert
                          clinical/public-health   oversight
                          advice                   
  --------------------------------------------------------------------------

## 11. Evaluation plan

-   Correct import and validation of synthetic records.
-   Accuracy of deterministic contradiction rules against a
    hand-labelled test set.
-   Ability to trace each displayed fact and graph edge to its source.
-   Correct distinction between event time and record time.
-   Correct identification of dependent views after a record correction.
-   No automatic hypothesis or case-classification changes without
    approval.
-   User ability to explain why a timeline or hypothesis changed.
-   Audit trail completeness.

Do not evaluate the MVP by claiming outbreak prediction accuracy.

## 12. Review 1 presentation outline

1.  Problem, users, and investigation context.
2.  WHO/CDC investigation workflow.
3.  Existing tools: Go.Data and WHO Outbreak Toolkit.
4.  What existing systems already provide.
5.  Proposed evidence provenance and Evidence Rewind opportunity.
6.  Bounded synthetic-data MVP and demonstration scenario.
7.  Website/system architecture.
8.  Privacy, feasibility, risks, and human oversight.
9.  Team ownership, timeline, and references.

## 13. Open decisions before implementation

-   Which fictional outbreak scenario will be used for the demo?
-   Which minimum fields are needed for case, contact, lab, and exposure
    records?
-   What contradiction rules are defensible and testable?
-   How will the team represent uncertainty and source verification?
-   Which features are rules-based versus optional AI assistance?
-   Who owns data modelling, timeline/graph UI, evidence logic, testing,
    and presentation?
-   Which public-health expert or faculty member can review the
    workflow?

## References

-   \[R1\] WHO. "Stages of an outbreak investigation."
    https://www.who.int/emergencies/outbreak-toolkit/standardized-data-collection-tools/investigating-outbreak-of-unknown-disease
-   \[R2\] CDC. "Conducting a Field Investigation." *CDC Field
    Epidemiology Manual*.
    https://www.cdc.gov/field-epi-manual/php/chapters/field-investigation.html
-   \[R3\] WHO. "Go.Data Frequently Asked Questions."
    https://www.who.int/tools/godata/go.data-frequently-asked-questions
-   \[R4\] WHO. "About Go.Data." https://www.who.int/tools/godata/about
-   \[R5\] WHO. "Outbreak Toolkit."
    https://www.who.int/emergencies/outbreak-toolkit/
-   \[R6\] WHO. "Initial generic case investigation form (T0)."
    https://www.who.int/emergencies/outbreak-toolkit/standardized-data-collection-tools/t0-initial-case-investigation-form

------------------------------------------------------------------------

**Research caveat:** This is a focused initial review, not a systematic
review or a formal gap analysis. Go.Data and other existing systems
require hands-on evaluation before any claim that Evidence Rewind is
unique.
