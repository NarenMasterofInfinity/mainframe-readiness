# Product scope and roadmap

The workbench takes a local export of a mainframe application and produces an evidence-backed decision about starting detailed migration planning. It assesses the supplied application in place. The deliverable is an assessment report, dependency map and action list. OpenShift can be recorded as a target assumption. Migration execution, code conversion, production access and compatibility certification are outside this release.

## Roles and decisions

| Role | Decision or task | Evidence they need |
|---|---|---|
| Analyst | Define the application perimeter and run the assessment | Expected exports, inventory, parser coverage, unresolved inputs |
| Mainframe or domain SME | Confirm or challenge a claim | Original location, competing evidence, review scope |
| Architect | Decide what to investigate before planning | Entry points, shared state, external contracts, recovery constraints |
| Planning lead | Decide whether detailed planning can begin | Independent assessment results, critical gates and open actions |

Single workstation and local review are the initial operating model. A reviewer name identifies an attestation within the assessment. Authentication and multi-user workflows are outside the first release.

## Release scope

| Priority | Capability | Deliverable |
|---|---|---|
| Required | Reusable intake | Local directory and ZIP, one versioned manifest, inventory, per-file diagnostics, declared evidence categories |
| Required | Code and non-code discovery | Scoped COBOL, copybook, JCL, data, CICS/BMS and typed evidence coverage as specified in the ingestion plan |
| Required | Investigation | Searchable graph and table, scoped job/transaction paths, evidence links and unresolved targets |
| Required | Decision | Separate complexity, confidence and readiness, prioritized evidence requests and candidate planning considerations |
| Required | Review and handoff | Review records, offline HTML, machine-readable JSON, limitations, reuse instructions |
| Required | Demonstration | Same engine and rubric on two original applications, plus an evidence change and reassessment |
| Follow-on | Deeper exploration | Visual run comparison, reviewable boundary analysis and document contradictions |
| Conditional | Tachyon | One optional modernization brief from a selected flow, subject to the dependency boundary |

The baseline uses first-party analyzers, UI assets, fixtures and documentation. The restriction on third-party licensed items applies to product and demonstration materials. The execution environment is unresolved. The [dependency register](../learn/external-dependencies.md) lists the outstanding decisions for the host, language runtime, browser, SQLite distribution and Tachyon. Product contracts and original fixtures can be designed before these decisions. The deterministic workflow has zero outbound calls and requires no model.

## Challenge traceability

| Judging criterion | Weight | Proof prepared for judges |
|---|---:|---|
| Breadth | 25% | Coverage matrix and two flows crossing code, batch, data, transactions/UI, integrations, security and operations |
| Accuracy and evidence | 25% | Independent fixture results, original line links and explicit unknowns |
| Reusability | 20% | Second application with different shape, same executable and rubric, manifest-only changes |
| Explainability | 15% | Score contributions, confidence denominators, readiness gates and actionable closure conditions |
| Extensibility | 10% | Analyzer and rule contracts, plus a documented adapter walkthrough |
| Usability | 5% | A reviewer navigates from summary to evidence and exports a portable report |

## Plan map

1. [Core engine](01-core-analysis-engine.md) defines records, resolution, storage behavior and commands.
2. [Ingestion and evidence](02-ingestion-and-evidence.md) defines file handling, analyzer coverage and non-code inputs.
3. [Assessments and recommendations](03-assessments-and-recommendations.md) defines the initial rubric and review semantics.
4. [UI and user journeys](04-ui-and-user-journeys.md) defines screens, graph and report behavior.
5. [Validation and demo](05-validation-and-demo.md) defines the fixtures, evaluation and judge walkthrough.
6. [Implementation sequence](06-implementation-sequence.md) assigns deliverables, dependencies and exits to work packages.
