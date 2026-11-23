# Ingestion and evidence

## Input contract

Accept a local directory through CLI and ZIP through UI. Associate each run with application name, scope, optional owner, declared artifact types, file-extension overrides, encoding and expected-but-missing categories. Support exported text artifacts; direct mainframe connectivity is out of scope. Never require a cloud connection for the deterministic pipeline.

## Pipeline

1. Validate archive paths and limits; reject traversal, unsafe links and oversized inputs with diagnostics.
2. Inventory every file with relative path and hash; identify type by configurable path, extension and content clues. Allow user correction and preserve unknown files.
3. Decode with an explicit selected encoding; record errors and original bytes. Respect fixed-format columns, comments, continuations and member names where supported; do not silently guess a successful parse.
4. Analyze supported types; retain parsed, partial, failed, unsupported and excluded status separately. Surface parser diagnostics and line locations.
5. Resolve program/copybook/dataset references using scoped identities and configured search paths. Record missing, ambiguous and dynamic targets rather than guessing.
6. Store evidence against original file locations, include analyzer and rule versions, and make snippets navigable. Manual declarations and SME answers carry provenance and reviewer/date.

## Evidence semantics

A source reference proves only that a statement exists in the supplied artifact; it does not prove execution or business intent. Label observed, inferred, assumed, unknown and manually confirmed information. Conflicting evidence remains visible. Reference existence does not prove semantic correctness.

## Coverage

Show supplied files, parser success, unsupported inputs and unresolved targets. A percentage of supplied files parsed is not a percentage of the full estate. Expected security, operations, runtime, scheduler and ownership evidence is tracked separately, including not supplied and not applicable with rationale.

## Manifest and user controls

Example conceptual manifest: `application_id`, `display_name`, `scope_description`, `source_roots`, `artifact_type_overrides`, `encoding_by_path`, `copybook_search_paths`, `expected_categories`, `known_exclusions`, `review_contacts` and `target_assumptions`. Paths are relative to the selected input root. Validate duplicate identifiers and invalid paths before analysis. Do not require a manifest for first use: collect defaults interactively, display detected values, and allow correction before starting.

ZIP handling must reject absolute paths, parent traversal, symlinks and duplicate output paths; limit entry count, individual size and total expanded bytes, report limits and avoid writing over existing files. Treat archives as input data, never execute their contents. Detect binary and undecodable files and preserve them as unsupported/failed inventory entries; do not display sensitive content in diagnostic logs by default. Do not silently replace undecodable characters for analysis.

## Minimum detection matrix

| Artifact | Initial target | Explicit gap if unavailable |
|---|---|---|
| COBOL/copybook | Program IDs, COPY, literal CALL, selected file and SQL/CICS references | Unsupported dialects, multiline constructs, dynamic calls or failed sections |
| JCL | JOB, EXEC, DD and named procedures within the supported subset | Symbolic resolution, procedure expansion or control statements not understood |
| DDL/data definitions | Selected table and field definitions | Unknown syntax or missing schema |
| CICS/BMS | Selected source statements and provided definition references | No claim of complete transaction or screen reconstruction |
| Security/operations/scheduler | Inventory, manual evidence and review checklists | No claim of effective permissions, schedules, recovery or runtime usage |

Every analyzer publishes the exact supported subset in the product. On encountering unsupported syntax, emit a diagnostic and partial status when extraction may be incomplete. Examples must be independently created and exercised before promoting a type to supported.
