# Core analysis engine

## Execution boundaries

```text
Input package → inventory → artifact extractors → scoped reference resolver
             → evidence graph → rubric → report model → CLI / UI / exports
```

Keep intake, extraction, resolution, scoring and presentation separate. Extractors emit source declarations and reference occurrences. The resolver looks across files. The rubric consumes normalized facts, gaps and active review records. One report model feeds the UI and both export formats. All stages run on local inputs.

## Records and invariants

Use a versioned logical schema independent of persistence. Fields below are required unless marked optional. Machine snapshots are immutable after completion.

| Record | Fields and constraints |
|---|---|
| Application | Stable application key, display name |
| Run | Run ID, application key, optional parent run ID, scope, input fingerprint, tool/analyzer/rubric versions, state, counts, stage diagnostics |
| Artifact | Artifact ID, relative path, byte hash when readable, kind, optional library, encoding/source format, status, optional exclusion reason, diagnostics |
| Evidence | Evidence ID, artifact ID and hash, original inclusive line or row range, optional columns or record key, location precision, extractor/version |
| Component | Component ID, kind, qualified key, display label, definition evidence IDs, attributes |
| Reference | Reference ID, source component ID, target kind, raw operand, normalized key when known, reference type, source evidence, optional execution context |
| Relationship | Relationship ID, reference or derivation ID, context, source, resolved target or candidate list, resolution state, basis, evidence IDs |
| Evidence item | Scope key, category, supporting evidence IDs, statement, declared owner where available, conflicts |
| Metric | Metric ID, name, subject, value where measurable, unit, evaluation state, basis IDs or derivation, algorithm version |
| Diagnostic/gap | Code, severity or materiality, scope key, explanation, affected IDs, available evidence IDs, disposition |
| Finding | Rule ID/version, subject, observation, optional inference, assumptions, unknowns, dependencies on findings, evidence IDs |
| Assessment | Dimension, complexity band and estimate status, confidence profile, signal trace, gate outcomes and policy version |
| Recommendation | Stable action key, triggering gaps/findings, linked gates, owner role, expected effect, completion criterion, status |
| Review event | Event ID, run, subject, subject fingerprint, decision, reviewer, role, time, rationale, evidence IDs |

Use application, kind, declared library or namespace and logical name for qualified component keys. Qualify JCL steps by job and step name or ordinal, DDs by step and name, and logical files by program. Preserve raw names alongside normalized names. Store repeated reference occurrences separately. Deduplicate graph presentation by type, endpoints and execution context while retaining every evidence link. IDs in a report are scoped to one run. Generate comparison keys separately from qualified identity and occurrence evidence so line shifts and duplicate names can be handled explicitly. A stable ordering by path, location, kind and ID makes repeated results comparable.

Start the type registry with job, step, procedure, program, copybook, logical file, dataset, table, transaction, screen and external interface. Edges are directed from the acting or referring component to its object. Use `contains` for job → step, `executes` for step → program or procedure, `calls` for program → program, `includes` for program → copybook, `opens_transaction` only when the supplied evidence identifies that action, `entry_program` for transaction → program, `uses_screen` for program → screen, and `reads` / `writes` / `references` for data operations. Use an explicit derived `data_handoff` edge from writer step to reader step and a separately sourced `scheduled_precedence` edge for reviewed schedule evidence. Record logical and physical data references separately.

Persist relationships with orthogonal fields. **Basis** is observed source reference, derived static relationship or reviewer declaration. **Resolution** is resolved, missing in scope, ambiguous, dynamic, unsupported or declared external. **Review disposition** is a separate event. A resolved static relationship still describes the supplied artifacts. A runtime record may support an observation only for its stated scope and time window.

## Analyzer and resolver contracts

```text
detect(path, sample, overrides) -> kind, reason, certainty
analyze(artifact, source_map, settings) -> definitions, references,
                                          evidence_items, diagnostics
resolve(definitions, references, scope) -> relationships, gaps
evaluate(graph, inventory, evidence_items, review_state, rubric) -> report
```

An analyzer declares kind, supported syntax, input settings, diagnostic codes and version. It operates on one artifact. It consumes the source map for every location. It returns partial output when a supported statement can be inspected independently from an unsupported statement. If it fails without trusted output, keep a failed artifact and discard its uncommitted records. Cross-file rules belong in the resolver or rubric. Register a new analyzer by adding its classification rules, supported constructs and positive/negative fixtures while keeping the common record contracts.

Resolve in this order:

1. Index parsed definitions by application, kind, library and normalized name. Keep duplicate definition evidence. Mark collisions in the same priority group ambiguous.
2. For a literal operand, use the applicable manifest search order, then the eligible local definitions according to scope. Stop at the first group with candidates. A unique candidate resolves. Multiple candidates remain visible. No candidates means missing in supplied scope or declared external when the manifest explicitly identifies that target. A dynamic operand remains dynamic even if one plausible name exists.
3. For each resolved JCL step/program pair, join its program logical file assignment, matching DD name and concrete dataset declaration in that execution context. Record the evidence for all three and for access direction when available. Unavailable or conflicting bindings remain unresolved. Keep the binding scoped to the step so different jobs can bind the same program differently.
4. Join confirmed writer and reader references to a common resource to derive a **possible data handoff**. Keep reader-only sharing separate. Report schedule order only from separately supplied schedule evidence and flag disagreement.
5. Propagate parser limits as explicit gaps. For a partial artifact, assert only successfully supported occurrences. A missing definition in the export is a scoped observation, with a gap about deployment.

An evidence record points into the captured state of the input through artifact hash and original location. Use stored local snapshots or validate the bytes before showing excerpts from a reopened file. A normalized line offset must never be shown as an original offset.

## Persistence, failure and repeatability

States are `created → inventorying → analyzing → resolving → assessing → completed`, with terminal `failed` and `cancelled`. Partial artifacts can coexist with a completed run. Show their limits in results. A failed or cancelled run exposes stage diagnostics and any inventory already saved, with assessment/export controls disabled. A retry creates a new run linked to the attempt. Make model drafting a separate optional operation on a completed run.

Hash original bytes with SHA-256. Build the input fingerprint from the sorted relative paths with artifact hashes, normalized manifest and schema version. Record tool, parser and rubric versions separately. Compare deterministic payloads across reruns with identical versions and inputs, excluding run identity, timestamps and user review events.

Implement one local writer per workspace. Commit at stage boundaries. Publish a completed result only after validating internal references and writing its entire report payload. Persist reviewer events with optimistic revision checks so parallel views can detect stale updates. Recompute a reviewed projection from the latest valid events. A new source or config run starts with a fresh review state. Subsequent carry-forward, if added, requires matching subject fingerprint and explicit review.

For each proposed review event, require run ID, expected revision, subject ID and fingerprint, decision, reviewer role and rationale. Confirming, correcting or marking an item inapplicable requires evidence or an attested explanation within the reviewer’s scope. Validate that all attached IDs belong to the run. A correction adds a reviewed interpretation. A retraction supersedes the earlier event and restores the applicable prior projection. A stale revision returns the latest revision and leaves the event unapplied.

Evaluate the exact SQLite distribution against the dependency rule before using it. Use one store adapter, with a first-party snapshot-file format as the database-free design if the approved host supports atomic file replacement. Both options must preserve snapshots, event history, path isolation and consistent read behavior. The environment decision is recorded in the [dependency register](../learn/external-dependencies.md).

## CLI and UI boundary

These are logical operations. Exact invocation depends on the approved environment.

| Operation | Required input | Output and behavior |
|---|---|---|
| `inspect` | Source, optional manifest | Inventory preview and validation diagnostics, no assessment |
| `assess` | Source and effective manifest | Run ID, stage progress, final state or actionable diagnostic |
| `show` | Run ID | Summary, optional evidence/record lookup and review revision |
| `export` | Completed run ID, format, destination, excerpt policy | Report path, selected run/review revision, output error if applicable |

Use operation outcomes 0 for completed operation, 2 for invalid input, 3 for failed processing or export, and 4 for cancelled runs where the host supports exit codes. A completed assessment may have unknown results. The UI uses the same operations and records. Suggested initial local demo budget is one active run, with limits defined in [ingestion](02-ingestion-and-evidence.md).

## Optional model cues

Keep one action on a selected flow. Tachyon is conditional on the [dependency decision](../learn/external-dependencies.md). Prefer an available instruction-following code model with structured output. Try the smaller approved model with low reasoning for summaries and medium for cross-file rule or risk interpretation, where the endpoint supports those settings. Compare a stronger approved model on a tuning set before freezing one candidate for the holdout. For an initial trial, reserve up to 1,200 output tokens, use a selected flow and its relevant definitions, and set temperature to 0–0.2 only where supported. Some models expose reasoning and sampling controls differently. Record capability, model ID, prompt version, settings, supplied evidence and truncation. Capture one structured result with proposed rules, implications, unknowns and questions. Require live evidence IDs for factual premises and flag invalid or incomplete output. Analyst review evaluates interpretation. Model output has no score, graph-write or execution authority.

## Concrete result contracts

Use the following payload shapes to align the analyzer, persistence, rule and presentation work. The identifiers are illustrative. The complete report must include definitions for referenced IDs. A run prefix scopes lookups, and comparison keys identify records across runs when identity is unambiguous.

```json
{
  "artifact": {
    "id": "a:stock-jcl",
    "path": "jobs/STOCKRUN.jcl",
    "digest": "sha256:<64-hex-digits>",
    "kind": "jcl",
    "bytes": 148,
    "classification": {"method": "extension_and_signature", "status": "confirmed_for_dispatch"},
    "status": "analyzed",
    "diagnostic_ids": []
  },
  "evidence": {
    "id": "e:stock-exec",
    "artifact_id": "a:stock-jcl",
    "location": {"kind": "source_spans", "spans": [{"start_line": 2, "end_line": 2}]},
    "extractor": {"id": "jcl", "version": "0.1"}
  },
  "relationship": {
    "id": "r:stock-exec",
    "comparison_key": "executes|job:STOCKRUN/BUILD|program:APP/POSTSTK",
    "source_id": "c:STOCKRUN/BUILD",
    "target_id": "c:APP/POSTSTK",
    "target_candidates": [],
    "raw_target": "POSTSTK",
    "type": "executes",
    "basis": "observed_source",
    "resolution": "resolved",
    "evidence_ids": ["e:stock-exec", "e:poststk-id"]
  },
  "metric": {
    "id": "m:poststk-call-sites",
    "name": "literal_call_sites",
    "subject_id": "c:APP/POSTSTK",
    "unit": "count",
    "value": 1,
    "evaluation": "partial",
    "basis_ids": ["e:poststk-call"],
    "limit_codes": ["COBOL_SCOPE_PARTIAL"]
  }
}
```

Metric evaluation is `measured`, `partial`, `unknown` or `not_applicable`. For a ratio, include numerator and denominator alongside the value. Zero is a measured result only if the supported construct was evaluated across the stated scope and the numerator was zero. An empty ratio denominator has no value. A partial positive count is a lower bound and cannot support an absence claim. Metric provenance may reference reviewed evidence, a set of source spans or a typed graph derivation. Store the algorithm version and a compact query basis when a result aggregates too many IDs for the default view. The full supporting set remains retrievable.

Validate referential integrity at each stage boundary. A resolved relationship needs a target and valid evidence. An ambiguous reference retains candidates and no resolved target. A dynamic reference can carry evidence-backed hints and keeps its dynamic state. An unresolved placeholder is a display projection of a reference, never a discovered component. Preserve separate source spans for a multi-line statement and every inference input. The evidence view verifies the stored artifact digest and bounds before opening a span. A location that fails verification shows a stale or unavailable state and blocks any affected review criterion.

### Stable identity and storage layout

Build a comparison key from escaped, typed fields with a canonical field order. Job step identity uses job name and step name if unique, otherwise a source-scoped ordinal. Definitions in different libraries remain distinct. For relationships, match the qualified source, type, target operand or qualified target and execution context. Use a reference occurrence key for evidence repetition, with a canonicalized statement hash and ordinal within its owner. Treat ambiguous cross-run matches as added and removed with a warning. A line shift by itself does not change a component or resolved relationship identity. A resolution change can appear as one removed and one added relationship where pairing is ambiguous. An identified reference with a unique cross-run key may be shown as a changed resolution.

If the chosen host supports the first-party file store, use a workspace with per-run manifest, staged inventory, immutable completed result, content-addressed input snapshots and a separate review journal. Use safe opaque IDs for folders and never join an untrusted logical path into the store. Same-content storage reuse may share original bytes while preserving both logical artifact records. Commit a complete version before exposing the completion marker. On startup, ignore incomplete temporary records, keep the last valid review revision and label interrupted runs failed. Keep snapshot retention a documented local operator setting. The optional SQLite adapter uses equivalent entities indexed by run, component type and qualified key, reference resolution and evidence artifact ID, with compound uniqueness scoped to the run. Run status, immutable snapshot and review revision update transactionally.

## Graph indices and dependency algorithms

Build a directed multigraph with first-party adjacency maps. Retain parallel typed edges and occurrence evidence. Maintain both forward and reverse maps, an index of unresolved references by source, and a membership map from step to owning job. The graph view consumes the same canonical records as the rule engine. Use a per-query projection to select relevant edge types. Keep containment and scheduling separate from code and data propagation. A reference marked external can contribute to an interface count and remain a traversal boundary.

| Metric | Computation | Interpretation |
|---|---|---|
| Fan-in and fan-out | Distinct adjacent component IDs for the selected edge types and scope | Count coupling separately from repeated statements. Never infer business importance from degree alone. |
| Shared copybook | Distinct programs including a resolved copybook | Display example source occurrences and unsupported expansion limits. |
| Data use | Distinct reader and writer components or execution contexts per dataset/table | Direction requires evidence. Undirected usage and candidate targets are a separate count. |
| Structural cycle | Strongly connected groups of the selected resolved call or schedule subgraph | A group of size greater than one or an explicit self-loop indicates a static cycle. Schedule cycles are reviewed for scope and contradictory rules. Do not enumerate every cycle. |
| Resolution counts | Counts by type, disposition and relevant scope | Retain missing, ambiguous, dynamic, unsupported and external separately. Zero eligible references yields N/A for the ratio. |
| Reachability | Counts of unique nodes reached by one typed query, with graph coverage and cap | Evidence for potential impact, with a lower bound when truncated. |

### Change Impact Explorer

A query answers either “who may depend on this?” or “what does this use?” for one root in one completed run and review revision. For dependents, traverse the reverse of eligible dependency edges, such as program calls, copybook includes and data reads/writes. Use a separate mode to follow the direction of data movement through a writer-to-reader handoff. Job/transaction entry point attribution is a summary join to the traversed components or steps. It must not create new causal paths. Schedule precedence is visible as an independent filter.

```text
validate root, mode, relationship types, depth and revision
queue root at depth 0
while queue has work and within visit budget:
    take next node; iterate eligible edges in canonical order
    record unresolved references as caveats; never traverse guessed targets
    record confirmed eligible neighbor and a representative evidence path
    expand unseen neighbor at its minimum depth if within depth limit
return unique direct/indirect nodes, path edge IDs, entry-point summary,
       unresolved references, traversal limits and truncation flags
```

Choose deterministic shortest paths by ordered edge key. Retain distinct parallel edge types in the detail panel. The display labels whether an edge was traversed in its declared or reverse direction. Root self-loops and cycles can be shown as caveats without adding the root to impact counts. Start with depth 2 for explicit impact queries, allow 1–3, cap machine visits at 1,000 nodes and render a first view of 80. Expose separate `visit_limit_reached`, `depth_limit_reached` and `display_limited` flags. When visit-limited, counts describe only the visited set. Pages must retain the same query and revision. Impact examples include selecting STOCK.OUT and tracing both job steps, and selecting a shared copybook and tracing caller programs and their owning entries. Unsupported or disconnected nodes return a reason and an empty impact result.

### Optional boundary candidates

The user selects a job, transaction or reviewed business label as a seed. Expand across supported code edges in the selected direction to a default depth of two. Keep the proposed membership small, with an initial 30-component cap. Exclude external resources from owned membership and list shared copybooks, datasets and tables as dependencies to review. Never absorb both sides of an unresolved or ambiguous edge. Show internal edges, incoming/outgoing crossing edges by kind, shared readers/writers, unresolved references and data/operational ownership gaps. List the shortest supporting paths.

Compare an analyst-edited membership with the original candidate on the same graph and revision. A one-member group, overlapping candidates, a truncated candidate or an externally written shared store raises a caveat. Preserve both memberships and evidence and require a human decision for any planning use. Use inspectable raw counts and named conditions. Hub degree or internal edges alone do not support a service boundary. Candidate removal or review does not change the underlying graph.

## Application query boundary

Bind the UI and CLI to the same named local operations. If the approved host warrants a loopback HTTP adapter, these names map to the routes shown. The core layer does not require a service or particular framework.

| Operation | Optional local route | Request and result |
|---|---|---|
| Start or inspect | `POST /runs`, `POST /intake/inspect` | Local package handle and effective manifest. Return run ID and initial state, or inventory preview and validation issues. |
| Poll run | `GET /runs/{id}` | Stage, state, processed/total where known, diagnostics and review revision. |
| List records | `GET /runs/{id}/inventory`, `/findings`, `/gaps` | Stable filter, sort and page cursor with a revision. Total counts describe the filter. |
| Show evidence | `GET /runs/{id}/evidence/{evidence_id}` | Locator and verified source excerpt when available. Resolve from an opaque ID within the run. |
| Query graph | `GET /runs/{id}/graph`, `/impact` | Root, allowed modes, type filter, depth and revision. Return nodes, edges, summary, paths and limits. |
| Review | `POST /runs/{id}/reviews` | Subject, decision, evidence and expected revision. Return documented review and recomputed projection or conflict. |
| Compare | `GET /runs/{id}/compare?base=...` | Same-application snapshot IDs and selected revisions. Return typed changes, methodology flags and caveats. |
| Export | `GET /runs/{id}/report` or local CLI output | Format, revision and excerpt policy. Return generated artifact or explicit error. |

Reject unknown filter types or out-of-range depth, missing or cross-run record IDs, access before completion and stale revisions. Return a structured diagnostic containing stable code, stage, user-readable message and recoverability. A local adapter binds only to an approved local interface and never echoes arbitrary absolute paths or source into logs. No authentication or multi-user server behavior is implied.

A selected impact response uses the following shape. This example shows a root dataset with a direct dependent step. The edge ID can be dereferenced to the complete typed relationship and its source spans.

```json
{
  "query": {"run_id": "run-stock-base", "review_revision": 0, "root_id": "c:STOCK.OUT", "mode": "dependents", "types": ["reads"], "depth": 2},
  "counts": {"direct": 1, "indirect": 0, "entry_points": 1},
  "items": [
    {"component_id": "c:STOCKRPT/REPORT", "depth": 1, "entry_point_ids": ["c:STOCKRPT"],
     "path": [{"relationship_id": "r:stock-read", "traversal": "reverse", "evidence_ids": ["e:rpt-dd", "e:rpt-read"]}]}
  ],
  "caveat_ids": [],
  "limits": {"visited": 2, "visit_limit": 1000, "visit_limit_reached": false, "depth_limit_reached": false, "display_limited": false},
  "next_cursor": null
}
```

The example is filtered to the report's read path. Counts are unique IDs within the same query before display pagination. The immutable query fingerprint includes root, allowed types, direction, version and review revision. A cursor belongs to that fingerprint. An excerpt lookup is independent of list pagination. For a failure payload, include the field and current revision where applicable, and return a stable conflict code when a UI action must be repeated on a newer revision.

## Evidence-delta workflow

Create a child run when source, effective manifest or applicable evidence changes. Capture the base run ID and rule/analyzer versions. For the first milestone, add one evidence file through the regular intake path and rerun deterministically. A next increment compares machine snapshots and explicitly selected review revisions. Match by qualified component/reference keys, rule plus subject for findings, and category plus subject for gaps. Emit added, removed and changed rows for artifact digest, relationship resolution, finding, gate, confidence check and action. Show whether a changed score is attributable to input, review, rules or parser version. Mixed changes remain mixed. A changed methodology shows a comparison caveat and cancels any isolated evidence-effect claim.

Optional preview uses an isolated child run with an explicitly supplied overlay. Base files stay immutable, same-path replacements require explicit selection, and the UI can keep the preview as a child run or discard it. A cached parser result is safe only with a key that includes bytes, analyzer version, resolved parse settings and any input consulted during extraction. Keep caching after the deterministic baseline. A hypothetical arrival with no supporting bytes can list affected checks but cannot produce a new score.
