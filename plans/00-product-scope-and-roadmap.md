# Product scope and roadmap

The workbench takes a local export of a mainframe application and produces an evidence-backed decision about starting detailed migration planning. It assesses the supplied application in place. The deliverable is an assessment report, dependency map and action list. OpenShift can be recorded as a target assumption. The project scope is assessment and planning support.

## One selection produces a planning report

On an installed workstation, select loose resource files, a directory or one ZIP. Selection starts the assessment. The workbench supplies the run identity, inventories the material, detects supported formats, follows local references, applies the shared rubric and opens a result with HTML and JSON already saved. Reviewers can inspect a dependency, add evidence or challenge an interpretation after reading the result.

The [files-to-results plan](07-files-to-results.md) specifies each handoff, exception and researched design decision. The [illustrated walkthrough](../learn/09-files-to-results.md) follows the operator through the same journey.

## Roles and decisions

| Role | Decision or task | Evidence they need |
|---|---|---|
| Analyst | Supply the resources and inspect the discovered perimeter | Expected exports, inventory, parser coverage, unresolved inputs |
| Mainframe or domain SME | Confirm or challenge a claim | Original location, competing evidence, review scope |
| Architect | Decide what to investigate before planning | Entry points, shared state, external contracts, recovery constraints |
| Planning lead | Decide whether detailed planning can begin | Independent assessment results, critical gates and open actions |

The operating model is one workstation with local review. A reviewer name identifies an attestation within the assessment.

## Delivery scope

| Priority | Capability | Deliverable |
|---|---|---|
| Required | Reusable intake | Loose files, directory or ZIP, generated intake record, coverage and per-file diagnostics |
| Required | Code and non-code discovery | Scoped COBOL, copybook, JCL, data, CICS/BMS, schedule, interface and access readers with original document leads as specified in the ingestion plan |
| Required | Investigation | Searchable graph and table, typed Change Impact Explorer, scoped job/transaction paths, evidence links and unresolved targets |
| Required | Decision | Separate complexity, confidence and readiness, risk hotspots and an Evidence Gap Planner with closure criteria |
| Required | Review and handoff | Review records, offline HTML, machine-readable JSON, limitations, reuse instructions |
| Required | Demonstration | Same engine and rubric on two original applications, plus an evidence change and reassessment |
| Required | Planning exploration | Selected evidence-delta view, targeted contradiction checks and candidate migration considerations |
| Required | Boundary exploration | Entry-point-seeded boundary analysis, editable alternatives and cross-application portfolio view |
| Conditional | Tachyon | One optional modernization brief from a selected flow, subject to the dependency boundary |

The baseline uses first-party analyzers, UI assets, fixtures and documentation. The restriction on third-party licensed items applies to product and demonstration materials. The execution environment is unresolved. The dependency register lists the outstanding decisions for the host, language runtime, browser, SQLite distribution and Tachyon. Product contracts and original fixtures can be designed before these decisions. The deterministic workflow must run with zero outbound calls and without a model. [Dependency register](../learn/external-dependencies.md)

One final acceptance walkthrough covers the full workflow on Harbor Stock, its evidence-driven reassessment and the separate Cedar Permits assessment. The implementation packages support that endpoint. [End-to-end acceptance checklist](../todo.md#the-single-acceptance-endpoint)

## Design decisions from the reference blueprint

The supplied implementation blueprint is a reference for feature depth and delivery structure. Its strongest ideas are a normalized model with evidence provenance, parsers described through supported and unsupported cases, graph queries tied to decisions, and independent validation with a repeatable demo. The workbench adopts those ideas with the following decisions.

| Reference strength | Plan decision | Implementation owner |
|---|---|---|
| Investigation starts from a concrete component and shows affected callers, jobs or transactions | Include a bounded Change Impact Explorer with typed directional paths, evidence along the path and explicit coverage limits | [Core engine](01-core-analysis-engine.md), [UI](04-ui-and-user-journeys.md) |
| Recommendations expose missing evidence and the action needed to resolve each gap | Include a gap planner and evidence-backed hotspot queue. Rank the investigation using blocker status and reachable critical entry points. | [Assessments](03-assessments-and-recommendations.md) |
| Evidence improvement is visible to judges | Use a real child run with a scheduler overlay and show what the new evidence supports. Use saved snapshots if live timing requires it. | [Validation](05-validation-and-demo.md) |
| Boundary candidates explain shared data and external coupling | Include a seed-and-compare workflow with raw crossing edges, ownership gaps and explicit preconditions | [Core engine](01-core-analysis-engine.md), [UI](04-ui-and-user-journeys.md) |
| Worked scoring examples and explicit prerequisites make scores auditable | Keep the established band-based rubric and required planning gates. Add rule IDs, display raw signals and label unresolved factors. | [Assessments](03-assessments-and-recommendations.md) |
| A concrete input and output contract lets a team build in parallel | Give the original, local implementation a versioned manifest, source-linked evidence, operation payloads and a shared report fixture | [Ingestion](02-ingestion-and-evidence.md), [Sequence](06-implementation-sequence.md) |

The reference proposes several licensed frameworks and libraries. The product and demo use original material and must meet the existing dependency restriction. The reference also treats writer-to-reader relationships as inferred scheduling and assigns confidence values such as 0.99 to parsed statements. This plan records a possible data handoff separately from evidenced schedule order and uses qualitative evidence checks. It keeps example enrichment distinct from the second application used to prove reuse.

## Product acceptance by user journey

| Journey | Minimum demonstrable outcome |
|---|---|
| Import a ZIP or folder | Inventory accounts for admitted inputs. Invalid packages produce actionable failures. Supported-but-incomplete files expose coverage and locations. |
| Investigate an entry point | A job or transaction shows a selected path and its supporting lines. Changing the graph root does not change the underlying snapshot. |
| Assess a component change | A selected dataset, copybook or program returns distinct direct and indirect dependents with paths, edge evidence and capped-scope indicators. |
| Prepare a planning decision | The reviewer can explain a provisional complexity result, a confidence denominator and an outstanding readiness gate, then find the action that would address it. |
| Add evidence | A new run records the input change and the previous run remains available. The report shows whether the new evidence closed a question or raised another. |
| Hand off and repeat | Export works locally. The second original application uses the same parser registry and rule profile, using the new resource files alone. |

The delivery target is a single workstation and one active assessment process per workspace. Each result states its supplied-file perimeter and whether the business perimeter has been reviewed. Real company artifacts, portfolio-wide completeness, production deployment decisions and automatic conversion fall outside the synthetic hackathon demonstration. The [validation plan](05-validation-and-demo.md) defines what the demo can substantiate.

## Challenge traceability

| Judging criterion | Weight | Proof prepared for judges |
|---|---:|---|
| Breadth | 25% | Coverage matrix and two flows crossing code, batch, data, transactions/UI, integrations, security and operations |
| Accuracy and evidence | 25% | Independent fixture results, original line links and explicit unknowns |
| Reusability | 20% | Second application with different shape, same executable and rubric, resource files alone |
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
7. [Files to results](07-files-to-results.md) ties the implementation to one user journey and records the commercial and open-source research.
