# Assessments and recommendations

## Three independent outputs

- **Complexity:** Low, Moderate, High or Unknown per dimension, based on observed signals. High observed coupling is a planning concern, not an effort estimate.
- **Confidence:** evidence availability, parser quality, reference resolution and review status per dimension. Show counts with explicit denominators; unknown estate size stays unknown.
- **Readiness:** gate status: passed, failed or unknown for scope, dependencies, data, operations, security and SME review. A material unknown prevents an unconditional ready result.

Dimensions include business logic/code, data, batch, transactions/UI, integrations, security and operations. Keep unsupported or absent evidence unknown. An overall complexity band is provisional if material dimensions are unknown; no numeric overall score is required. Configurable rules, thresholds and weights are initial team heuristics, versioned and subject to SME calibration, not industry standards.

## Initial rule for the first slice

If a required copybook is referenced but not found within the configured search scope, record its reference as unresolved, add a code/data confidence gap and set the relevant dependency-review gate to unknown. Mark it resolved only when a uniquely matched copybook has been analyzed or an SME documents a justified resolution. Do not change complexity solely because it is missing. Other thresholds and aggregation rules must be defined and SME-reviewed before implementation; retain unknown until then. Overrides retain the original result, reviewer, date and rationale.

## Explainability contract

Each result shows contributing findings, evidence IDs, rule/version, known gaps and human overrides with rationale. Separate a source observation from its migration implication. A manual review never deletes the original finding.

## Recommendations

A recommendation links to findings and a gap or gate and includes action, rationale, evidence, suggested owner, completion criteria, status and expected effect. Prioritize by impact on planning, affected scope and evidence quality; label effort unknown unless supplied by a reviewer. Examples:

- Missing copybook → obtain it and rerun analysis to assess affected data structures.
- Missing scheduler → obtain definitions and confirm job order; this may improve batch confidence.
- Shared dataset → map readers/writers and confirm ownership and consistency before deciding a boundary.

Suggestions to improve confidence or readiness concern evidence and review. Complexity recommendations investigate risk and trade-offs; do not modify code to improve a score. New evidence can increase confidence and reveal higher complexity. Compare runs by source hashes and rule versions; do not promise an outcome before reassessment.

## AI-assisted interpretation

An optional Tachyon task may draft an evidence-linked recommendation, candidate business rule, modernization brief or test scenario. Provide only selected source context and normalized findings; require references, unknowns and structured output. Validate citations exist, preserve the generated label, and require SME review. LLM text does not independently establish facts or change gate status. See model cues in [core engine](01-core-analysis-engine.md).

## Reproducible evaluation procedure

1. Declare the in-scope dimensions and expected evidence categories; a reviewer can mark an item not applicable only with a reason.
2. Run rules over observed, normalized evidence and preserve each triggered signal and non-evaluable prerequisite. Deduplicate repeated observations only when their identities and evidence warrant it.
3. For each dimension, report observed signals and a band only when that dimension's rule prerequisites are satisfied. Otherwise show Unknown or provisional with the precise missing prerequisites. Do not average unknown into zero.
4. Confidence is a profile, not a probability: report category availability, parse status, unresolved critical references and SME verification independently. A denominator refers only to the declared scope and is labeled; do not calculate a full-estate completeness percentage without an independent inventory.
5. Gate outcomes: **passed** requires all defined evidence and reviews; **failed** means an explicit required condition is not met; **unknown** means the evidence or review needed for a decision is unavailable. Overall readiness is not ready if a critical gate fails, insufficient evidence if none fails but a critical gate is unknown, and ready for detailed planning only if all critical gates pass. This is a proposed policy requiring SME approval.
6. Recompute from a new run after evidence changes. Overrides include reviewer, rationale, date and original result; never quietly alter source evidence.

## Rule specification and example

Each rule defines scope, predicates, prerequisites, evidence selection, signal, band mapping, unknown behavior, rationale and version. Example initial heuristic: a missing required COPY target produces an unresolved-reference finding and an unknown dependency-review gate; it does not establish low or high code complexity. If an artifact fails to parse, downstream rules that require its output cannot pass by default. Set no arbitrary count thresholds until calibrated with independent SME-labeled examples.

## Recommendation lifecycle

Generate a deterministic recommendation for each actionable critical gap, combining only findings that share an identifiable resolution. Deduplicate by target and action; allow a reviewer to mark open, in progress, resolved, accepted risk or not applicable with notes. Resolution requires explicit completion evidence and reevaluation; closing a task alone does not pass a gate. Rank first by critical gate impact, then affected scope and documented urgency; where information is unknown, display it rather than fabricate effort or benefit. Optional LLM text must cite existing evidence and must not change scores, ownership or task state automatically.
