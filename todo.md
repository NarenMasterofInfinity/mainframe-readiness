# End-to-end delivery checklist

**Current state.** The repository contains eight implementation plans, eleven learning guides and a reference blueprint. The audit found no application code, runnable fixture bundles, acceptance tests, generated assessments or demo package. The plans describe the intended behavior. This checklist tracks the missing work and the proof needed to call it delivered.

**First decision.** The permitted execution environment is still unresolved under the exclusion of all third-party licensed items. Record the actual host, runtime, build tools, browser and storage choice before depending on them. Original fixtures, contracts and rule cases can progress while that decision is open. [Dependency register](learn/external-dependencies.md)

## The single acceptance endpoint

From a clean workspace in the permitted environment with outbound access blocked, one reviewer must be able to complete this walkthrough using the same runnable build:

1. Select only the original Harbor Stock resources. Verify zero required metadata fields and zero blocking prompts after selection. The overview opens with the generated scope, coverage, gaps and saved HTML and JSON.
2. Explain complexity, confidence and readiness separately. From a hotspot or blocking gate, open the affected dependency path, original source evidence and the action needed to close the gap.
3. Choose only the scheduler CSV in Add evidence, let the child assessment finish and compare the two runs. Verify the supported scheduling change and the still-open recovery question. Inspect a review record and confirm the original machine result is available.
4. Assess Cedar Permits with the same engine, schema and rubric. Follow a transaction-to-data path, inspect a boundary crossing and compare the two application scopes.
5. Open the automatically saved HTML and JSON, read the HTML independently and verify the matching run and review revision. Hand over the operator guide, limitations, fixture bundles and recorded acceptance results.

The endpoint passes when the entire walkthrough succeeds and every required checklist item has evidence. Individual work packages are implementation checkpoints. If any required capability fails, record the blocker and keep the endpoint open.

## How to use this checklist

- Work through the sections in order and build the first complete evidence path early. The [implementation tickets](plans/06-implementation-sequence.md#implementation-tickets-and-integration-tests) give dependencies and merge checks.
- Each checkbox describes a gap. **Deliver** says what must exist. **Verify** says what to demonstrate or record before checking it off. Record file paths, run IDs, test results and unresolved limitations with the handoff.
- Sections 1–9 cover the agreed workbench, including the selected change view, contradiction checks and candidate migration considerations. Section 10 covers boundary and portfolio proof and the eligibility of auxiliary features. Any included feature is verified in the same delivery.
- Use the actual executable, its exports and recorded checks to verify runtime items. A plan, screenshot or mock can document intent or layout.

## 1. Resolve the execution boundary and build the first slice

Source plans: [Roadmap](plans/00-product-scope-and-roadmap.md) · [Implementation sequence](plans/06-implementation-sequence.md)

- [ ] **Permitted runtime and reproducible setup are missing (ENV-1).** Deliver a decision record for the exact host, runtime, build, browser, storage and demo environment, with the origin and status of each external item. Select an eligible SQLite distribution or the first-party snapshot file store permitted by the environment. Provide local setup, launch and test instructions. Verify from a clean workspace that the chosen setup needs no forbidden item or outbound runtime service.
- [ ] **Versioned contracts and a shared fixture result are missing (CONTRACT-1).** Deliver schemas for the generated manifest and optional typed evidence, normalized records for artifacts, components, references, relationships, evidence, metrics, findings, checks, gates, actions and reviews, plus a diagnostic and report contract. Verify the example payloads, internal references and schema versions. Mark illustrative IDs where the example is intentionally partial.
- [ ] **Runnable scaffolding and the first vertical slice are missing (RUN-1, PARSE-1, LINK-1).** Deliver the local runner and callable inspect, assess, show and export operations. Process an original Harbor Stock input from a JCL program reference to its source lines, then a missing copybook, its gap and an actionable JSON finding in the same run. Verify from the normal entry point with an independently labeled expected reference and an intentional unresolved case.

## 2. Create original applications and independent expected results

Source plan: [Validation and demo](plans/05-validation-and-demo.md#fixture-packages-and-annotation-format)

- [ ] **Harbor Stock base and enrichment bundles are missing (FIX-1).** Deliver manifest-free original job, COBOL and copybook inputs for `STOCKRUN/BUILD` and `STOCKRPT/REPORT`. Include scoped DD bindings to `STOCK.OUT`, one missing copybook and an unsupported JCL case. Keep schedule and recovery evidence incomplete in the base. Enrich the same application with a source-linked scheduler rule and leave recovery open. Verify the program bytes and rule versions match across runs, and record the intended change and still-open requirement.
- [ ] **Cedar Permits is missing (FIX-2).** Deliver a separate original online application with transaction/program and BMS mapping, embedded SQL and DDL, a declared external interface, access or audit evidence, and an ambiguous or dynamic target. Verify it runs with the same executable, parser registry, schema and rubric. The only new operator input is the Cedar resource selection.
- [ ] **Independent answer keys and holdout inputs are missing.** Deliver a development corpus and independently authored held-back variations. Record expected artifact statuses, typed endpoints, execution scopes, resolution states, original source spans and forbidden outputs for negative cases. Verify the key was produced without tool output and the holdout was held back through tuning. Record reviewer roles and any preliminary or disputed judgments.

## 3. Implement intake, source mapping and analyzers

Source plan: [Ingestion and evidence](plans/02-ingestion-and-evidence.md)

- [ ] **Loose-file, folder and ZIP intake are missing (IO-1).** Deliver bounded, read-only intake with staged hashing, automatically scoped selection, path and archive admission checks, documented size limits and per-file inventory. Verify equivalent folder and ZIP contents produce equivalent machine results. Rejected bundles preserve an actionable diagnostic without publishing a completed assessment or writing outside staging.
- [ ] **Detection details and source locations are missing (IO-2).** Deliver an automatically generated intake record with application, scope, workload, expected categories, search order and per-path overrides. Show detection reason, encoding, format, duplicate content, identity collisions and analyzed, partial, failed, unsupported or excluded status. Verify a corrected classification, explicit encoding failure and a continued statement whose original spans open correctly.
- [ ] **First-party artifact analyzers are missing (PARSE-1, PARSE-2).** Deliver the supported COBOL, copybook, JCL, SQL/DDL and CICS/BMS subsets, with a common analyzer interface and a documented coverage matrix. Include literal and dynamic calls, COPY and layout facts, job/step/procedure/DD references, scoped data operations, transaction/program/screen references, bounded condition/effect candidates and the copybook field dictionary. Verify supported statements, comment and string lookalikes, unsupported variants and missing targets against original spans.
- [ ] **Non-code evidence intake is missing (EVID-1).** Deliver direct readers for supported schedule CSV, CSD resource definitions, access statements and interface registers, plus runbook leads and optional typed claims linked to original documents for schedule, recovery, monitoring, business behavior and baseline tests, data ownership, interfaces, and access/audit. Verify an unrelated document leaves the requirement open, incomplete rows preserve diagnostics, and conflicting declarations and their sources remain available to the reviewer.

## 4. Connect dependencies and support impact investigation

Source plan: [Core engine](plans/01-core-analysis-engine.md)

- [ ] **Reference resolution and execution context are missing (LINK-1, GRAPH-1).** Deliver a scoped identity index and explicit statuses for resolved, missing, ambiguous, dynamic, unsupported and declared external references. Keep definition evidence, reference occurrences and all contributing spans. Verify that identical names in separate libraries follow configured precedence and that one program can bind to different datasets in two distinct steps.
- [ ] **Evidence graph and metrics are missing (GRAPH-1).** Deliver typed nodes and edges for job, step, program, copybook, data, transaction, screen and external interface paths. Preserve separate evidence for observed references, derived relationships and review. Verify that a writer-to-reader handoff needs supported scope and direction, two readers imply no handoff, and schedule precedence requires separate schedule evidence. Graph counts distinguish source occurrences from unique components and disclose partial coverage.
- [ ] **Change Impact Explorer is missing (GRAPH-1, UI-2).** Deliver Dependencies, Dependents and Data handoff modes with a selected root, edge filters, direct and indirect counts, supporting paths, affected entry points and unresolved caveats. Verify repeatable traversal and pagination on cycles, self-loops and parallel edges. Show the difference between display limits and traversal limits and label capped counts.

## 5. Produce explainable assessments and actions

Source plan: [Assessments and recommendations](plans/03-assessments-and-recommendations.md)

- [ ] **Executable rubric and evidence requirements are missing (RULE-1, SCORE-1).** Deliver validated, versioned rule and requirement catalogs for code, data, batch, transactions/UI, integrations, security and operations. Track layout and database/store evidence within Data. Every material finding carries the rule and subject, observed evidence or explicit gap, inference, assumptions, conflicts and unknowns. Verify invalid configuration is rejected and results follow identical rules across applications.
- [ ] **Three independent outputs are missing (SCORE-1).** Deliver complexity bands with signal and provisional rationale, confidence with applicable check totals and reasons, and readiness with explicit scope, dependency, business/data, operations, security/interface and review gates. Verify Unknown differs from Low, an unresolved critical gate prevents an unconditional Ready result, and a high-complexity case can qualify for detailed planning when its gates pass.
- [ ] **Ranked hotspots and an Evidence Gap Planner are missing (SCORE-1, UI-2).** Deliver trigger and evidence, impact or affected scope, priority rationale, suggested owner role, requested action and completion criterion. Verify ranking is reproducible, links open the correct impact scope, and acknowledging a task leaves its gate governed by evidence. Show which checks an action could affect. Record the actual result after reassessment.
- [ ] **Targeted contradiction checks and candidate migration considerations are missing.** Deliver scoped checks for comparable conflicting claims and conditional considerations that state the trigger, constraints to preserve, trade-offs and evidence still needed. Verify both sides of a contradiction stay visible, and a planning option does not change the assessment or imply an architecture decision.

## 6. Preserve runs, reviews and evidence changes

Source plans: [Core engine](plans/01-core-analysis-engine.md#persistence-failure-and-repeatability) · [UI and user journeys](plans/04-ui-and-user-journeys.md#show-change-after-adding-evidence)

- [ ] **Recoverable run storage and revision history are missing (RUN-1, REV-1).** Deliver immutable completed snapshots, input fingerprints, analyzer/rubric versions, progress and error states, review events and a recomputed reviewed projection. Verify cancellation and failure retain diagnostics, retries produce linked attempts, a stale review cannot overwrite another one, and the original machine record stays available.
- [ ] **Actual reassessment and a selected change view are missing (DELTA-1).** Deliver Add evidence, one real child run from the Harbor Stock scheduler addition, navigation between both runs, and a change view for changed requirements, findings, confidence checks and gates. Verify original results remain retrievable, the newly supported schedule is traceable, recovery remains open where evidence is missing, and changed methodology is labeled separately. An unchanged score is a valid outcome if the underlying evidence change is visible.

## 7. Deliver the UI, reports and reuse handoff

Source plans: [UI and user journeys](plans/04-ui-and-user-journeys.md) · [Reuse handoff](plans/06-implementation-sequence.md#reuse-handoff)

- [ ] **Working local UI is missing (UI-1, UI-2).** Deliver intake and progress, overview, inventory, searchable graph and table, original evidence view, findings and review, gap actions and export. Show selected run and revision consistently. Verify a reviewer can navigate from a blocking gate or impact result to original evidence by keyboard, and distinguish an empty filter from a failed analysis or unresolved reference.
- [ ] **Generated reports are missing (REP-1).** Deliver self-contained HTML and schema-versioned JSON from the same run and revision. Include scope and versions, inventory and coverage, the three results, dependency graph or map and table, evidence locators, blockers, hotspots, actions, review history and limitations. Default to omitting excerpts. Verify references resolve, HTML opens offline without the workbench running, and one finding can be reconstructed from JSON.
- [ ] **Operator and extension guidance is missing.** Deliver working setup and usage instructions, a reuse example for a new application, a manifest and typed evidence example, supported inputs and limits, review/rerun/export steps, and a recipe for adding an analyzer or rule. Include limitations for unsupported artifacts, incomplete evidence, assumptions, synthetic validation and human review. Verify a second operator can follow the guide on Cedar Permits and the extension uses the same evidence and report contract.

## 8. Run the tests and collect proof

Source plan: [Named acceptance cases](plans/05-validation-and-demo.md#named-acceptance-cases)

- [ ] **Executed acceptance results are missing (SHIP-1).** Deliver runnable checks and an evidence log for `IN-01`–`IN-05`, `AN-01`–`AN-08`, `EV-01`–`EV-02`, `GR-01`–`GR-03`, `AS-01`–`AS-04`, `RV-01`, `RP-01`–`RP-02` and `RE-01`–`RE-02`. Each entry records expected and actual outcome and a command or artifact path. Verify the supported golden cases have zero unresolved failures. Run `GR-04` and `FLOW-01` through `FLOW-07` at the same endpoint.
- [ ] **Measured accuracy and repeatability are missing.** Deliver independent held-out results with expected, found, extra and missed edges and locator correctness, grouped by artifact family. Record actual denominators, unresolved scope and differences between rule conformance and SME usefulness. Verify repeated runs on identical inputs and versions match after removing run-specific metadata.
- [ ] **Offline and clean-workspace proof is missing.** Deliver a run log for both original apps with outbound access blocked, covering engine, UI and report behavior and attempted outbound requests. Record actual stage timings and environment. Verify the normal flow, the reports and the reuse path without a remote model or assets.

## 9. Assemble the demo and prove the end-to-end endpoint

- [ ] **Judge-ready demo package is missing.** Deliver both original app bundles, Harbor Stock's enrichment, repeatable local commands, matching fallback reports, actual test results and a rehearsed walkthrough. Verify the demo can open one job-to-data dependency, its original evidence, an impact path, a hotspot and a gap action. Show the scheduler change and remaining recovery question, then assess Cedar Permits in the same tool. [Judge walkthrough](plans/05-validation-and-demo.md#judge-walkthrough)
- [ ] **The end-to-end acceptance record is missing.** Complete the walkthrough at the top of this file. Attach actual proof for the six challenge deliverables below and record any open required item. The overall result remains incomplete while any required check is open.

| Challenge deliverable | Proof to attach |
|---|---|
| Reusable tool with a defined input model | Local start command, versioned input example and completed run IDs for both applications |
| Multidimensional assessment | Domain outputs with actual code and non-code evidence, supported coverage and explicit unknowns |
| Three independent, explainable results | One exported example linking results to signals, evidence checks and gates |
| Structured report, dependency graph and next steps | HTML/JSON exports, traceable graph path and prioritized actions |
| Reuse guide | Instructions and the second operator's result or documented walkthrough |
| Limitations | Delivered coverage matrix, assumptions, missing evidence and human-review requirements |

## 10. Validate investigation views and record optional feature eligibility

Boundary and portfolio views require working evidence. For auxiliary outputs or a model brief, record the agreed scope and eligibility decision with proof. An included feature with unfinished work remains open at the same acceptance endpoint. [Delivery scope](plans/00-product-scope-and-roadmap.md#delivery-scope)

- [ ] **Boundary exploration (BOUND-1).** Deliver a seeded group from a job, transaction or reviewed business label, with internal and crossing links, shared writers, ownership gaps and unresolved references. Preserve the original candidate and edits. Verify `GR-04`.
- [ ] **Cross-application portfolio view.** Compare separately scoped application results and rule versions while preserving each app's evidence and limitations. Verify a user can open both underlying runs and interpret differences in scope.
- [ ] **Staged evidence preview and auxiliary exports.** If included, preview a real isolated child run with explicit keep/discard behavior. Keep the base intact. Validate any CSV or standalone graph output against the same revision and evidence contract.
- [ ] **Tachyon modernization brief (AI-1).** If the client, model and endpoint satisfy the licensing and offline boundary, deliver one selected-flow brief with evidence IDs, candidate behavior, open questions and human review. Compare approved model settings on reviewed examples. Verify invalid citations and incomplete context are surfaced, and generated output does not change the graph, scores or readiness gates.

## Files-only evidence to attach

- [ ] Record real picker and CLI submissions for loose files, folder and ZIP. Prove that Harbor and Cedar both publish revision-zero reports without a manifest, mapping, questionnaire or manual export. Attach the report pairs and the observed mandatory-input counts.
- [ ] Run the raw non-code positives and negatives, mixed-workload and same-name cases, and the interrupted publication/retry case. Attach actual results beside each expected outcome. [Files-only acceptance cases](plans/05-validation-and-demo.md#files-only-acceptance-cases)
- [ ] Demonstrate a documented design lesson from the commercial tools and one from open source using only first-party fixtures and code. Open the source evidence in the delivered tool. [Research-to-demo mapping](plans/07-files-to-results.md#what-existing-tools-teach-us-about-the-workflow)
