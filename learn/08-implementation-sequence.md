# Plan 06 · Implementation sequence

[Implementation plan](../plans/06-implementation-sequence.md)

## How the build fits together

Build a small complete path first. The first **slice** takes one job to the program it names, finds a missing copybook and carries the evidence through to a recommendation in a JSON report. It shows that the parts communicate before the team expands format coverage.

```text
One JCL statement names a program
               │
               ▼
Resolve it and keep the source lines
               │
               ▼
Report one missing COPY target
               │
               ▼
Show a gap and an action
               │
               ▼
Export all of it from the same run
```

## What can be built in parallel

```text
P0  Decide the permitted execution environment ─┐
                                                ├─► P2  Intake and store
P1  Design original fixtures and test key ──────┘           │
                                                             ▼
                                                     P3  First slice
                                                             │
                                              ┌──────────────┼─────────┐
                                              ▼              ▼         ▼
                                          P4 Parsers    P5 Rules    P6 UI
                                          and impact    and review  and report
                                              └──────────────┼─────────┘
                                                             ▼
                                                     P7 Test and demo
```

P0 and the first fixture cases open the way for executable intake work. Fixture and contract design can begin while the environment is undecided. Rule and UI design can begin from agreed records and sample output. Their full implementation integrates with P4 as the parser and graph contracts stabilize. At the same acceptance endpoint the team records whether the optional model can run in the permitted environment.

## What gets handed from one part to the next

```text
Intake          → Generated manifest, artifact status and source map
Extractors      → Definitions, references and diagnostics
Resolver/graph  → Scoped links, evidence paths and caveats
Assessment      → Signals, checks, gates, findings and actions
UI/export       → Same report data for display and handoff
Validation      → Expected outcomes and test results
```

Each line describes a **contract**, an agreed shape and meaning for data. For instance, the UI can show an ambiguous relationship as soon as it knows how the engine represents candidates. It does not need to interpret COBOL itself. One small report example gives teams a common result to build against.

## How a ticket gets to done

```text
Ticket: step-scoped DD bindings and typed impact
     │
     ├─ Needs: JCL and program extraction plus scoped resolution
     ├─ Delivers: reader/writer edges and evidence paths
     └─ Check: one program used in two steps with different datasets
                    │
                    ▼
         Both bindings stay independent
```

The plan gives each ticket a dependency, output and merge check. Before a milestone, the team runs the normal entry point and reopens one source-linked finding in the export. The target for a short event is two readable application results with explicit gaps. Boundary exploration and comparison use that same evidence path and are checked at the single delivery endpoint. The optional model has its own eligibility check.

## How another application or analyzer joins

```text
New application                  New supported artifact type
      │                                    │
Select the resource files        Declare coverage and parser rules
      │                                    │
Use the existing workflow         Add positive and negative fixtures
      │                                    │
Open saved reports                Emit the existing record types
```

The reuse handoff shows local setup, the file picker, supported inputs and how to open a generated result. Optional settings and annotations are explained with the review tools. Cedar Permits demonstrates application reuse. A small new analyzer or rule demonstrates extension through the standard evidence and report contracts. [Implementation tickets](../plans/06-implementation-sequence.md#implementation-tickets-and-integration-tests)

## How different entry points share the result

```text
Local CLI ─┐
           ├─► Same analysis and query operations
Local UI ──┘                  │
                              ▼
                Saved run + selected review revision
                    │           │           │
                    ▼           ▼           ▼
                  Screen       HTML        JSON
```

One run can be inspected from the command line or the UI. Both use the same scopes, evidence records and review rules. The approved host determines how the UI reaches the local operations. This lets the team develop the engine and report contract before choosing a browser adapter. [Local operation contracts](../plans/01-core-analysis-engine.md#application-query-boundary)
