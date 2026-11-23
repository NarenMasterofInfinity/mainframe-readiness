# Implementation sequence

Build one end-to-end evidence path before expanding the syntax or polishing the UI. The first target is a literal JCL program reference, its original line, a related missing copybook, a confidence or readiness gap, a recommendation and an exported report from the same run.

## Work packages

Each owner is a role. Assign people after staffing and event duration are known. Effort is relative and used to order work, with no implied delivery date.

| ID | Depends on | Owner / effort | Work and deliverables | Exit |
|---|---|---|---|---|
| P0 | — | Lead / small | Record the exact allowed host/runtime/browser/build/storage boundary. Capture decision, rejected options and local run path. In parallel, assign work owners and agree on the first fixture and test-key ownership. | Executable work has an approved environment. Original fixture, contract and rubric work can continue while this decision is open. |
| P1 | — | Analyst + SME / medium | Specify manifest-free input sets for both original applications, author the first stock job and program, define expected nodes/edges/lines, prepare independent holdout and record intentional gaps. | First sample and independent expectations are reviewable. Fixture authoring follows the [validation plan](05-validation-and-demo.md). |
| P2 | P0, P1 first cases | Engine / medium | Create the local runner, logical schema, inventory preview, safe admitted source reader, source map, run state and recoverable store. Implement inspect and stage diagnostics. | Same preview from equivalent ZIP and folder contents, failed intake has explicit reason, original locations survive normalization. |
| P3 | P2 | Engine / medium | Implement initial lexical scanning, PROGRAM-ID, COPY, literal EXEC PGM, scoped definitions/resolver, evidence and one missing-target rubric rule. Implement machine-readable export for the slice. | Initial job-to-program and missing-copybook path survives assess → show → export. Independent expected record and source line pass. |
| P4 | P3 | Engine / large | Expand supported COBOL, copybook, JCL, SQL and CICS/BMS subset. Implement step context, file binding, resource direction, ambiguity, typed non-code evidence, graph indices, impact queries and diagnostic catalog. | Stock and permits fixtures produce their expected paths and failures without sample-specific code. |
| P5 | P3, stable P4 contracts | Analyst + engine / large | Implement dimension rules, confidence checks, readiness truth table, hotspot and gap ranking, and review projection. Make further rule changes versioned. | Rule corpus covers positive, negative and unknown. All material conclusions trace to signals, gaps and evidence. |
| P6 | P3, P0 UI environment | UI / large | Start from saved report payload. Build intake, overview, accessible graph/table, impact and evidence views, review and export. Add child-run navigation and HTML report. Integrate with P4/P5 contracts as they stabilize. | An analyst can complete intake → finding → original evidence → action → export. Browser and reports run locally. |
| P7 | P4, P5, P6 | QA + demo lead / medium | Freeze rubric and supported subset. Run held-out tests, both clean assessments and network observation. Compare the two Harbor Stock runs. Write operator reuse and limitations notes and rehearse. | Pass the [single end-to-end acceptance walkthrough](../todo.md#the-single-acceptance-endpoint) and attach the [validation evidence](05-validation-and-demo.md). |
| P8 | P5, P6, separately cleared Tachyon | Optional / small | Configure a single optional modernization brief, compare model settings and run claim review. | Accepted candidate outputs include references, uncertainty and reviewer status. Declare the feature enabled or unavailable at P7. The deterministic assessment works in both cases. |

Critical path is environment → inventory and evidence → first slice → full evidence and rubric → end-to-end acceptance. Fixture development and rubric review can begin alongside environment resolution. UI layout and sample report work can start against a documented report example. Use one workstation and one active run per workspace.

## Work products and integration points

Use first-party modules grouped by intake, analyzers, graph resolution, assessment, presentation and local storage. Keep the artifact schema, rule definitions and report version co-located with the owning modules. Use separate original fixture folders for development and held-out cases. Store local assessment output outside source inputs. Implementation commits should leave the existing path runnable in the approved environment and record breaking schema/rubric changes.

For each analyzer change, submit one supported case with an original location, one negative case and a published coverage note. For each rubric change, submit a fixture with the same evidence but a different completeness outcome. For each UI change, show how to reach the relevant evidence from the result. Integrate the two fixture paths as soon as the minimum analyzer set works so the second application can still reveal design assumptions.

The intake team hands the supported source formats to fixture authors and the generated manifest schema to the runner, normalized records from the engine to assessment, the report contract from assessment to UI/export, and frozen rules plus fixtures to QA. Use one small report golden example as a shared integration contract. Reviewers should be able to trace a changed result to a rule or input revision.

## Work order under time constraints

Prioritize the agreed path through searchable relationships, original evidence, all three assessments, the before-and-after evidence view, portable reports and both applications. Integrate boundary and portfolio investigation into the same final walkthrough. Document unsupported variants in the coverage screen and report. If the execution environment remains unresolved, continue original fixtures and contracts and record the blocker against the single acceptance endpoint.

## Reuse handoff

The final operator guide must show the local setup and command or UI entry point, accepted file formats and limits, an automatically generated intake record and a source row example, how to add a new application, how to interpret partial analysis, how to review and rerun, and how to export. Provide an extension recipe that adds one analyzer or rule without rewriting the existing application workflow. A second operator should follow the guide with the second fixture during rehearsal.

## Implementation tickets and integration tests

Use this breakdown to assign work within the packages. Ticket names describe repo responsibilities without choosing a licensed runtime. If parallel work is available, divide items by owner and contract. Feature tests use the named cases in [validation](05-validation-and-demo.md).

| Ticket | Parent | Depends on | Implementation output | Merge check |
|---|---|---|---|---|
| ENV-1 | P0 | — | Record host, runtime, build tools, browser and storage decision. Define local startup and outbound-blocked execution method. | A clean local startup path has an admissible environment and no unresolved needed dependency. |
| CONTRACT-1 | P1/P2 | — | Freeze manifest v1, normalized records, evidence location, diagnostic envelope and report example. | An input and an output example are reviewable and every referenced ID is defined or explicitly illustrative. |
| FIX-1 | P1 | CONTRACT-1 | Harbor Stock slice with one job/program/copybook chain, then full base and enrichment. Independent answer key held separately. | Two reviewers can locate the expected statements and describe open scope. |
| FIX-2 | P1 | CONTRACT-1 | Cedar Permits package and independent variation. Specify model-free expected results. | Fixture workload, paths and names differ from Harbor Stock. |
| IO-1 | P2 | ENV-1, CONTRACT-1 | Admission, staged hashing, file states and normalized logical paths. | IN-01 to IN-04 pass with no run publication on structural failure. |
| IO-2 | P2 | IO-1 | Generate default scope, classify loose/folder/ZIP input, save setting provenance and link source maps. | IN-05 and AN-02 pass with path and original line preserved. |
| RUN-1 | P2 | IO-1, CONTRACT-1 | Run journal, progress/status, artifact snapshots and local read model. | A failed or cancelled attempt remains inspectable without a completed export. |
| PARSE-1 | P3 | FIX-1 slice, IO-2 | Narrow COBOL/COPY and JCL extraction, registration and diagnostic mapping. | One supported call or COPY and a negative lookalike are traceable. |
| LINK-1 | P3 | PARSE-1, RUN-1 | Scoped identity index, reference statuses and first evidence-linked result. | One intentional missing reference stays unresolved. Export of the slice is readable. |
| PARSE-2 | P4 | PARSE-1 | Copybook detail, JCL DD/procedure reference support and explicit unsupported cases, SQL/DDL and CICS/BMS. | AN-03 through AN-08 pass, including the bounded behavior recognizer and copybook field dictionary. |
| EVID-1 | P4 | CONTRACT-1, LINK-1 | Direct non-code readers, plain-text evidence leads, optional typed evidence, scoped statement links and conflicts. | EV-01 and EV-02 preserve separate provenance and diagnostic. |
| GRAPH-1 | P4 | LINK-1, PARSE-2 | Step-scoped bindings, graph indices, counts, impact paths and caps. | GR-01 through GR-03 pass and traversal modes are documented. |
| RULE-1 | P5 | LINK-1, CONTRACT-1 | Evidence catalog, typed evaluator registration and initial dependency gap. | An invalid profile is rejected with rule/path context. A quote or comment cannot create a supported rule signal. |
| SCORE-1 | P5 | RULE-1, EVID-1, GRAPH-1 | Full rubric, ranked hotspots, open gaps, suggested actions, and blockers. | AS-01 through AS-04 match the band/gate cases and prove that evidence can change one output independently. |
| REV-1 | P5 | RUN-1, RULE-1 | Append-only review, stale revision handling and affected projection. | RV-01 preserves original snapshot and action history. |
| UI-1 | P6 | ENV-1, CONTRACT-1, RUN-1 | Intake, run overview and inventory using the shared operation contract. | One real run is available after refresh with correct progress and data. |
| UI-2 | P6 | UI-1, GRAPH-1, SCORE-1, REV-1 | Evidence and graph/table, impact, hot spots, gap actions and review. | The route from a gate or hotspot to its scoped source works by keyboard. |
| REP-1 | P6 | CONTRACT-1, SCORE-1, UI-2 | Shared report rendering, local export and verified evidence references. | RP-01 and RP-02 pass. Export uses the selected review revision. |
| DELTA-1 | P6/P7 | REP-1, FIX-1 enrichment | Child run entry, base/result links and a change table. | Evidence addition leaves the base intact and the table shows both resolved and remaining questions. |
| SHIP-1 | P7 | FIX-2, REP-1, DELTA-1 | Held-out evaluation, reuse and extension proof, offline checks, judge rehearsal. | RE-01 and RE-02 outcomes and limitations are recorded. |
| BOUND-1 | P4/P6 | GRAPH-1, UI-2 | Seeded boundary comparison and evidence caveats. | GR-04 passes. Candidate and analyst edit remain reviewable. |
| AI-1 | P8 | ENV-1 plus Tachyon eligibility, REP-1 | One selected flow brief and display of evidence references and unsupported claims. | Independent optional-model checks pass. Core output stays available when it is disabled. |

Where a ticket depends on “slice,” integrate that subset first. Keep separate branches or tasks small enough to merge while the shared case still runs. Every ticket closes with a working schema example, a documented limitation or test outcome. A screenshot without underlying source links does not complete an evidence feature.

## Time-boxed execution options

Use this three-day order only if it fits the event and available staffing. Rehearse actual performance before promising a schedule.

| Window | Engineering focus | Demonstrable checkpoint |
|---|---|---|
| First two hours | Confirm execution decision, lock manifest/evidence vocabulary and goldens, assign owner roles | Everyone can name the first input, output and unsupported case. |
| Day 1 | Work IO-1/2, PARSE-1/LINK-1 and the report skeleton; build both original fixture paths in parallel | Source input → inventory → one resolved and one unresolved edge → clickable source or CLI evidence → JSON slice. |
| Day 2 | Extend parsers and typed evidence, add graph impact and rubric, connect UI to real results | Run Harbor Stock end to end, explain all three outputs and a blocker, export HTML/JSON. |
| Day 3 | Reassess after new evidence, run Cedar Permits, finish validation, reuse, reporting and rehearsal | Judge path works offline, including second app and supported comparisons. Verify the boundary, portfolio and files-only cases at the same endpoint. |

For a shorter window, use the same acceptance endpoint and published syntax subset. Parallelize independent engineering work and leave any unmet requirement open. Prioritize original fixtures and the complete slice before parser breadth. The second application must remain a distinct application. Document actual non-code and online coverage even if it becomes limited. If the environment remains undecided, the contract examples and original test fixtures can advance independently.

## Integration checks before each milestone

- Run the current completed slice from the normal UI/CLI entry point and open a source-linked finding from an export.
- Verify that every change carries the intended run, effective manifest and active review revision.
- Recheck supported constructs with a negative test and record any scope reduction.
- Record open work, evidence limitations and prerequisite decisions against the final acceptance checklist.

## Integration responsibilities for the single endpoint

| Owner and tickets | Handoff to the next owner | Proof before final acceptance |
|---|---|---|
| Intake and runner, IO-1/2 and RUN-1 | Source selection → safe receipt, generated settings, immutable bytes and original paths | FLOW-01, FLOW-02 and FLOW-05 from the normal picker and CLI |
| Parser and evidence, PARSE-2 and EVID-1 | Native CSD/access statements, supported schedule/interface rows and text leads → scoped facts or diagnostics | FLOW-03 and FLOW-04 on raw resources alone, with unrelated and conflicting input variations |
| Graph and rubric, GRAPH-1, BOUND-1 and SCORE-1 | Flow index, crossing edges, provisional signals, independent checks, gates and cited actions → one report model | GR-04 and AS-01 through AS-04, plus a trace from an initial result to a real source |
| Presentation and publication, UI-1/2 and REP-1 | Picker event → progress → automatic result and atomic HTML/JSON pair. Local application list → comparison and drill-down | FLOW-06 and FLOW-07, revision parity and a working screen with the model disabled |
| Demo, DELTA-1 and SHIP-1 | Same build, two distinct resource sets, one additional schedule file and independent keys → recorded proof | Attach all seven FLOW case outcomes and the named domain cases to the single endpoint |

Work packages are engineering dependencies. The product has one acceptance decision covering the full workflow, including boundary and portfolio views. The eligibility decision for the optional Tachyon feature is documented with the same handoff.
