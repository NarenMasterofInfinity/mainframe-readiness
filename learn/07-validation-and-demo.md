# Plan 05 · Validation and demo

[Implementation plan](../plans/05-validation-and-demo.md)

## How we test and demonstrate it

A **fixture** is a small input assembled to exercise known behavior. The team will create two original fictional applications. Harbor Stock models batch work, and Cedar Permits models an online lookup or update. Harbor Stock also has an enriched input that adds schedule evidence.

```text
HARBOR STOCK               HARBOR STOCK               CEDAR PERMITS
Base                       Enriched                   Separate app

Batch build and report  →  Same program bytes        Online transaction
Missing schedule           + scheduler evidence      Screen and table
Missing recovery           Recovery still missing   External interface

       └────────────── Same workbench and rubric ────────────────┘
```

The enriched Harbor Stock package shows what the added evidence changes. Cedar Permits shows whether the same parser registry, schema and rules work for another application with different inputs and scope.

```text
Cedar Permits

Online entry → Program ── uses ──► Screen map
                  │
                  ├── reads / writes ──► Table
                  ├── has declared link ─► External interface
                  └── unresolved operand → Review question
```

These are planned fixture relationships. Each expected link needs an independent explanation and source location.

## How we know a relationship is correct

```text
Original inputs                      Workbench
      │                                  │
      ▼                                  ▼
Independent reviewer                 Extract and resolve
labels expected links                actual relationships
and original locations               and locations
      │                                  │
      └──────────► Compare ◄─────────────┘
                    │
                    ├─ Correct relationship
                    ├─ Extra or missed relationship
                    └─ Original location correct?
```

The reviewer makes the answer key without first seeing tool output. Use a development set to tune the workbench and a separate set held back until the method is fixed. Compare identities, relationship types, scope and source spans by artifact family. A score produced by the rubric can be checked against its written rules. Whether the rubric helps a planning decision is a separate SME judgment.

```text
Example only

8 expected links          7 reported links
├─ 6 found correctly       ├─ 6 correct
└─ 2 missed                └─ 1 extra

Recall = 6 / 8             Precision = 6 / 7
```

**Recall** asks how many expected links the workbench found. **Precision** asks how many reported links match the answer key. Test original locations separately. The example counts explain the calculation and do not represent a test result.

## Test the things that can mislead a reviewer

```text
Lookalike code       → A comment must produce no false call
Same program twice   → Two jobs can bind different datasets
Two readers          → Sharing alone produces no data handoff
Missing evidence     → Unknown or provisional stays visible
Review conflict      → A stale edit cannot overwrite newer review
Export offline       → The HTML and JSON identify the same revision
```

The validation plan names the cases and expected outcomes. It also checks wrong encoding, unsupported statements, source changes, graph limits, cancellation, reruns, keyboard navigation and optional model citations. With outbound access blocked, confirm that the core workflow finishes and observe whether it tries to make an outbound request.

## Walk through the judge demo

```text
0:00       0:45          2:00          3:00          4:15      5:00
  │          │             │             │             │         │
  ├─ Intake ─┼─ Evidence ──┼─ Reassess ──┼─ Cedar ─────┼─ Export ┤
  │  Stock   │  and impact │  Stock      │  Permits     │         │
```

Start from a clean local workspace and select Harbor Stock’s ordinary files. Let the workbench finish without touching the controls. Both reports should already be saved. Trace one Harbor Stock dependency to its source, open the action it triggers and show what changes when you add one scheduler CSV. Then run Cedar Permits and open the portable report. The final handoff includes actual test results, fixture limits and the reuse guide. [Named acceptance cases](../plans/05-validation-and-demo.md#named-acceptance-cases)
