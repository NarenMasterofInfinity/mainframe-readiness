# UI and user journeys

The local UI uses one data model shared with the CLI and report generator. The browser and execution environment are decisions in the [dependency register](../learn/external-dependencies.md). The design assumes first-party UI code and assets. If approved host capabilities allow a browser UI, use native controls and a first-party SVG diagram with a table view. Establish graph usability after the first evidence slice.

## Navigation and interaction

Keep application, run and review revision visible. Links preserve run and entity identity so a report reference opens the intended evidence. Switching runs resets entity selections and incompatible filters. Use a common path from any finding, edge or assessment driver to its underlying evidence.

| View | Default content | Primary interactions | Empty and failure states |
|---|---|---|---|
| Assessments | Local applications and runs sorted by latest activity | New assessment, resume viewing a run, duplicate scope to reassess | Direct first-time users to intake. Label cancelled, failed and incomplete runs. |
| Intake | One local resource picker and progress | Select files/folder/ZIP to start, inspect progress or cancel | Show structural rejection with remedy. Continue through individual file gaps. |
| Overview | Readiness result and first blocking gate, distinct complexity and confidence panels, top actions | Expand dimension detail, follow source links, open gaps/actions | Use explicit Unknown, Provisional and Not applicable labels. Scope and rule versions remain visible. |
| Inventory | Counts by kind and status, expected evidence coverage | Search by path, filter by status/category, inspect diagnostics and original evidence | Separate no matches, missing evidence, unsupported files and parser failure. |
| Dependencies | Selected entry point and neighborhood, paired relationship table | Pick root, depth, direction and types. Select nodes or edges. Open source evidence. | Display disconnected roots, truncation and unresolved references. Offer complete relevant table paging. |
| Findings and review | Actionable findings and open decisions first | Filter by dimension/status. Confirm, reject or correct with evidence and rationale. | Report missing source excerpts and stale review revisions with recovery steps. |
| Recommendations | Ranked actions with owner, decision affected and completion criterion | Open affected finding, add evidence, record action disposition, start reassessment | Show explicit reason when an issue has no executable recommendation. |
| Export | Report contents, excerpt setting and selected revision | Open the generated HTML/JSON or save another copy | Disable export on incomplete runs. Retry locally after destination or write failure. |

A single selection leads directly to the overview and saved reports. Investigation, review and reassessment are available from the result. Recovery paths include an invalid package, a partial parse, an ambiguous target and unmet readiness gates. Reassessment pre-fills the manifest and creates a linked new run. Both runs can be opened and exported, and the comparison shows the effect of the added evidence.

## Graph and evidence panel

Start at a reviewed critical entry if available, otherwise the discovered job or transaction with the most resolved direct edges, breaking ties by qualified key. If no entry exists, show inventory and gaps. Offer both directions at depth one by default and a depth limit of three. Use breadth-first traversal, visited keys for cycles, stable node order and a starting cap of 80 graph nodes. Count hidden nodes where known and indicate the applied cap. Give the table access to all matching relationships through paging. A selected node lists callers, outgoing references, findings and declaration sites. An edge shows basis, resolution, source and candidate targets, its execution context and every supporting span. Unresolved targets use visible labels and distinguish missing, dynamic and ambiguous states. The interface explains whether a path is a static reference, a derived handoff, or supported by observed runtime evidence.

The evidence panel displays relative path, artifact hash or short fingerprint, original location, excerpt where available and diagnostic status. Joined statements show every contributing span. Reviewer disputes appear beside the machine interpretation. Use plain wording with progressive disclosure. The table and evidence controls have visible focus and accessible labels. Color reinforces textual status. Keyboard users can follow the full graph-to-evidence journey from the table.

## Report and stable export contract

Generate a single self-contained HTML file and one versioned JSON file. Both exports use the same run and review revision. Include:

1. Application, scope, workload classification and target assumptions.
2. Three outputs, complexity signal trace, confidence check counts and readiness gates.
3. Prioritized findings, graph/table, relevant flow snapshots and supporting evidence locations.
4. Recommendations with rationale, expected effect, suggested role and closure criteria.
5. Coverage, unresolved references, limitations, versions and reviewer events.
6. Instructions for reusing the same workflow on another application.

JSON uses top-level objects or arrays for `report_schema_version`, `run`, `scope`, `inventory`, `components`, `relationships`, `evidence`, `metrics`, `findings`, `gaps`, `hotspots`, `assessment`, `recommendations`, `review_events`, `limitations`, `entry_flows`, `fields`, `behavior_candidates` and `boundary_candidates`. Comparisons identify their base run and revisions. An eligible model brief includes draft provenance. Use empty arrays for evaluated lists with no rows and explicit availability fields for unavailable features. Validate references before export. Escape supplied content in HTML and build graph labels as text. Use relative source paths. Exclude snippets by default and let the user select inclusion in local export. Test by opening HTML with outbound access blocked and by reconstructing one evidence-linked finding from JSON without the running app.

## Modernization brief, if available

Add the optional action on a selected flow. Show the selected evidence scope and any truncation, followed by candidate behavior, implications and questions. Reviewers can challenge claims using the same evidence panel. The main navigation and deterministic report remain usable when the model feature is unavailable. The [core engine](01-core-analysis-engine.md) specifies model settings and constraints.

## Screen specifications and interaction details

### Intake and progress

The empty workspace offers **Choose files**, **Choose folder** and **Choose ZIP**, plus equivalent drag-and-drop. Committing a selection starts one run. Use an approved first-party local picker or bridge where the browser lacks folder access. The CLI accepts the same inputs. Announce stages and counts to assistive technologies and move focus to the overview when the report pair is ready.

Inventory is viewable during and after analysis. Show each file’s path, type, detection reason, parse status and diagnostics. An expert can open settings from a result, adjust encoding, library order or classification and reassess. Keep the base intact. A well-formed bundle with zero supported files opens the results screen with unknowns and collection actions. An unsafe or empty selection returns its precise rejection before publication. Reopening the workbench restores the active run or its terminal outcome.

### Overview and dimension drill-down

Use this order so a planning reviewer can read the page without opening another view:

```text
Application  /  Run and review revision       Open earlier run    Export
Scope summary, run status, rubric and analyzer versions
-----------------------------------------------------------------------
Complexity                 Confidence              Readiness
Band + provisional reason  Band + checks in scope   Planning decision
Open evidence and signals  Open coverage details   Open blocking gates
-----------------------------------------------------------------------
Critical planning questions | Risk hotspots | Evidence requests
-----------------------------------------------------------------------
Domain row  →  observed inputs / rules / unknowns / evidence / reviewer
```

Show one-sentence explanations drawn from the report model. Keep all three answers visible in compact form on detail pages. The complexity detail lists detected signals and the highest contributing band, including known versus unmeasured requirements. The data panel includes layout and database/store views. The confidence detail presents denominator, check outcome and evidence for each dimension, with separate counts for present files and usable evidence. The readiness view lists the applicable gate conditions, failed vs unknown prerequisites, affected subjects and actions. A reviewed inapplicable row shows who reviewed the exclusion and why.

### Impact, inventory and evidence

Use a fixed structure on desktop: controls above the graph/table, a graph or table in the middle, source evidence in a detail panel. On a narrow screen, default to the table and open evidence below the selection with a named back action. Group filters for relationship type, basis, resolution and direction. Render focus and selected state with labels as well as color. Give graph/table rows a stable query order. A reset restores the current default root and filters. Never hide known unresolved items by default.

Selecting a node exposes inventory location, definition evidence, scoped uses and “Investigate impact.” In the impact view, offer Dependencies, Dependents and Data handoff as named modes. Show direct and indirect counts, path length and affected entry points, with an expandable evidence chain. Distinguish a graph display cap from the engine's exploration cap. An input filter can yield zero rows even with a nonempty graph. A disconnected component in the supplied export is listed as having no discovered neighbors. Avoid labeling it unused in production.

Selecting evidence opens exact original spans, which may include multiple lines or separate spans. Use readable code display with horizontal scroll or a wrap toggle, line numbering and text highlighting. Obtain excerpts through the run's evidence ID. If the stored digest disagrees or source bytes are unavailable, retain the locator and state. Display contradictory documents side by side with their separate scope and date. Keep model candidates, reviewer resolution and machine basis visible in the same finding detail.

### Behavior and field detail

From the overview, list supported behavior candidates as condition → local effect → original statements. For Cedar, an original IF/MOVE branch can explain when a status field is set. Attach the question “What does this status mean to the business?” only when its meaning lacks support. From a layout finding, open the field dictionary filtered to its copybook and field path. Show the declaration, known includers and any ambiguous field binding. Include both lists in the report appendix and JSON as `behavior_candidates` and `fields`.

### Risk hotspots and Evidence Gap Planner

The hotspot list explains why each item appears at its position with gate state, materiality and the count or lower bound of affected critical entry points. Clicking the count opens the same typed impact query used by the engine. Filters cover dimension, gate, action status and evidence disposition. A risk awaiting evidence and an evidence-supported migration difficulty have distinct labels. Preview closure criteria before recording a decision.

Each gap card has a clear title, affected subject, evidence status, suggested role, source needed and completion condition. Example: “Confirm when STOCKRPT can read STOCK.OUT.” The card links to the handoff, explains that the schedule is absent and requests the applicable predecessor rule and success condition. The case expands to show that recovery is a separate requirement. “Add evidence” starts a linked run when a new resource is chosen. Show the input delta as progress and in the completed comparison. “Record review” requests reviewer, role, disposition, rationale and evidence. If a stale revision is returned, show the latest revision and require the reviewer to reapply their decision against it.

### Show change after adding evidence

Open the completed child run and its base run from a shared run header. The comparison presents changed checks, gates and findings in one view. Write a plain explanation for each change, such as “Schedule now documented. Recovery procedure still missing.” Use stable IDs to anchor both sides and show differing rule or analyzer versions. Offer Added, Removed and Changed views with an Unchanged toggle. A new risk belongs next to any resolved gap. Preserve the listing and labels if all bands happen to stay the same. If preview of an uncommitted child run is implemented, display its draft state and provide keep/discard actions.

### Boundary review and portfolio

Open a candidate from a selected entry point. Show scope and included components first, then crossing-edge counts, shared reads/writes, pending links and conditions for evaluation. Selecting a crossing edge opens the actual dependency path. Compare initial and edited membership before recording a review. The candidate remains a proposed grouping even after a reviewer agrees it deserves investigation.

## Export sections and handoff quality

Use a readable first page with the three outputs, critical gates and top actions, then a detailed appendix. The graph snapshot or relationship map needs a legend and scope. The dependency and impact tables name the relation type, resolution, basis, origin and evidence links. Show supported constructs, unsupported situations and how the parser handles partial inputs. Include unresolved targets, assumptions, contradictory evidence and the proposed review or collection action. Include candidate migration considerations only with prerequisites and supporting evidence. The reviewer log identifies revisions and attached evidence. The optional model log records model/settings and selected evidence context when used.

Include query limits and scope in copied or exported impact details. Reports use stable internal anchors for evidence even with excerpts omitted. Print view must present headings, table labels and source links in readable order, with the requirement and version summary intact. Include a short reuse checklist and a limitations link in the report. The required handoff contains HTML and JSON. The JSON includes actions and graph relationships for local reuse.

Source inclusion applies across the report, including finding summaries, graph labels, review text and any draft context. Keep excerpts out of diagnostics and logs. The export preview should show which fields can contain source content and which file will be written. In an excerpt-free export retain relative path, line or row, run ID and evidence ID. Explain how another authorized reviewer can reopen the locator from the retained run. A view of the source can include masked spans where identified. The report exporter treats mask coverage as limited and leaves excerpts off by default.

## Default landing and handoff

The result header offers **Open HTML**, **Open JSON**, **Add evidence** and **Assess another application**. The files already exist at the shown local path. Put the strongest supported driver and first unresolved planning decision directly below the three ratings. A flow row reads, for example, “BUILD can write STOCK.OUT. REPORT can read it.” Its evidence action should open the original spans with one selection. Label statements derived from multiple artifacts. Keep a return path to the originating result. Show a neutral inventory landing when there is no entry point.

The local application list doubles as the portfolio comparison. One row per application shows its selected run/revision, evidence perimeter, workload clues, three separate results and rubric version. Drill down to the original result in one action. Mark different methods, incomplete scope and unknown dimensions. A side-by-side comparison describes each application’s evidence; it does not rank migration cost. Expert settings, optional review and any eligible brief live inside the result. None interrupts a first assessment.
