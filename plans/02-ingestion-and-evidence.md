# Ingestion and evidence

## Version 1 intake contract

Accept a single selection of loose local files, a directory or one ZIP. Generate the effective manifest internally. A normal assessment uses zero configuration files. The following example shows optional expert settings for a corrected run.

```json
{
  "schema_version": 1,
  "application_id": "harbor-stock",
  "display_name": "Harbor Stock",
  "scope": "Inventory posting and its overnight handoff",
  "workload_modes": ["batch"],
  "source_roots": ["src", "copy", "jobs", "evidence"],
  "path_overrides": {
    "src/POSTSTK": {"kind": "cobol", "format": "fixed", "library": "APP"}
  },
  "library_search_order": {"program": ["APP"], "copybook": ["APP", "COMMON"]},
  "expected_categories": {
    "scheduler": {"state": "required"},
    "transactions_ui": {"state": "not_applicable", "reason": "Batch-only declared scope"}
  },
  "critical_entry_points": [{"kind": "job", "name": "STOCKRUN"}],
  "declared_external": [],
  "target_assumptions": ["Distributed container platform"]
}
```

The runner fills schema version, workspace application key, display label and the perimeter “supplied files” before extraction. Business ownership and business perimeter start unreviewed. Default roots to the input root, encoding to strict UTF-8, workload mode to unknown, and unset category declarations to unclassified. Reject unknown manifest fields and invalid enums with field paths in messages. Apply overrides by exact relative path in v1. Require a reason for exclusions and not-applicable declarations. Record these as analyst declarations pending relevant review. Discovered evidence conflicting with a declaration opens a scope gap. Treat all entry points as potentially critical until scope review confirms their criticality.

Use `batch` and `online` as workload mode values. Use `code`, `data`, `batch`, `transactions_ui`, `integrations`, `scheduler`, `security` and `operations` as coverage categories. `expected_categories` accepts `required`, `optional` and `not_applicable`. Missing declarations remain unclassified. An optional category stays visible when supplied evidence or a rubric rule makes it relevant. Base required categories on reviewed workload and discovered entry points. The manifest cannot remove a discovered dependency from a gate without a documented scope resolution. `source_roots` limits analysis. Inventory files outside those roots as excluded with the reason.

## Intake sequence

1. Enumerate logical paths before parsing. Preserve display paths and normalize separators for identity. Reject unsafe paths, links, collisions under the selected host path rules, duplicate archive entries and encrypted packages. Inventory nested archives without opening them. Use streamed byte limits. Read from an isolated workspace or directly from validated entries.
2. Set starting demo limits of 2,000 members, 10 MiB per member and 100 MiB total expanded input. Make limits explicit in configuration and the UI. A structural archive failure stops the run. A readable but unclassifiable file remains an inventory record. An empty package returns an actionable validation outcome.
3. Hash original bytes and capture provenance. Attempt only the configured encoding. Handle UTF-8 BOM and line ending normalization with a source map. For other encodings, enable named decoders only after fixture validation on the approved host. Record an action requesting a supported text export when encoding is unsupported. Keep raw packed records and binary objects as unsupported inventory.
4. Classify by exact override, then the hint and bounded signature decision table below. If signals conflict at the same priority, record a diagnostic and continue with other files. Default extension rules are configuration, with original filenames retained. Assign library from configuration and leave unknown libraries explicit.
5. Dispatch supported files. Record `analyzed`, `partial`, `failed`, `unsupported` or `excluded` with codes and reasons. `analyzed` describes the published subset. Preserve per-construct blind spots separately so reports can distinguish coverage from inventory.
6. Store locations against original bytes and artifact hash. Use inclusive 1-based lines, columns where trustworthy and row/key locations for structured evidence. For joined or continued statements, retain the spans of all contributing physical lines. Preserve both declaration and use evidence on a derived link.

## Extractor coverage and implementation order

| Family | Implement in v1 | Output or explicit limit |
|---|---|---|
| COBOL | Configured fixed or free layout. Tokenize comments, literals and tested continuation forms before recognizing PROGRAM-ID, COPY, literal and identifier CALL, SELECT/ASSIGN, supported file operations, and selected delimited SQL/CICS commands | Program definition, references, logical file and access observations. Unknown layout, unsupported continuation or COPY transformations emit diagnostics. General control flow and dynamic dataflow remain gaps. |
| Copybook | Member identity, selected level/name/PIC declarations and observed OCCURS, REDEFINES or packed-decimal indicators | Copybook and layout flags with lines. Report unsupported computed offsets, variable layouts and unexpanded substitutions. Dependency indexing keeps the original copybook location. |
| JCL | Join tested continuation forms. Read JOB, explicit EXEC PGM, supported PROC references, DD with literal DSN, and explicit declarations of step and DD names | Job/step containment, program/procedure and dataset references. Symbolic DSNs, DD overrides, concatenation, in-stream data and conditional execution emit targeted limits wherever unimplemented. PROC reference discovery has separate status from expansion. |
| Data and SQL | Named CREATE TABLE and straightforward embedded SELECT/INSERT/UPDATE/DELETE statements with an unambiguous table target | Tables and explicit reads/writes. Mark dynamic, unqualified ambiguous, complex multi-table or unknown constructs partial where unsupported. Job dataset associations use the resolver. |
| CICS/BMS | Literal CICS LINK/XCTL program, named file or map operands, and selected explicit BMS map definitions. Read literal transaction/program bindings in the supported CSD definition form below or optional typed evidence. | References and defined maps. Identifier operands stay dynamic. Transaction entry mapping and static calls remain tied to evidence, with effective behavior requiring review. |
| Integrations | Read a supported interface register directly, or accept a supplementary declaration linking an entry point or component to an external system, file exchange or queue. Recognize additional source signatures only after fixture tests. | Interface, direction, declared contract and owner. Unsupported product commands and missing environment bindings become gaps. |
| Scheduler, security and operations | Read supported schedule CSV and access command exports directly. Accept supplementary typed rows and index plain-text evidence leads. Provide explicit claims for schedule, restart/reconciliation, access/audit and ownership | Evidence-backed declarations and linked affected scope. Arbitrary scheduler or security exports can be inventoried and attached for review when parsing is unsupported. |

A lexer tracks quotes, comments and physical source spans. Statement recognizers consume only supported forms. Add a known-unsupported marker when a relevant statement cannot be interpreted safely. If unsupported source may go undetected, list that limitation in the coverage matrix. Source reports retain private local excerpts by default. See [report export policy](04-ui-and-user-journeys.md) for inclusion controls.

## Typed evidence for non-code dimensions

For optional expert annotations use a JSON array in version 1. Each row has `category`, `scope_key`, `subject`, `predicate`, `value` and `owner_role`. Optional provenance fields are `source_path`, `source_location` with inclusive start/end lines, and `observation_window`. Require the path and location together when supplied. Validate the value against its predicate.

| Category | Predicate | Value |
|---|---|---|
| Code | `behavior` | Entry point key, stated condition and outcome, plus source reference where available |
| Data | `ownership` | Owner role and store key |
| Transactions/UI | `entry_mapping` | Transaction key, program key and optional screen/map key |
| Integrations | `interface_contract` | External system key, direction (inbound, outbound or bidirectional), contract summary and owner role |
| Scheduler | `precedes` | Predecessor job key, successor job key and documented condition (for example success) |
| Operations | `recovery` | Affected entry point, restart point, reconciliation procedure and responsible role |
| Operations | `monitoring` | Affected entry point, monitored outcome and escalation role |
| Security | `access_audit` | Resource key, identity or role class, permitted operation and audit requirement |

An explicitly supplied annotation is an analyst declaration. Rows extracted from an original export carry the adapter identity and original row locator. Attach a supporting source span where available and store it separately. An out-of-root or malformed pointer is invalid. A missing in-root document, unresolved subject or missing corroboration produces a gap. Conflicting rows stay visible. Only a named review can make them satisfy a gate. Retain unknown predicates in row diagnostics. Do not reinterpret them as known categories.

Coverage shows raw inventory counts, analyzed/partial/failed/unsupported/excluded counts, expected categories with usable evidence, missing categories and out-of-scope declarations. A required category with a supplied but unreadable document is present in inventory and missing usable evidence. Show both dimensions.

## Bundle layout and input examples

This example includes optional expert material. The classifier accepts arbitrary directory names. Extensionless members are eligible.

```text
harbor-stock/
  manifest.json                (optional expert settings)
  src/POSTSTK.cbl
  src/RPTSTK.cbl
  copy/STOCKREC.cpy
  jobs/STOCKRUN.jcl
  jobs/STOCKRPT.jcl
  evidence/observations.json
  evidence/runbook.txt
```

In the files-only flow a resource called `manifest.json` is inventoried as data. Apply settings only when explicitly chosen in advanced reassessment. Validate that file independently and report field errors there. Ignore no bundle files silently. An explicitly selected manifest outside the source root is configuration with a digest, not an artifact to be discovered as a program. For an archive with one enclosing directory strip that wrapper once and retain the original path in provenance. With multiple top-level folders retain their relative paths under one virtual root. Each evidence row refers to logical paths beneath the chosen root.

Example optional expert annotation of an unparsed scheduler document follows. The demonstration consumes the scheduler CSV directly.

```json
[
  {
    "category": "scheduler",
    "scope_key": "nightly-stock",
    "subject": "job:STOCKRPT",
    "predicate": "precedes",
    "value": {
      "predecessor_job_key": "job:STOCKRUN",
      "successor_job_key": "job:STOCKRPT",
      "condition": "success"
    },
    "owner_role": "batch operations",
    "source_path": "evidence/stock-schedule.txt",
    "source_location": {"start_line": 1, "end_line": 1}
  }
]
```

The row states a schedule claim. It links to a supplied schedule line. The reviewer checks the two job identities, condition and applicable environment before accepting it for readiness. An unresolved or conflicting schedule is preserved for investigation. A supported export yields a source observation automatically. A manual annotation retains its own origin and locator.

## Admission and diagnostics

Use a two-pass intake. First enumerate names and metadata, reject structural problems and check declared sizes. Second stream accepted members, enforce expanded limits against actual bytes, hash them and publish the staged inventory atomically. The compressed ZIP limit is 25 MiB for the demo profile. Use a declared expansion ratio limit of 100:1 as an early guard when metadata supplies a usable denominator. Always enforce actual byte limits. An implausible or inconsistent length is a structural failure. Resource defaults are tunable local settings, with applied settings saved per run.

| Condition | Diagnostic and decision | Analyst action |
|---|---|---|
| Empty folder or ZIP | `INPUT_EMPTY`, reject intake | Select an application export. |
| Absolute, traversing, conflicting or reserved path | `ZIP_PATH_INVALID` or `PATH_COLLISION`, reject intake | Re-export with safe unique relative names. |
| Symlink, special file, encrypted member, malformed or truncated ZIP | `INPUT_ENTRY_UNSUPPORTED` or `ZIP_INVALID`, reject structurally invalid input | Create a regular unencrypted export. |
| Limit exceeded | `INPUT_LIMIT`, reject without publishing an assessment | Reduce the scoped export or adjust a documented local setting. |
| Nested archive | `NESTED_ARCHIVE`, inventory as unsupported. Expand zero levels. | Supply its contents separately if in scope. |
| Empty or unreadable regular file | `FILE_EMPTY` or `FILE_UNREADABLE`, failed artifact when a supported type was expected | Obtain an intact file. |
| Copy of identical bytes at another path | `DUPLICATE_CONTENT`, retain paths and candidate scopes | Confirm library/alias mapping. Byte equality alone does not identify deployment copies. |
| Different bytes with same declared identity | `DEFINITION_COLLISION`, retain all candidates | Specify search scope or submit supported resolution. |
| Mixed, unsupported or invalid encoding | `ENCODING_UNSUPPORTED` or `DECODE_FAILED`, no lossy analysis | Provide per-path setting or a verified text export. |
| All files unsupported or excluded | `NO_SUPPORTED_ARTIFACTS`, complete inventory with coverage finding if input is structurally valid. Scores and gates follow the unknown policy. | Provide supported source and structured evidence. |

For directory intake, traverse only admitted regular files and verify the path stays within the selected root. Apply equivalent member and byte limits. Do not inspect local directories outside the selected root. On cancellation or intake failure, remove temporary staged bytes where safe and retain the failure record. The assessment never executes artifacts or treats text inside them as commands.

### Classification decision table

Probe the first 200 physical lines or 64 KiB of decoded text, whichever comes first. An exact supported override wins and is marked analyst-supplied. Without it, use extension or directory hint plus a narrow content signature. Agreement dispatches automatically. A decisive contrary signature against a known hint creates an ambiguous classification with a diagnostic in the completed result. A decisive signature with no known extension can dispatch. An unsupported but identifiable type remains recognized-unsupported. Treat weak/unknown signals as unrecognized, recording the reason and candidates instead of assigning a numeric classification confidence.

| Kind | Strong starter clues | Conservative edge case |
|---|---|---|
| COBOL | `PROGRAM-ID` in source context plus division marker or configured COBOL member | An ordinary text passage mentioning `PROGRAM-ID` is insufficient. |
| Copybook | Known copybook path/extension with source-level data declarations and no program definition | Fragmentary members can use a manifest override. |
| JCL or PROC | Supported control records with `JOB`, `PROC`, `EXEC` or `DD` in operation position | A document quoting these records stays a document unless intentionally classified. |
| SQL or BMS | Recognized statement or macro in source position with compatible hints | A textual reference to a table or screen remains documentation evidence. |
| CSD or access commands | Dedicated listing with supported literal control statements in command position | Prose, quoted examples, conflicting file clues or surrounding unsupported commands limit what is extracted. Retain a review lead if ambiguous. |
| Schedule or interface CSV | An unambiguous matching header and validated row shape, independent of filename | Unknown headers stay visible in inventory. A malformed matching row keeps a diagnostic and original span. |
| Typed evidence | Exact configured file or supported directory convention and valid version-1 row shape | Generic JSON configuration is inventoried separately. |
| Unsupported | Explicit extension/signature for binary data, another language or an unsupported document format | Publish specialist/custom-adapter gaps for relevant files. |

## Source mapping and parser loops

Store original bytes, decoded physical lines and a mapping from normalized spans to original physical spans. Preserve line endings and original columns for evidence display. Separate preprocessing from syntax recognition. The adapter scope must declare whether it handles an entire file or only selected statements. An unterminated statement returns a diagnostic with the best trustworthy span and does not create an unsupported edge.

```text
read record → select code area → skip comments outside quoted text
            → join only supported continuations → tokenize
            → recognize one supported statement → attach original spans
            → emit observations and reference operands
```

### COBOL and copybooks

1. Use explicit fixed/free settings if provided. For automatic detection, use consistent sequence/indicator columns on a bounded sample or decisive free-format syntax. An ambiguous layout requires a setting or partial diagnostic. The v1 fixed-format profile reads code columns 8–72 and treats documented comment/debug indicators separately. Physical evidence includes every continuing line. Unrecognized indicators, mixed modes or untested continuation forms preserve raw spans with a scope warning.
2. Lex identifiers, delimiters, strings and statement boundaries. Ignore keywords in comments and string contents other than recognized literal operands. Maintain the current program scope. A malformed or ambiguous nested program must not lend its references to a guessed enclosing program.
3. Recognize `PROGRAM-ID`, literal `CALL`, identifier `CALL`, `COPY`, `SELECT ... ASSIGN`, simple file declarations and a narrow operation subset. Store token kind so a quoted target and an identifier remain distinct. A literal call adds an unresolved reference record for the linker. A dynamic call retains its identifier. An optional literal-assignment hint may list possible names with supporting lines, without claiming a runtime binding.
4. Bind a file operation through a supported logical file or uniquely mapped record declaration. For `WRITE` forms that name a record, join the record to its owning file before assigning direction. Otherwise retain the operation with unknown file binding. `OPEN` flags can add intent, with no proof that a read or write executed.
5. For copybooks, record member identity and supported level/name, PIC, USAGE, OCCURS and REDEFINES facts. Qualify duplicate field names by parent path and ordinal where needed. Group entries can omit PIC. Record nested COPY references, detect cycles, and keep expansion separate from member discovery. Unsupported REPLACING, variable lengths and offset calculations are documented gaps. Raw packed data remains out of scope.
6. Count supported constructs by unique source occurrence. Use decisions and `GO TO` as observed counts where supported. Skip control-flow depth or full cyclomatic claims until a tested structural parser exists.

For a small behavior catalogue, support one explicitly delimited COBOL form: a simple field-to-literal comparison in an IF, one literal MOVE to a named field, and END-IF. Record program, condition, effect and original spans. Label it a candidate behavior to preserve. A statement can establish the assignment it describes without establishing its business purpose or whether the branch executes. Nested control, ELSE, abbreviated comparisons and unbounded statements stay outside this recognizer. Recognize them as limits without emitting a misleading partial rule. No model participates.

Expose the existing copybook observations as a searchable field dictionary. Key a field by copybook, parent path and occurrence. Show PIC, supported usage, repetition/overlay flags, the declaration location, and programs known to include the member. Including a copybook establishes a member-level dependency. Display field-level usage only when an actual field reference can be bound.

Examples to test include comment text containing `CALL`, an actual quoted target on a continued statement, a variable target with one plausible assignment, two sibling groups with the same field name and a copybook cycle. Each example declares expected evidence spans and which output is withheld.

### JCL, embedded SQL and online source

| Family | Parsing steps | Direction and unresolved behavior |
|---|---|---|
| JCL | Recognize control-record prefix and operation. Join supported comma continuations, split operands only outside parentheses and quotes, then group JOB, EXEC and DD records by context. Track in-stream data so text resembling control statements is skipped. Preserve ordinal and physical span. | A literal EXEC PGM names a program. PROC references use distinct target type. Capture literal DSN, DD and selected DISP metadata. Use code binding evidence for read/write. DISP, job file order and shared reads do not establish actual schedule or write behavior. |
| JCL variations | Distinguish symbolic or temporary DSNs, GDG generations, DD DUMMY/SYSOUT, concatenation, overrides, backward references and conditional execution when recognizable. | Emit literal references only when sufficiently scoped. Unsupported symbol substitution or expansion is partial. A temporary dataset stays within its scope. A relative generation remains an expression. |
| SQL/DDL | Recognize delimited EXEC SQL outside COBOL comments, tokenize SQL comments and literals with separate rules, then classify named CREATE TABLE and straightforward single-table SELECT/INSERT/UPDATE/DELETE. | Store declared identifier and statement location. Resolve qualified names and safe configured schema matches. Dynamic SQL, CTEs, complex joins and ambiguous aliases retain a scoped diagnostic. A table reference without DDL stays unresolved or documented external. |
| CICS/BMS | Recognize tested delimiter and literal operand forms for LINK, XCTL and named map/file references, plus explicit map definitions. Join transaction to program through literal CSD or typed evidence, and program to map through supported source references. | Dynamic operands remain unresolved. Name collisions across mapsets require mapping. Absence of CICS statements does not establish batch-only scope. Transaction state, syncpoints and advanced region definitions need separate review or supported input. |

### Non-code evidence and consistency

For optional post-result review, offer annotation fields by affected scope and show which checks use each answer. The analyst may enter a typed declaration, point to a supplied file/line and request review. Provide fields for ownership, business behavior, interface direction and contract, scheduler condition, recovery and monitoring, and access/audit. Add `test_coverage` records in the code category when a team supplies test scope, validation owner and artifact location. This informs the business/data and review gates when behavioral preservation requires baseline tests. A department or person is named only when supplied by the team. Use role labels in suggested actions.

Apply exact contradiction checks to comparable subjects and predicates. Examples include two incompatible current owners for one scoped store, different declared predecessors for one exclusive schedule condition, or an interface without a matching configured endpoint. Different environments or time windows remain separate. A handoff without a schedule is a gap, and a separate schedule direction with a plausible mismatch creates a review question. A text excerpt shows what a document says. Freshness or applicability needs provenance or review. Never replace one source with another silently.

Record an evidence item as usable only for the assertions its schema and source support. A syntactically valid but unrelated file changes inventory, not the targeted requirement. Conflict preserves both records and may affect the gate. Display artifact provenance, capture date and environment where supplied. A fresh artifact can still be irrelevant.

## Reading existing non-code resources without a worksheet

Inspect supported file contents regardless of the chosen folder. Version each adapter and store its identity beside every observation. The following narrow formats define what the demonstration must implement. All example resources are newly authored. Include the source export exactly once. The user has no obligation to translate another format into these shapes.

| Original resource | Recognizer and output | Where extraction stops |
|---|---|---|
| Scheduler table in a CSV export | Parse a header and quoted cells, with original physical row spans. Match case-insensitive trimmed columns `predecessor`, `successor`, `condition`. For a row such as `STOCKRUN,STOCKRPT,success`, emit a documented predecessor, successor and condition tied to that row. Link only unique in-scope jobs. | Reject duplicate headers, empty keys and unsupported condition expressions as row/file diagnostics. Preserve environment or date columns when present. A declaration can improve availability immediately. A reviewed precedence edge requires a scope check. |
| CICS definition listing in text | Recognize a bounded DFHCSDUP `DEFINE TRANSACTION(...) GROUP(...) PROGRAM(...)` statement with literal operands. Capture group, transaction and program with their source spans. Resolve the named program and follow separately observed screen/table uses. [IBM DEFINE](https://www.ibm.com/docs/en/cics-ts/6.x?topic=commands-dfhcsdup-define-command) · [IBM EXTRACT](https://www.ibm.com/docs/en/cics-ts/6.x?topic=commands-dfhcsdup-extract-command) | Missing operands, ambiguous groups and unsupported syntax create gaps. A definition does not establish which region installed it. |
| Access configuration statement in text | For a supported literal RACF `PERMIT` with one profile, `CLASS`, `ID` and `ACCESS`, capture a declared access instruction and its original line. Link a resource only with an evidenced class/profile mapping. [IBM RACF guide](https://www.ibm.com/docs/en/SSLTBW_2.2.0/pdf/ich2a111.pdf) | Wildcards, member expansion, commands in quoted documentation, revocation and unimplemented operands cannot establish effective access. Command presence supplies neither effective authorization nor proof of auditing. |
| Interface register in CSV | Recognize `component`, `external_system`, `direction`, `contract`, with optional `owner_role`. A unique scoped component and a supported direction give a declared interface. Keep the evidence row for every field used. | Multiple possible subjects stay ambiguous. An empty contract leaves contract coverage open even when the endpoint name is present. Ports, queues and observed delivery need separate evidence. |
| Text runbook, ownership note or test description | After indexing discovered identifiers, find exact delimited subject mentions under recognizable headings such as Recovery, Monitoring, Ownership or Expected outcome. Retain a bounded paragraph and its original lines as a review lead. | Treat quotations, negations and historical instructions conservatively. A mention or section title cannot pass a gate. An unrelated document stays in inventory. PDFs, images, binary dumps and unrecognized vendor formats get a visible coverage limit. |

Structured CSV schedules and interface registers above are supported export shapes, not claims of generic vendor compatibility. Unknown layouts go to inventory and a scoped action. In Cedar, use original CSD and permit statements plus an existing-format interface register and an audit note. In Harbor, demonstrate a schedule change by adding one CSV. Neither fixture contains an answer key, manifest or annotated claims in the selected inputs.

## Settings and evidence provenance

Before discovery, persist scope and settings with an origin of `default`, `supplied_override` or `reviewed`. Store post-parse workload/entry observations separately as `detected` with evidence IDs. This avoids a circular dependency between choosing analyzers and knowing what they will find. Keep reviewed exclusions separate from the automatic intake perimeter. Apply a per-path decoding override only on an explicit advanced run. A default decoder must fail strictly, retain the digest and request a supported export when bytes cannot be read. Never guess a production code page from a filename.

A first-party CLI or picker constructs a virtual root for loose files. Enumerate exactly the selected paths. Siblings on disk and remote libraries are outside that root. Preserve relative paths supplied by the picker, or assign unique basenames if it supplies none. Indistinguishable collisions fail admission with instructions to select their enclosing folder. For multiple directory trees, keep top-level directory names. Preserve the submitted path separately from any stripped archive wrapper. [Automatic defaults](07-files-to-results.md#defaults-that-prevent-accidental-claims)
