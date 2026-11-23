# Validation and demo

## Original applications

| Application | Different shape | Required evidence and expected outcomes |
|---|---|---|
| Harbor Stock | Batch inventory posting and the overnight handoff to reporting | Two jobs, distinct steps, literal CALL and COPY, logical file/DD binding, a writer and reader for one dataset, one missing copybook and an unsupported JCL construct. Initial schedule and recovery evidence are incomplete. Add an original scheduler record and show that availability changes while the recovery question can stay open. |
| Cedar Permits | Interactive permit lookup and status update | Transaction-to-program and BMS mapping, literal CICS transfer, selected SQL read/write, declared external notification, role/audit evidence and an ambiguous or dynamic reference. Show transaction and integration findings with batch applicability reviewed separately. |

Author all source, non-code evidence, sample report examples and fixtures for this project. Keep setup instructions, expected interpretations and deliberate gaps with the fixture definitions. Use the same executable, schema and rule version for both. Select only each application’s ordinary resource files. Use optional configuration in separate override tests. Label sample owners and attestations as part of the synthetic scenario. Report the limits of validating against synthetic fixtures.

## Independent answer key

Before testing, an independent reviewer records expected artifact statuses, nodes, relationship types and endpoints, resolution and evidence lines for named cases. They inspect the inputs and applicable semantics without seeing tool outputs. For rule conformity, derive expected bands and gate outcomes separately from the frozen rubric. For rubric usefulness, ask an SME to judge sample planning decisions and omissions from source and domain evidence before revealing the computed ratings. Keep both sets of results separate. Preserve the signed-off keys and any disputed entries. Keep a separate held-out set that implementers do not inspect while tuning. Use independently authored variations so neither sample identifiers nor previously reviewed test prompts can stand in for reasoning. If the team has no independent SME, record the reviewer role and label domain judgments preliminary.

Use a development corpus for iteration and execute the held-out set after rules are frozen. Evaluate deterministic extraction and the optional model separately. Match observed graph outputs against typed endpoints, context, resolution and original locations. Report true/false/missed edges and correctly located expected relationships, by construct and application. Compute precision and recall only when denominators are nonzero and show underlying counts. Any adjudication of a disputed answer key must document its source and preserve the first result. One end-to-end demo outcome cannot substitute for these checks.

## Test matrix and exit criteria

| Area | Cases that must be exercised | Passing behavior |
|---|---|---|
| Admission | Normal folder/ZIP, malformed or out-of-policy archive, empty input, over-limit input, unsupported binary and incorrect encoding | Safe failure or explicit per-file result, with correct stage and count |
| Extraction | Comment and quoted lookalikes, fixed/free sources, continuation, supported and unsupported constructs | No phantom edges in negative cases, precise evidence for expected positives and visible partial status |
| Resolution | Scoped duplicate, dynamic call, literal missing, external declaration, one program bound differently by two steps, shared reads | Correct statuses and step-scoped bindings. No ordering inferred from shared reads. |
| Evidence | Multi-span statement, moved or changed input, conflicting evidence | Original locations and verified snapshot/hash. Conflict stays visible. |
| Rubric | Unknown vs Low, Moderate provisional, higher confidence with new risk, applicable gate failure and reviewed exclusions | Matches written rule cases, with linked rationale and no premature Ready result |
| Review | Rejected inference, competing evidence, stale revision and resolution without support | Original output retained. Stale action rejected. Gate changes only with valid evidence and review. |
| Reports | Initial, subsequent and incomplete runs, escaped labels, snippet policy, offline open | One completed revision throughout, valid references and useful report without a running app |
| Reuse | Both applications from a clean workspace | Both run with the same logic and rubric, with configuration and limitations documented |
| Offline | Approved environment with outbound access denied, both engine and UI/report | Workflow completes and observation shows zero attempted outbound requests |
| Optional model | Held-out rule candidates, unsupported claims, omitted context and invalid references | Review-supported claims reported separately, schema and evidence failures visible, core scores unchanged |

End-to-end acceptance requires zero unresolved failures in supported golden cases, evidence or explicit gaps for all material displayed claims, exported results for both original applications and a documented offline walkthrough that includes the Harbor Stock reassessment. For optional AI, tune by comparing independently assessed support and omissions, latency and repeatability. Freeze the selected model and settings before holdout evaluation. Use independently prepared answer keys for model evaluation. [Final acceptance walkthrough](../todo.md#the-single-acceptance-endpoint)

## Judge walkthrough

Allow roughly five minutes and keep matching precomputed reports as a clearly labeled fallback.

1. **0:00–0:45** Choose the Harbor Stock files and take hands off the controls. Show its automatically published result, detected scope, inventory and one unsupported input.
2. **0:45–2:00** Open the handoff, trace job/step/program/dataset evidence and explain why a static handoff leaves scheduling unknown.
3. **2:00–3:00** Open the recommendation, add schedule evidence and show the new run. Explain resulting changes in confidence and readiness with the surviving recovery question.
4. **3:00–4:15** Run Cedar Permits with the same tool. Inspect the different transaction/SQL path and ambiguity. Inspect a boundary crossing and compare the scopes in the local application list.
5. **4:15–5:00** Open the exported report and show the evidence appendix, reuse guide and test results.

Record actual run timings before choosing which parts to perform live. Packaged materials include both manifest-free fixtures, separate override examples, expected outputs, validation results, offline instructions and limitations. Keep the brief focused on planning decisions and demonstrated evidence.

## Fixture packages and annotation format

Use three labeled input sets. Harbor Stock base and Harbor Stock enriched share one application identity. The enriched set exists to exercise change attribution and is not the second reuse case. Cedar Permits has a separate identity, workload and application files. All content is first-party and synthetic.

| Package | Minimum constructed inputs | Demonstration assertion |
|---|---|---|
| Harbor Stock base | `STOCKRUN` and `STOCKRPT` JCL, `POSTSTK` and `RPTSTK` COBOL, a supported copybook, one missing COPY target, one unsupported JCL variant and one deliberately unsupported binary member. `BUILD` writes through `STOCKDD`, `REPORT` reads through `REPORTDD`, and both bind `STOCK.OUT`. | Separate job/step/program/dataset edges, possible handoff, unresolved copybook, partial diagnostic, Moderate provisional batch signal with schedule and recovery gaps. |
| Harbor Stock enriched | Same program bytes and effective rule profile. Add just the original scheduler CSV with predecessor, successor and successful completion condition. Leave recovery evidence absent. | The source row documents the order immediately. Only a separate attributed review can confirm applicability. Existing handoff remains. Recovery gap persists. A confidence band or readiness label can stay unchanged. |
| Cedar Permits | Independent online program names, a bounded IF/MOVE status assignment, a declared copybook field, transaction binding, BMS map, table DDL and embedded read/write statements, an external interface declaration, access/audit evidence and an ambiguous or dynamic reference | Transaction, screen and table path with different planning questions. The initial report leaves batch applicability open. A separate review may establish its applicability. |

Each expected occurrence includes test ID, artifact path, original span(s), source component kind and qualified key, raw target, expected type and resolution, and forbidden outputs for negative cases. Rules have an independent expected case table with evidence inputs and expected signals, confidence check outcomes, gates and action keys. Distinguish incorrect tool output from inconclusive reference interpretation. The reviewer may mark the latter uncertain and preserve the reason before scoring.

## Named acceptance cases

| ID | Setup | Expect and check |
|---|---|---|
| IN-01 | Equivalent folder and ZIP with matching logical paths and no manifest | Identical logical inventory, hashes and machine assessment content after excluding packaging metadata. |
| IN-02 | Unsafe/colliding paths or byte limits exceeded | Structured rejection with no completed snapshot and no writes outside the staging area. |
| IN-03 | Duplicate bytes at two paths in separate libraries | Two inventory rows. Shared digest. Resolution follows configured library precedence. |
| IN-04 | Malformed or unreadable file beside supported source | Run completes with a diagnostic and a partial coverage view. Valid findings remain individually traceable. |
| IN-05 | Extensionless supported statement and ambiguous layout | Classification records the clue. Analyzer uses an explicit format or records a partial result. |
| AN-01 | Literal CALL, identical text inside comment/literal, identifier CALL | One resolved or scoped literal reference, no edges from lookalikes and one dynamic reference pending evidence. |
| AN-02 | Continuation with multiple original spans | Every contributing line can be opened from one statement occurrence. |
| AN-03 | Nested COPY cycle plus COPY needing substitutions | Reference cycle visible. Document unsupported expansion. No recursive loop or fabricated layout offsets. |
| AN-04 | One program called by two jobs with different DD bindings | Two scope-specific bindings. Changes to one step do not alter the other. |
| AN-05 | JCL in-stream text that resembles a step | No phantom component or edge, with partial coverage if the construct is unsupported. |
| AN-06 | Named table, quoted lookalike and unsupported dynamic SQL | Supported reference has original evidence. Complex target stays unresolved with diagnostic. |
| AN-07 | Valid CICS literal map and unknown variable or missing map definition | Expected literal reference and correct unresolved status for the variable or absent definition. |
| AN-08 | One bounded IF/MOVE and an unsupported nested or ELSE variant. Two same-name fields in different copybook groups. | Only the supported condition/effect appears as a candidate, with original spans. Dictionary rows retain parent paths. Member inclusion never establishes a field read. |
| EV-01 | Two disagreeing current declarations for the same scoped subject | Both sources remain. Requirement says conflicted, with a review action. |
| EV-02 | Unrelated export or changed original source file | Availability for the original question does not pass. Changed-byte location is stale unless a stored verified snapshot is available. |
| GR-01 | Parallel edges, cycle, self-loop and disconnected component | Repeated query returns the same ordering, typed paths and unique impact counts. Root excluded from counts. |
| GR-02 | Impact query level has more than the screen/table cap | Display count and traversal limit are distinct. Every reachable visited row can be paged. |
| GR-03 | Two readers of the same dataset without a writer | Shared resource only. No handoff or schedule edge. |
| GR-04 | Boundary candidate overlaps another and one resource has an external writer | Both memberships and the crossing caveats visible. No architecture decision generated. Required for the single delivery. |
| AS-01 | Observed high operations complexity with complete reviewed planning requirements | High complexity can coexist with high confidence and Ready for detailed planning. |
| AS-02 | All relevant analyzers unsupported and no usable supporting evidence | Unknown complexity, unknown confidence and incomplete gates. No false simplicity. |
| AS-03 | Add usable schedule evidence with unknown restart | The old run is intact, schedule coverage changes, the operational gate can stay Unknown. No automatic band increase. |
| AS-04 | Missing material target is supported by approved evidence later | Reference requirement can resolve after reassessment or documented review. Prior machine evidence and gate result remain available. |
| RV-01 | Two attempted reviews of same run revision | First valid event persists. Stale event returns conflict and does not overwrite it. |
| RP-01 | Report opened offline with excerpts disabled | All report sections and evidence locators available. JSON references match the HTML revision. No unauthorized source text emitted. |
| RP-02 | Cancel, retry and export | No ordinary completed report from cancelled state. Retry is linked to the earlier run. Output uses the selected completed revision. |
| RE-01 | Run Cedar Permits after Harbor Stock on clean workspace | Same runtime artifacts, analyzer registry and rules. Only select new files. No mapping, template or previous scope is supplied. |
| RE-02 | Add one small first-party test analyzer or rule | Demonstrate registration with a supported fixture, a negative fixture and standard graph/report evidence. No application-specific branch. |

## Validation protocol and reportable measurements

Run three layers of checks. Isolated cases exercise one supported form and its negatives. A canonical snapshot checks complete relationships, source references, rules, UI contracts and report consistency. A blind holdout uses independent original variations. Freeze methods before the holdout. Use representative supported constructs and report denominators per family. Keep unsupported syntax listed independently of accuracy claims. Engineering goldens may repeat known inputs. They do not replace independent labels.

For each relationship family, compare unique typed endpoints and context, then separately measure original location correctness. If 8 expected edges exist, 7 are emitted and 1 emitted edge is incorrect, report 6 true positives, 1 false positive and 2 misses. Example precision is 6 of 7 and recall 6 of 8. These numbers illustrate reporting and are not observed results. Record coverage and rejected findings separately. Measure rubric conformity against the written rule and rubric usefulness against independent SME decisions. Do not grade usefulness by agreement with the same score generator.

Repeat each baseline on the same versions and inputs. Normalize run IDs, timestamps and review events where appropriate and compare content. Add invariant cases: missing inputs never add complexity points, resolution can change only when supported, changing unrelated evidence may still surface an applicability or consistency update, and any changed assessment result retains a rationale. Do not promise that new evidence always raises confidence, improves readiness or narrows uncertainty. Evidence can contradict an earlier claim.

Capture elapsed time and input size separately for intake, analyze, link, assess and export. Start with a demo usability target of completing both small synthetic packages within the rehearsed timing. Record the host and measurements before claiming larger capacity. Check graph and evidence views with keyboard navigation, filtered empties and a large fixture with deterministic cap behavior. Observe local engine/UI/export while outbound networking is blocked. The optional model path is evaluated separately under its approved boundary.

## Demo exit checklist

- Both applications open from a clean local workspace and produce portable reports.
- Harbor Stock demonstrates one source-linked handoff, one missing or unsupported item, one action and a before/after evidence change.
- Cedar Permits demonstrates online and data paths through the same tool.
- A selected component shows impact paths and caveats, with a direct link back to evidence.
- A judge can distinguish all three assessments and identify the reason for an unresolved gate.
- The reuse guide, limitations and case results are visible in the handoff.
- Any optional feature is shown only after its relevant acceptance cases and policy conditions are met.

## Files-only acceptance cases

For each run capture the exact selection, fixture digest, build and rubric versions, mandatory fields, blocking prompts after selection, elapsed time, terminal outcome, paths to both reports and a sampled evidence locator. Start with an empty workspace and the model disabled. Keep independent keys outside the submitted inputs. These are acceptance specifications pending execution.

| ID | Exercise | Pass condition |
|---|---|---|
| FLOW-01 | A new operator selects ordinary Harbor Stock files with no manifest, saved profile or questionnaire. Repeat using an equivalent folder and wrapped ZIP. | Zero metadata fields and zero blocking prompts for each. All three answers and both files appear automatically. For identical logical paths, normalized machine results match. |
| FLOW-02 | Supply an extensionless job, a mislabelled source, duplicate copybook names, a mixed batch/online export and an irrelevant runbook. | Detect affirmative clues, preserve both workload observations, unresolved candidates and diagnostics. No guessed library order, reviewed exclusion or document-backed gate pass. Result still publishes. |
| FLOW-03 | Submit Cedar as a fresh selection after Harbor. Include literal CSD, COBOL, SQL/DDL, interface table and access source. Repeat with the same display name as Harbor. | Distinct application key. Inspect transaction → program → table, interface declaration, stated access and the outstanding effective-access question. No previous bindings, reviews or settings leak. |
| FLOW-04 | From Harbor select Add evidence and choose only the original scheduler CSV. Try an unrelated CSV, duplicate headers and missing jobs as negative cases. | Only a usable, scoped row supplies the schedule claim. Zero manual mapping. Compare the two snapshots, verify unchanged program bytes, locate the new row and keep recovery open. |
| FLOW-05 | Submit an admitted selection consisting entirely of unsupported files, then an empty or structurally invalid selection. | First returns a completed report pair and replacement request. Second returns a rejection and no completed pair. Neither asks for a scope form. |
| FLOW-06 | Trigger duplicate UI submission, interrupt before the report commit, retry, then open both default exports with the application closed. | One run per submission token, coherent published pair per successful run, no half-published result. With networking disabled, reports and evidence locators remain usable. |
| FLOW-07 | Ask a new reviewer to identify a blocker, trace one edge to source, inspect a crossing dependency, compare both app scopes and name the evidence needed next. | Capture actions taken, wrong turns and task completion. Verify the same journey by keyboard. All claims and requested evidence trace back to the selected run/revision. |
