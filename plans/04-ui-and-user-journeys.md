# UI and user journeys

The UI framework is undecided: Streamlit, React, Cytoscape.js and their dependencies are not approved under the current exclusion of third-party licensed items. The analysis engine and UI must remain independent. Design the screens first; implementation requires a compliant runtime and display technology.

## Screens

1. **Assessments and intake:** create/open a run, upload ZIP, set scope/encoding, declare expected artifacts and see processing errors.
2. **Overview:** show separate complexity, confidence and readiness with “why,” coverage, critical gaps and prioritized actions. Use words and icons as well as color.
3. **Components and dependencies:** searchable table plus filterable graph. Select a node/edge for source, relationship status, unresolved targets and related findings. Start with local neighborhoods.
4. **Flows and data:** entry point → jobs/programs → reads/writes; distinguish observed statements from inferred bindings and runtime facts. A program-to-physical-dataset link may require both a logical file reference and its JCL DD binding.
5. **Findings and review:** evidence excerpts, inference, unknowns, action, SME status and notes. Provide a focused optional “create modernization brief” action.
6. **Report and export:** inspect limitations and generate self-contained HTML plus JSON.

## Primary journey

Upload → review coverage → inspect three assessments → open a finding → follow evidence and graph → review recommendation → export. Re-run on a second application with the same workflow. Retain run snapshots; if the stretch comparison is implemented, compare runs explicitly and label stale analysis.

Support keyboard navigation, clear labels, readable excerpts and a tabular graph alternative. Never imply a missing artifact was analyzed. Bundle assets and disable documented telemetry; verify the complete application under blocked outbound networking, including browser requests. Tachyon use is optional and endpoint location must be explicit.

## Detailed interaction states

**Intake:** empty state explains accepted export types and scope; show archive size, file count, type guesses and encoding. The user can correct classifications, declare expected categories and review exclusions. Reject unsafe archives with an actionable error and no partial assessment labeled complete. Analyzing shows run stage and per-file counts; cancellation preserves a visibly incomplete run.

**Overview:** show separate labeled results, rule version, scope and last run time. Each card links to its driving findings, missing prerequisites and relevant gate. Show “insufficient evidence” independently of complexity. A change of source, settings or rules requires a fresh run; do not mix runs.

**Graph:** filter by node/edge type, observed versus inferred and review state. Provide search, depth limit and accessible table. Selecting an edge opens all original evidence, resolution method and uncertainty. Missing or ambiguous targets appear distinctly; no edge implies runtime execution. Large graphs start with a component neighborhood and announce truncation.

**Finding detail:** show observation, evidence snippets and original path/line, inference, competing evidence, assumptions, unknowns, rule and review history. A reviewer can submit confirm/reject/correct with name and rationale. The tool preserves machine and human positions separately. No source is edited.

**Recommendations:** display why, evidence, owner, completion criterion, effect and status; clicking an action opens the corresponding artifact or question. When a reviewer closes it, request supporting evidence; recomputation, not closure alone, determines gate outcome.

**Export:** preview scope, versions, limitations, unresolved critical gates and reviewer notes. HTML must work locally without external assets; JSON must include stable identifiers and evidence references. User chooses destination and is told if source snippets are included. Export failures do not lose the run.

## Accessibility and usability checks

Exercise keyboard navigation and focus order, readable contrast, text labels in addition to color, empty/error/loading states and a graph-free route to every relationship. Use plain language with expandable technical detail. Confirm a new analyst can complete an assessment without reading implementation documents. No telemetry or background downloads are permitted under the stated runtime requirement.
