# Assessments and recommendations

## Evaluation order and rule format

Evaluate scope and applicability, then evidence completeness and reference gaps, then complexity signals, confidence checks, readiness gates and recommendations. Use one versioned rubric across applications. Each rule specifies inputs, applicability, supported observations, positive and negative cases, an unknown outcome, evidence to preserve and a recommendation template. A finding has an observation, separately labeled interpretation, assumptions and unresolved questions. Use stable rule IDs. Review can supply evidence to the rubric through a separate event stream.

## Complexity rubric v0.1

Use the table to seed the initial heuristic. A qualifying signal needs case-specific evidence. Score the strongest observed signal per dimension. Show **High** for a major signal, **Moderate** for a material signal, and **Low** when prerequisites are usable and positively reviewed coverage supports the absence of material signals in the assessed scope. If the strongest signal is present while a prerequisite is incomplete, show its band and mark it provisional. With incomplete prerequisites and no qualifying positive signal, show **Unknown**. Reviewed inapplicable dimensions show **Not applicable** with a reason.

| Dimension | Example prerequisite for a non-provisional result | Moderate signal | High signal |
|---|---|---|---|
| Code and business logic | Critical entry points and their local call/copy paths reviewed with supported coverage | Literal calls cross a reviewed application boundary | Resolved mutually dependent program group spans candidate boundaries |
| Data | Critical stores, layouts, access direction and ownership documented | Shared copybook or layout feature needing conversion checks | Multiple critical flows write one persistent store or a reviewed invariant spans stores |
| Batch | Critical JCL and relevant schedules and recovery procedures reviewed | Evidence-supported writer-to-reader handoff | Confirmed restart after partial updates requires reconciliation across jobs/stores |
| Transactions and UI | Critical transactions, program/screen mappings and state boundaries reviewed | Multiple supplied transaction paths share a stateful program | Reviewed behavior requires an atomic update across resources or persistent conversational state across steps |
| Integrations | Critical external endpoints or interfaces, direction and contracts reviewed | Contracted dependency on another system | Confirmed synchronous dependency on an external system within a critical business update |
| Security | Critical access boundaries, authorization and audit requirements reviewed | Target needs a documented identity or authorization mapping | Reviewed cross-system authorization and audit obligation spans multiple critical owners |
| Operations | Critical restart, monitoring, service objectives and ownership reviewed | Documented manual intervention or recovery timing dependency | Reviewed recovery requires coordinated actions across systems or teams |

The aggregate is the highest demonstrated band among applicable dimensions. Mark it provisional if any applicable dimension is unknown or provisional. If none yields a band, show Unknown. Display the dimension profile alongside the aggregate. This ranking is a proposed planning heuristic. It has no conversion to hours, cost or probability. Calibrate definitions with an SME during rubric implementation and record changes as a new version.

Implement each signal as a named predicate. For example, `BATCH-HANDOFF` is true when two distinct in-scope job steps have evidence-backed write and read bindings to the same concrete resource. Repeated references from the same step give one contribution per resource and pair. Two reads cannot satisfy it. Unresolved bindings produce a gap. `DATA-SHARED-WRITE` uses two distinct critical flows with writes to the same persistent resource. The other predicates in the table use the same true, false and unknown contract. Require reviewed evidence for predicates explicitly marked reviewed or confirmed. Preserve missing prerequisites by subject so a reviewer can see exactly why a band is provisional.

## Assessment confidence rubric

Evaluate four checks independently for each applicable dimension:

1. **Availability.** Required evidence categories for the dimension have usable material for each critical subject.
2. **Extraction quality.** Relevant automated input is analyzed for the required constructs, or the relevant statements are manually assessed with source links.
3. **Resolution.** Material references are resolved, explicitly identified as external with reviewed contracts where relevant, or otherwise reconciled with supporting evidence.
4. **Independent review.** A suitable named reviewer confirmed the material interpretation and resolved relevant contradictions.

For each check show Pass, Fail, Unknown or Not applicable with rationale. Availability can fail when an expected item is known missing, and extraction can fail on an observable parsing problem. Unknown covers indeterminate scope or evidence. Evaluate within the declared input scope and review perimeter. The denominator includes Pass, Fail and Unknown. A reviewed Not applicable check is excluded. All applicable checks passing yields **High**. At least half passing with one or more unmet or unknown checks yields **Moderate**. Fewer than half passing yields **Low**. Show **Unknown** when there are zero applicable checks in an otherwise applicable dimension, scope prevents establishing a denominator, or no usable evidence exists for that dimension. The overall confidence is the lowest applicable dimension band, with Unknown reported separately as incomplete coverage. If any applicable dimension is unknown, show the lowest known band as provisional, or Unknown if no known bands exist. Always show category/file/reference counts, their denominators and unresolved critical subjects. These bands measure evidence support, with no implied statistical probability.

## Planning readiness gates

| Gate | Evidence required for Pass |
|---|---|
| Scope | Named owner and reviewed list of in-scope critical entry points, missing categories and justified exclusions |
| Dependencies | Critical references and shared-resource relationships resolved or reconciled with traceable review |
| Business and data | Critical behavior risks, stores, direction and ownership documented and reviewed |
| Operational | Relevant scheduler, recovery/reconciliation, monitoring and ownership evidence reviewed |
| Security and interfaces | Relevant access/audit and external contract responsibilities reviewed |
| Review completion | Critical findings and contradictions reviewed, with dispositions and rationale |

A gate fails when evidence demonstrates an unmet criterion. It is unknown when evidence or a review is missing. A justified not-applicable decision needs a named reviewer and evidence of perimeter. All listed gates are critical when applicable. Aggregate precedence is Fail → **Not ready**, Unknown → **More evidence required**, all applicable Pass → **Ready for detailed planning**. Unverified scope cannot produce a ready result. A review correction recomputes the projection while retaining the original machine assessment. Accepted risk is visible and leaves the gate outcome governed by its criteria.

Evaluate each gate per critical entry point or shared resource where applicable, then roll up by Fail, Unknown and Pass in that order. Scope and review completion always apply. No in-scope entry points yields Unknown for scope until a reviewer establishes a valid perimeter. Link unresolved predicates and affected subjects directly to the gate. Accept reviewed exclusions only when they agree with discovered evidence or have an explicit resolution.

## Recommendations

Emit one action per unresolved requirement or investigating step, deduplicated by subject and requested remedy. Include trigger, evidence or missing requirement, requested action, owner role, decision affected, scope, completion evidence and conditional effect. Rank critical failed or unknown gates first, then material risk investigations, then housekeeping. Within a tier sort by impacted critical entry-point count and stable key. Supply deterministic templates for each signal and gate gap.

| Trigger | Action | Closure evidence |
|---|---|---|
| Required COPY target missing in supplied scope | Application SME supplies exact library member or documents supported external resolution | Unique parsed definition or reviewed external resolution with provenance |
| Writer-to-reader handoff, schedule unavailable | Batch owner supplies schedule export and confirms normal and recovery order | Supported schedule and restart evidence linked to both jobs |
| Critical layout has packed fields or overlays | Data owner validates record interpretation and plans a representative conversion check | Reviewed field examples and documented validation scope |
| Shared writable store | Architect and data owner compare ownership and consistency options | Reviewed write paths and explicit decision questions. Complexity still derives from the observed design. |
| Effective access unknown | Security owner supplies scoped access and audit evidence | Applicable gate requirements satisfy named review |

Action states are open, in progress, resolved, accepted risk and not applicable. Require supporting evidence and rationale for closure. An action status by itself leaves the gate unchanged. An optional model may rewrite a supported action as a plain-language brief. It cannot assign a band, decide readiness or resolve conflicting evidence.

For a complex design, provide a candidate planning consideration alongside the evidence request. For shared writes, compare retaining the common store initially with investigating a single owning boundary. For a batch handoff, compare preserving the current file exchange and schedule with investigating another handoff once delivery and restart behavior are understood. For each option record its prerequisite, behavior to preserve and unresolved trade-off. Emit options only for triggered signals and keep them separate from approved design decisions.

## Worked trace

A job step writes STOCK.OUT, and a second step in another job reads it. The graph exposes both static bindings with original program and JCL evidence. The batch result is at least **Moderate, provisional** under the handoff rule. With missing scheduler evidence, the confidence profile shows an availability gap and the operational gate is unknown. The action requests the schedule and a restart review. Attaching a scheduler file alone changes availability only when it contains usable evidence. A later review may satisfy sequencing while a missing restart procedure keeps the gate open. The underlying data handoff can remain Moderate after confidence improves.

## Evidence requirement catalog

Evaluate expected evidence by requirement and critical subject. A requirement record contains ID, dimension, scope selector, trigger, expected artifact or declaration, evidence acceptance conditions, gate IDs and a gap template. Implement triggers as named predicates. A reviewed batch-only perimeter can exclude online checks. A parser's failure to find online code cannot establish that perimeter.

| Requirement ID | Trigger and scope | Evidence accepted for evaluation | Gate link |
|---|---|---|---|
| `REQ-SCOPE` | Every run | Perimeter, named application owner, entry points and reasoned exclusions | Scope |
| `REQ-CODE-PATH` | Each critical job/transaction or another explicitly included entry point | Usable program path, copybooks and material call resolution with original locations | Dependencies, business and data |
| `REQ-LAYOUT` | A critical record or dataset has a layout dependency | Reviewed layout and ownership, including any unsupported fields that affect the planning decision | Business and data |
| `REQ-STORE` | Critical table or store use | DDL or reviewed schema/description, evidence for access direction, owner and consistency requirements where applicable | Business and data |
| `REQ-BATCH-ORDER` | Batch workload with a critical scheduled or data handoff | Schedule evidence scoped to the affected jobs and condition | Operational |
| `REQ-RECOVERY` | Critical persistent update or applicable operational recovery requirement | Reviewed restart, partial-update and reconciliation procedure | Operational |
| `REQ-ONLINE-ENTRY` | Online workload or relevant observed transaction/map reference | Transaction/program and applicable screen mapping, state boundaries and ownership | Dependencies, business and data |
| `REQ-INTERFACE` | Discovered or declared critical external dependency | Contract, direction, relevant availability or timing, owner | Security and interfaces |
| `REQ-ACCESS-AUDIT` | Critical resource with an access or audit requirement or unresolved applicability | Reviewed identity, applicable operations, resource scope and audit responsibility | Security and interfaces |
| `REQ-MONITOR` | Critical workload | Monitored outcome, service objectives where required and escalation responsibility | Operational |
| `REQ-BASELINE-TEST` | Critical behavior whose preservation needs a baseline, as determined in scope review | Existing test or documented test scenario, expected outcome source, validation scope and owner | Business and data, review completion |
| `REQ-REVIEW` | Critical finding, gap, contradiction or gate with review requirement | Named disposition within reviewer scope, with supporting material and rationale | Review completion |

Per requirement record `usable`, `partial`, `missing`, `conflicted`, `unreviewed` or reviewed `not_applicable`. A `usable` record can pass availability and still await review before passing the relevant gate. Record source and extraction integrity separately. A requirement with missing data has a reason and a specific completion criterion. A document available elsewhere according to an analyst remains a declaration until the workbench can link evidence or record a reviewed attestation. An absence of checks yields unknown or reviewed not applicable according to applicability.

## Rule and finding contract

Use data to configure named evaluators. Keep predicates in first-party code and reject unknown evaluator IDs or fields at configuration load. Profile validation checks unique IDs, references to requirement and gate IDs, legal scope/applicability and required output fields. Run the same profile for both original applications. A changed rule version travels with every affected result and produces a new run or clearly labeled assessment projection.

```json
{
  "id": "BATCH-HANDOFF",
  "version": "0.1",
  "dimension": "batch",
  "subject_kind": "resource",
  "evaluator": "distinct_writer_reader_steps",
  "requires_for_full_coverage": ["REQ-BATCH-ORDER", "REQ-RECOVERY"],
  "on_match": {"signal": "moderate", "materiality": "material", "action": "CHECK-BATCH-ORDER"},
  "on_unknown": {"gap": "VERIFY-DATA-DIRECTION"},
  "evidence_selector": "read_write_bindings_with_step_context"
}
```

The engine returns `matched`, `evaluated_no_match`, `unknown` or `not_applicable`. It needs a corresponding evidence or requirement basis. An evaluator can observe a positive signal over a partial scope and record its limit. It cannot use partial extraction to justify “no match” for the whole scope. Group repeated occurrences into one finding per rule and canonical subject. Preserve counts, source IDs and statement occurrences separately. Prioritize a small set in the UI while retaining all grouped findings for export.

| Rule | Trigger and evidence | Result or action |
|---|---|---|
| `CODE-DYNAMIC-TARGET` | Identifier CALL remains dynamic in scope. Cite operand and any hints. | Unresolved path gap and owner question. Missing target alone adds no complexity band. |
| `CODE-COUPLED-CYCLE` | Resolved typed SCC crosses a reviewed organizational or candidate boundary. Cite member edges and boundary evidence. | High code signal and joint behavior/release review. A self-loop without that scope evidence is a diagnostic. |
| `CODE-EXTERNAL-CALL` | Resolved literal call crosses a reviewed boundary. Cite call and boundary ownership. | Moderate code signal, scoped contract review. |
| `DATA-LAYOUT` | Overlay, variable layout or packed-storage feature needing conversion checks, or shared record definition with a reviewed compatibility concern | Moderate data signal. Ask for representative records and field validation. Report conversion as candidate work. |
| `DATA-SHARED-WRITE` | Two distinct critical flows write the same persistent store with supported access evidence | High data signal. Ask who owns it and what consistency constraints apply. |
| `DB-SCHEMA-GAP` | Critical SQL reference without resolved schema or reviewed contract | Data requirement gap. This is not a second domain score or an automatic high signal. |
| `DATA-CROSS-STORE` | A reviewer confirms a business invariant across stores and links evidence | High data signal and a request to clarify update/reconciliation behavior. |
| `BATCH-HANDOFF` | Distinct step write/read pair established | Moderate batch signal. Link schedule and recovery questions independently. |
| `BATCH-RECONCILE` | Reviewed restart requires reconciliation after partial writes across jobs or stores | High batch signal and baseline recovery-test consideration. |
| `ONLINE-SHARED-STATE` | Multiple transaction paths use a common program and evidence establishes shared state | Moderate transactions/UI signal. A common program alone is an observation. |
| `ONLINE-STATE-BOUNDARY` | Reviewed online behavior needs coordinated writes or a preserved state boundary across exchanges | High transactions/UI signal. Verify session/transaction behavior before suggesting a boundary. |
| `INTEGRATION-CONTRACT` | Reviewed dependency and contract for another system | Moderate integration signal. |
| `INTEGRATION-IN-UPDATE` | Evidence confirms a synchronous external dependency in a critical update | High integration signal. Review timing, failures and replay semantics. |
| `SEC-IDENTITY-MAP` | Reviewed access boundary needs identity/authorization mapping | Moderate security signal. Configuration absence is a gap. |
| `SEC-AUDIT-COORDINATION` | Reviewed cross-system authorization/audit obligation spans critical owners | High security signal. Preserve scope, evidence and ownership. |
| `OPS-MANUAL-STEP` | Documented manual intervention or recovery timing dependency | Moderate operations signal. |
| `OPS-COORD-RECOVERY` | Reviewed recovery requires coordinated action across systems or teams | High operations signal. |
| `EVIDENCE-CONFLICT` | Comparable, mutually exclusive claims for the same subject and context | Preserve both sources, mark review pending and link the applicable requirement. |
| `UNSUPPORTED-CRITICAL` | Unsupported artifact lies on or supplies evidence for an identified critical path | Specialist review flag and coverage gap. Record affected scope even when the asset cannot be parsed. |

Capture two data tracks in the data panel, record layouts and stores/database, and roll both into the one Data band. Expose measurable signals versus unresolved prerequisites in all dimensions. Give each row a rule ID, affected scope, input facts, outcome, provisional reason and source links. Preserve the rubric's existing aggregate based on the strongest demonstrated band. Unknown factors remain explicit. Optional sensitivity notes may describe which finding could raise or qualify a band, conditional on a future observation. Avoid a numeric range presented as a prediction of migration effort.

### Decision cases for automated checks

| Case | Complexity | Confidence | Readiness |
|---|---|---|---|
| Critical export exists, every scored construct is unsupported | Unknown except for separately supported positive signals | Unknown where usable evidence is absent | More evidence required unless a separate critical failure is confirmed |
| One supported batch handoff and no recovery or schedule evidence | Moderate, provisional for batch | Availability cannot pass for the missing requirements | Operational gate Unknown |
| Usable schedule is added and reviewed, recovery still missing | Batch handoff signal unchanged unless new evidence supports a different signal | Schedule availability can improve. The overall band may be unchanged. | Operational gate can stay Unknown |
| Confirmed cross-system recovery is costly to coordinate and all planning gates pass | High operations | Can be High | Ready for detailed planning |
| Reviewer resolves a source conflict but rejects a derived interpretation | Recompute only affected signals from active evidence | Review and consistency can change | Recompute affected gates, retain original machine result |

## Risk hotspots and evidence-gap ranking

A hotspot is a ranked investigation item grouped by subject and root cause. It can be a supported complexity signal, critical unresolved dependency, contradiction or operational planning blocker. Its record includes materiality, trigger IDs, scope, evidence, graph-derived affected entry points, unknowns, limit flags and recommended action IDs. Duplicate manifestations can join a root-cause group without overwriting their evidence. A false positive rejected in review remains in history and is removed from the active queue unless another active trigger remains.

Sort active items by this priority tuple:

1. Confirmed failed critical gate, then unknown critical gate, then other material signal, then informational.
2. Documented materiality, with critical ahead of material and informational.
3. Number of distinct affected critical entry points established by the typed impact query, descending.
4. Evidence disposition and stable action key. Put unresolved material items before reviewed items of otherwise equal priority. If impact counts are capped, show the lower bound and expose the cap in the explanation.

Record the sort inputs. An unknown or large graph count is not a probability. Business priority comes from reviewed scope and evidence. A highly referenced utility is a structural hub and may be worth inspecting, with business impact undecided. Do not add a guessed owner name or effort estimate.

### Gap Planner output and recomputation

Tie each action to one or more requirement IDs and the relevant gate or signal. The action template includes where to look, who can answer, what qualifies as evidence and what outcome is possible. Examples of actionable specificity include a scheduler extract for both jobs and its success condition, a copybook member and library alias for the exact missing reference, or a recovery excerpt that answers how partial writes are reconciled. Items sharing a request can group together and retain independent closure criteria.

For each action, say which checks the evidence **could affect**. A syntactically valid file can be unusable or contradictory. Do not promise score or readiness gains from its mere addition. The evidence-delta view compares actual recomputation and reports which requirement changed status and why. Report new findings along with closed gaps. If the rubric or analyzer version changed, label the comparison and withhold an isolated evidence-effect claim.

### Candidate planning considerations

| Evidence trigger | Candidate to investigate | Preconditions and caveats |
|---|---|---|
| Stable batch handoff and accepted timing/recovery evidence | Preserve the existing exchange in an initial phase, then compare alternate handoff methods | Confirm delivery ordering, restart and duplicate handling. |
| Seeded group with few demonstrated code crossings and no observed external writers | Examine the group as a unit for phased planning | Review missing links, ownership, test evidence and runtime constraints. The visible subgraph is limited by the export. |
| Multiple critical writers or a cross-store invariant | Compare keeping the shared store temporarily with establishing a single owning boundary | Investigate write coordination, reconciliation and approved ownership before choosing any design. |
| Documented critical external update dependency | Compare retaining the dependency temporarily with further contract or isolation investigation | Confirm latency, failure semantics and audit requirements. |
| Unsupported critical artifact or unreviewed deployment-specific behavior | Defer the design decision pending specialist analysis | Identify the exact artifact or gap and required specialist role. |

Each option carries trigger evidence, behavior to preserve, unanswered prerequisites and a reviewer decision if recorded. A candidate output does not change readiness or marks in the rubric. Architecture choices remain outside the implementation workbench.

## Results before anyone has answered a question

At revision zero, evaluate available source observations immediately. Establish the technical perimeter from the submitted files and list each discovered entry as potentially critical. Derive eligible evidence checks from that technical scope. Display the applicable denominator when it can be established. Unknown business perimeter, review or deployment facts stay visible even for a fully parsed export. Keep security, operations and other domains visible when their applicability is open. Leave the scope and review gates open until their criteria have independent support.

| What the machine sees | What a reviewer receives immediately | Action and possible effect |
|---|---|---|
| Two steps bind the same dataset with supported write/read evidence | Batch complexity Moderate, provisional under `BATCH-HANDOFF`. Operations gate Unknown. Both statements and both bindings are available. | Request the scheduler export for those two jobs. A usable row can establish documented order; recovery and actual execution remain separate questions. |
| A permit command and a matching resource definition, with no proof of effective identity or audit | The report names the declared permission and its scope. Confidence explains which evidence is missing. | Request the applicable access and audit evidence from the security owner. Verify the resource and effective environment before closing the gate. |
| A data layout has a demonstrated hard-to-preserve feature | Cite the exact field and list entry points using its copybook. The observed complexity signal survives a more complete export. | Ask for representative records and an expected-value comparison. This can improve planning preparedness while leaving the underlying difficulty unchanged. |
| Only binaries or uninterpretable material | Show inventory, reasons and Unknown for unmeasurable dimensions. The planning result is More evidence required unless an independent critical failure is established. | Name the smallest usable replacement, for example a text export of the JCL member used to run the job. |

A helpful recommendation answers “Which decision is blocked, who could answer it, and what evidence would close it?” Show the lowest-effort relevant request, affected checks and an observable completion test. Receiving new evidence triggers a real reassessment. Show the actual effect, including a newly discovered risk or an unchanged band.
