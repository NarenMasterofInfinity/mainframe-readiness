# Implementation sequence

1. **Foundation and spike:** create project skeleton and CLI; pin sample scopes; evaluate the first-party extractor against a small annotated fixture, source locations and offline execution. Confirm a compliant runtime before implementation.
2. **Input and model:** implement ZIP/directory intake, safe extraction, artifact inventory, decoding, local persistence schema and run snapshots after the storage dependency is resolved.
3. **Discovery:** implement supported COBOL/copybook/JCL paths and selected SQL/CICS references; preserve evidence and report unknowns and parse failures.
4. **Assessment:** implement versioned dimensional rules, coverage, readiness gates and evidence-linked recommendations. Add reviewer notes and status.
5. **UI and report:** implement intake, overview, graph/table, findings, actions and self-contained HTML/JSON export.
6. **Optional AI:** connect Tachyon only if deployment constraints permit; implement one structured business-flow brief with evidence links and review status.
7. **Validate and rehearse:** independently check fixtures, run both applications, test blocked-network operation and export, document licenses, limitations and reuse instructions.

At each stage, complete a vertical slice before adding breadth. First milestone: one JCL → program → copybook or dataset path with source evidence and an exported finding. Do not add later features if the core evidence path is unreliable.

## Decision and quality gates

**Gate 0 — environment:** resolve the third-party license restriction against the OS, runtime, browser, local store and optional model. No implementation dependency is approved by these plans. If unresolved, continue only dependency-free design and original fixture authoring.

**Gate 1 — evidence slice:** ingest a first-party fixture and trace one observed JCL statement through a resolved program to evidence. A DD-to-dataset binding is inferred only with supporting statements. Include at least one unresolved reference and parse diagnostic.

**Gate 2 — meaningful assessment:** apply a documented rule and readiness gate, expose an unknown and generate a traceable recommendation. A reviewer can record disagreement without changing the original observation.

**Gate 3 — end-to-end:** UI and exports expose the same run, source locations, limits and version data. Confirm no external assets or calls in the approved environment.

**Gate 4 — reuse:** assess two independently authored fixture applications using the same engine and rules; document configuration differences and manual review. Validate against held-out annotations and list unresolved coverage.

Optional AI and boundary exploration start only after their prerequisites are approved and must not block deterministic delivery. Assign owners and dates when staffing and hackathon duration are known; no schedule is assumed.
