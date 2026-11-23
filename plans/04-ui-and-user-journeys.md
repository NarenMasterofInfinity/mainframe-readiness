# UI and user journeys

The local UI uses one data model shared with the CLI and report generator. The browser and execution environment are decisions in the [dependency register](../learn/external-dependencies.md). The design assumes first-party UI code and assets. If approved host capabilities allow a browser UI, use native controls and a first-party SVG diagram with a table view. Establish graph usability after the first evidence slice.

## Navigation and interaction

Keep application, run and review revision visible. Links preserve run and entity identity so a report reference opens the intended evidence. Switching runs resets entity selections and incompatible filters. Use a common path from any finding, edge or assessment driver to its underlying evidence.

| View | Default content | Primary interactions | Empty and failure states |
|---|---|---|---|
| Assessments | Local applications and runs sorted by latest activity | New assessment, resume viewing a run, duplicate scope to reassess | Direct first-time users to intake. Label cancelled, failed and incomplete runs. |
| Intake | Source picker, scope/workload form, expected categories, inventory preview | Override file type, encoding or library, review exclusions, start or cancel | Show exact item and correction for invalid values. Preserve draft scope on recoverable errors. |
| Overview | Readiness result and first blocking gate, distinct complexity and confidence panels, top actions | Expand dimension detail, follow source links, open gaps/actions | Use explicit Unknown, Provisional and Not applicable labels. Scope and rule versions remain visible. |
| Inventory | Counts by kind and status, expected evidence coverage | Search by path, filter by status/category, inspect diagnostics and original evidence | Separate no matches, missing evidence, unsupported files and parser failure. |
| Dependencies | Selected entry point and neighborhood, paired relationship table | Pick root, depth, direction and types. Select nodes or edges. Open source evidence. | Display disconnected roots, truncation and unresolved references. Offer complete relevant table paging. |
| Findings and review | Actionable findings and open decisions first | Filter by dimension/status. Confirm, reject or correct with evidence and rationale. | Report missing source excerpts and stale review revisions with recovery steps. |
| Recommendations | Ranked actions with owner, decision affected and completion criterion | Open affected finding, add evidence, record action disposition, start reassessment | Show explicit reason when an issue has no executable recommendation. |
| Export | Report contents, excerpt setting and selected revision | Export HTML or JSON and show path/result | Disable export on incomplete runs. Retry locally after destination or write failure. |

The happy path is intake → overview → evidence → action → export. Recovery paths include an invalid package, a partial parse, an ambiguous target and unmet readiness gates. Reassessment pre-fills the manifest and creates a linked new run. A dedicated visual comparison is follow-on work. Both runs can be opened and exported in the first release.

## Graph and evidence panel

Start at a reviewed critical entry point, or ask the user to choose one. Offer both directions at depth one by default and a depth limit of three. Use breadth-first traversal, visited keys for cycles, stable node order and a starting cap of 80 graph nodes. Count hidden nodes where known and indicate the applied cap. Give the table access to all matching relationships through paging. A selected node lists callers, outgoing references, findings and declaration sites. An edge shows basis, resolution, source and candidate targets, its execution context and every supporting span. Unresolved targets use visible labels and distinguish missing, dynamic and ambiguous states. The interface explains whether a path is a static reference, a derived handoff, or supported by observed runtime evidence.

The evidence panel displays relative path, artifact hash or short fingerprint, original location, excerpt where available and diagnostic status. Joined statements show every contributing span. Reviewer disputes appear beside the machine interpretation. Use plain wording with progressive disclosure. The table and evidence controls have visible focus and accessible labels. Color reinforces textual status. Keyboard users can follow the full graph-to-evidence journey from the table.

## Report and stable export contract

Generate a single self-contained HTML file and one versioned JSON file. Both exports use the same run and review revision. Include:

1. Application, scope, workload classification and target assumptions.
2. Three outputs, complexity signal trace, confidence check counts and readiness gates.
3. Prioritized findings, graph/table, relevant flow snapshots and supporting evidence locations.
4. Recommendations with rationale, expected effect, suggested role and closure criteria.
5. Coverage, unresolved references, limitations, versions and reviewer events.
6. Instructions for reusing the same workflow on another application.

JSON uses top-level objects or arrays for `report_schema_version`, `run`, `scope`, `inventory`, `components`, `relationships`, `evidence`, `metrics`, `findings`, `gaps`, `hotspots`, `assessment`, `recommendations`, `review_events` and `limitations`. Optional features add documented fields for comparisons, candidates and draft provenance. Use empty arrays for evaluated lists with no rows and explicit availability fields for unavailable features. Validate references before export. Escape supplied content in HTML and build graph labels as text. Use relative source paths. Exclude snippets by default and let the user select inclusion in local export. Test by opening HTML with outbound access blocked and by reconstructing one evidence-linked finding from JSON without the running app.

## Modernization brief, if available

Add the optional action on a selected flow. Show the selected evidence scope and any truncation, followed by candidate behavior, implications and questions. Reviewers can challenge claims using the same evidence panel. The main navigation and deterministic report remain usable when the model feature is unavailable. The [core engine](01-core-analysis-engine.md) specifies model settings and constraints.

## Screen specifications and interaction details

### Intake and progress

Arrange intake as source → scope → preview → run. Accept a local folder or ZIP through the approved entry point. If the browser cannot provide a safe local directory handle, let the local CLI run directory bundles and open the resulting run in the UI. Show package name and applied byte and file limits before starting. Keep the explicit application name, assessed perimeter and workload modes above advanced settings. Disclose synthesized manifest defaults in preview.

In the preview table, show file path, type, reason for classification, encoding/source format, library, size and status. Distinguish a duplicate-byte badge from duplicate declared identity. A correction updates the effective manifest and reruns preview. For ambiguous items, keep a pending or excluded state with the reason. Preserve validation messages by field or artifact. Start is available only when the package and manifest are admitted. Unsupported files can remain in a completed run. After start, show stage and counts when the denominator is known, otherwise an indeterminate indicator. Reopening the view restores progress by run ID. Cancellation has an explicit terminal state and a rerun action.

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

### Risk hotspots and Evidence Gap Planner

The hotspot list explains why each item appears at its position with gate state, materiality and the count or lower bound of affected critical entry points. Clicking the count opens the same typed impact query used by the engine. Filters cover dimension, gate, action status and evidence disposition. A risk awaiting evidence and an evidence-supported migration difficulty have distinct labels. Preview closure criteria before recording a decision.

Each gap card has a clear title, affected subject, evidence status, suggested role, source needed and completion condition. Example: “Confirm when STOCKRPT can read STOCK.OUT.” The card links to the handoff, explains that the schedule is absent and requests the applicable predecessor rule and success condition. The case expands to show that recovery is a separate requirement. “Add evidence” launches a new run using the current effective manifest and a preview of the input change. “Record review” requests reviewer, role, disposition, rationale and evidence. If a stale revision is returned, show the latest revision and require the reviewer to reapply their decision against it.

### Show change after adding evidence

The first release can open the completed child run and its base run from a shared run header. The should-have comparison presents changed checks, gates and findings in one view. Write a plain explanation for each change, such as “Schedule now documented. Recovery procedure still missing.” Use stable IDs to anchor both sides and show differing rule or analyzer versions. Offer Added, Removed and Changed tabs with an Unchanged toggle. A new risk belongs next to any resolved gap. Preserve the listing and labels if all bands happen to stay the same. If preview of an uncommitted child run is implemented, display its draft state and provide keep/discard actions.

### Optional boundary review

Open a candidate from a selected entry point. Show scope and included components first, then crossing-edge counts, shared reads/writes, pending links and conditions for evaluation. Selecting a crossing edge opens the actual dependency path. Compare initial and edited membership before recording a review. The candidate remains a proposed grouping even after a reviewer agrees it deserves investigation.

## Export sections and handoff quality

Use a readable first page with the three outputs, critical gates and top actions, then a detailed appendix. The graph snapshot or relationship map needs a legend and scope. The dependency and impact tables name the relation type, resolution, basis, origin and evidence links. Show supported constructs, unsupported situations and how the parser handles partial inputs. Include unresolved targets, assumptions, contradictory evidence and the proposed review or collection action. Include candidate migration considerations only with prerequisites and supporting evidence. The reviewer log identifies revisions and attached evidence. The optional model log records model/settings and selected evidence context when used.

Include query limits and scope in copied or exported impact details. Reports use stable internal anchors for evidence even with excerpts omitted. Print view must present headings, table labels and source links in readable order, with the requirement and version summary intact. Include a short reuse checklist and a limitations link in the report. CSV for action tracking and a standalone graph export are optional conveniences conditional on the baseline HTML and JSON releases.

Source inclusion applies across the report, including finding summaries, graph labels, review text and any draft context. Keep excerpts out of diagnostics and logs. The export preview should show which fields can contain source content and which file will be written. In an excerpt-free export retain relative path, line or row, run ID and evidence ID. Explain how another authorized reviewer can reopen the locator from the retained run. A view of the source can include masked spans where identified. The report exporter treats mask coverage as limited and leaves excerpts off by default.
