# Validation and demo

## Independent checks

Create a small fixture corpus annotated by a mainframe SME without viewing analyzer output. Include fixed-format source, comments and continuations, static/dynamic calls, missing/ambiguous copybooks, JCL steps/DD/procedures, selected SQL/CICS, encoding failures and unsupported input. Measure detected expected edges, false positives, missed edges and correct source locations against the annotated scope. Separately review candidate business rules for support, material omissions and misleading interpretation; a valid citation alone is insufficient. Report sample size and limitations; do not present fixture accuracy as estate-wide accuracy.

Test rules with known inputs: missing evidence never becomes low complexity; confidence can rise while complexity rises; unknown critical gates block an unconditional ready result; rule versions and overrides remain visible. Test input safety and error reporting. Validate exports, UI evidence navigation and no outbound calls with network access blocked after dependencies and sample artifacts are installed.

## Demo

Create two distinct original synthetic applications within the project, with independently annotated expected outputs and no third-party source copied into them. Run the same configuration-driven workflow on each. These fixtures are illustrative and cannot establish accuracy on production systems.

1. Show intake and unsupported/missing evidence.
2. Show the three assessments and trace one driver to original source.
3. Follow job → program → data in the graph/table.
4. Open a prioritized recommendation and explain what evidence would resolve it.
5. If available, generate and review a labeled AI brief.
6. Assess the second application, then export HTML and JSON.

Prepare a precomputed backup while demonstrating at least one actual run. Human review must be completed before claims of readiness.

## Test matrix and exit criteria

- **Ingestion:** nested archives, traversal, duplicate paths, expansion limits, binary inputs, encoding mismatch, empty archive and partial upload. Expected result is explicit rejection or diagnostic, never silent omission.
- **Parsing:** annotated positives and negatives including comments, literals that resemble syntax, continuation, fixed-format columns, multiple definitions, dynamic identifiers and unrecognized syntax. Check original line mapping after preprocessing.
- **Resolution:** same program name in two libraries, absent copybook, unresolved symbolic JCL, logical-file/DD binding and two readers of one dataset. Verify ambiguous links remain ambiguous and shared reads do not imply job ordering.
- **Assessment:** missing inputs do not lower complexity; failed parsers cannot satisfy gates; not-applicable requires a reason; reviewer overrides retain originals; rule version changes trigger a new result.
- **Recommendations:** every critical gap has a traceable action or documented reason for none; completing a task without evidence does not pass a gate; newly added evidence may reveal additional complexity.
- **AI, if approved:** independently annotate sample rules and compare correct, unsupported and omitted claims; verify evidence IDs, truncation handling and prompt-injection isolation. Human review establishes correctness, not the model checking itself.
- **System:** run both original fixtures through the same rules, inspect UI and exported source references, test interrupted runs and blocked-network execution in the approved environment.

Fixture authors and expected-output reviewers should work independently where feasible. Keep a holdout the implementer does not inspect before final evaluation; document reviewer and any shared assumptions. Any unsupported production claims remain limitations regardless of a passing synthetic fixture suite. Record actual counts and failures; do not invent numeric success rates before running tests.
