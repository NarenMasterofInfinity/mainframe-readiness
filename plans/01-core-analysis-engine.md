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
| Artifact | Artifact ID, relative path, byte hash, kind, optional library, encoding/source format, status, optional exclusion reason, diagnostics |
| Evidence | Evidence ID, artifact ID and hash, original inclusive line or row range, optional columns or record key, location precision, extractor/version |
| Component | Component ID, kind, qualified key, display label, definition evidence IDs, attributes |
| Reference | Reference ID, source component ID, target kind, raw operand, normalized key when known, reference type, source evidence, optional execution context |
| Relationship | Relationship ID, reference or derivation ID, context, source, resolved target or candidate list, resolution state, basis, evidence IDs |
| Evidence item | Scope key, category, supporting evidence IDs, statement, declared owner where available, conflicts |
| Diagnostic/gap | Code, severity or materiality, scope key, explanation, affected IDs, available evidence IDs, disposition |
| Finding | Rule ID/version, subject, observation, optional inference, assumptions, unknowns, dependencies on findings, evidence IDs |
| Assessment | Dimension, complexity band and estimate status, confidence profile, signal trace, gate outcomes and policy version |
| Recommendation | Stable action key, triggering gaps/findings, linked gates, owner role, expected effect, completion criterion, status |
| Review event | Event ID, run, subject, subject fingerprint, decision, reviewer, role, time, rationale, evidence IDs |

Use application, kind, declared library or namespace and logical name for qualified component keys. Qualify JCL steps by job and step name or ordinal, DDs by step and name, and logical files by program. Preserve raw names alongside normalized names. Store repeated reference occurrences separately. Deduplicate graph presentation by type, endpoints and execution context while retaining every evidence link. IDs in a report are scoped to one run. Generate comparison keys separately from qualified identity and occurrence evidence so line shifts and duplicate names can be handled explicitly. A stable ordering by path, location, kind and ID makes repeated results comparable.

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

An evidence record points into the photographed state of the input through artifact hash and original location. Use stored local snapshots or validate the bytes before showing excerpts from a reopened file. A normalized line offset must never be shown as an original offset.

## Persistence, failure and repeatability

States are `created → inventorying → analyzing → resolving → assessing → completed`, with terminal `failed` and `cancelled`. Partial artifacts can coexist with a completed run. Show their limits in results. A failed or cancelled run exposes stage diagnostics and any inventory already saved, with assessment/export controls disabled. A retry creates a new run linked to the attempt. Make model drafting a separate optional operation on a completed run.

Hash original bytes with SHA-256. Build the input fingerprint from the sorted relative paths with artifact hashes, normalized manifest and schema version. Record tool, parser and rubric versions separately. Compare deterministic payloads across reruns with identical versions and inputs, excluding run identity, timestamps and user review events.

Implement one local writer per workspace. Commit at stage boundaries. Publish a completed result only after validating internal references and writing its entire report payload. Persist reviewer events with optimistic revision checks so parallel views can detect stale updates. Recompute a reviewed projection from the latest valid events. A new source or config run starts with a fresh review state. Subsequent carry-forward, if added, requires matching subject fingerprint and explicit review.

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
