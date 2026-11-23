# Plan 01 · Core analysis engine

[Implementation plan](../plans/01-core-analysis-engine.md)

## Evidence, dependencies and gaps

A **dependency** connects two pieces of the application. **Evidence** is the file, location or record that supports the connection. In Harbor Stock, the useful connection is that one job can produce the data another job needs. To see why we believe that, start with the smaller statements that support it.

## Trace the Harbor Stock handoff

```text
STOCKRUN / BUILD step                  STOCKRPT / REPORT step
    │ names POSTSTK                        │ names RPTSTK
    │ binds DD STOCKDD to STOCK.OUT        │ binds DD REPORTDD to STOCK.OUT
    ▼                                      ▼
POSTSTK                                RPTSTK
    │ writes through STOCKDD              ▲ reads through REPORTDD
    │                                      │
    └─────────────► STOCK.OUT ──────────────┘

Arrows around STOCK.OUT show the direction of data.
The two step-to-program links show what the supplied JCL names.
```

The program side tells us which named file it uses and what it does with it. The JCL side tells us which physical dataset that name points to in this particular step. In this example, the workbench combines the program's assignment and write operation with the step's DD statement to derive the write relationship. It does the same for the read. Another step could run `POSTSTK` with a different DD binding, so the evidence retains the job and step.

| Statement in the report | Supporting material | What the reviewer can conclude |
|---|---|---|
| The BUILD step names `POSTSTK`. | `jobs/STOCKRUN.jcl`, the program execution statement | The supplied step names that program. |
| The supplied configuration points to a write from BUILD to `STOCK.OUT`. | `src/POSTSTK.cbl`, the file assignment and write, plus `jobs/STOCKRUN.jcl`, the matching DD | The write is supported for that configuration. |
| `STOCKRPT` is a possible consumer of the output. | The write and read evidence together | The two jobs may share a data handoff. |
| The scheduler waits for a successful `STOCKRUN`. | A scheduler rule or relevant run evidence | This remains unknown in the first export. The source and possible handoff cannot establish scheduler behavior. |

These paths and identifiers are illustrative. In a real report, an evidence link opens the original line or row and the relevant excerpt when available. If two programs only **read** the same dataset, the workbench can show a shared resource. It has no writer-to-reader handoff to derive.

## Three labels for one connection

**Basis** tells you where the statement came from. It may be a direct source reference, a relationship derived by combining facts or a reviewer declaration. **Resolution** tells you whether the workbench could identify the target. It may be resolved, missing within the supplied scope, ambiguous or dynamic. **Review** tells you whether a person has confirmed, challenged or corrected the claim. These labels can coexist. A derived relationship can be resolved and still await review.

A finding can also state an **assumption** for the reader to check. For instance, the team may assume the supplied JCL represents the production setup. That assumption remains visible until someone confirms the scope. The workbench can still verify what the supplied JCL says.

A **gap** records what the team could not establish. If a referenced copybook is absent, ask for the correct library member. If the file is present but unsupported, ask for a usable export or SME interpretation. If two definitions fit, ask which library applies. If a program name is supplied dynamically, ask for configuration or relevant runtime evidence.

Use the graph to follow a path and the table to search or compare connections. Check the scope and original evidence before treating a connection as a planning assumption. [Core evidence model](../plans/01-core-analysis-engine.md)

## How the engine connects records

```text
JCL analyzer                        COBOL analyzer
  "BUILD names POSTSTK"               "POSTSTK is defined in APP"
  + source line                       + definition line
           \                              /
            └───────── Resolver ─────────┘
                           │
                           ▼
           BUILD ── executes ──► APP/POSTSTK
           Both original locations remain available
```

Each analyzer emits the same kinds of records, such as a component definition, a reference and a source location. The resolver checks the expected component type and applicable library search order. One eligible definition resolves the target. Two definitions at the same priority yield an ambiguous reference with both candidates visible. An identifier whose value is assigned at runtime can have hints and still remain dynamic.

```text
Reference to a program
       │
       ├─ One eligible definition wins    → Resolved target
       ├─ Multiple equally eligible       → Ambiguous, list candidates
       ├─ No eligible definition          → Missing in supplied scope
       ├─ Explicit external declaration   → External, confirm contract
       └─ Target computed at runtime      → Dynamic, retain any hints
```

A name is scoped by its type and context. A program is identified with its library. Two steps can use the same program with different DD bindings. The tool keeps repeated source occurrences even when the graph draws a single relationship. A changed file cannot silently replace evidence from an earlier run. The viewer opens the retained snapshot or verifies the file fingerprint before showing the original span.

## Change Impact Explorer

Choose a graph item and a question. **Dependencies** follows what the item uses. **Dependents** follows the links backward to see what may rely on it. **Data handoff** follows a supported writer-to-reader connection, separately from schedule order.

```text
STOCKRPT / REPORT  ── reads ──► STOCK.OUT ◄── writes ──  STOCKRUN / BUILD

Choose STOCK.OUT, then choose Dependents and filter to Reads.
  STOCK.OUT  ── follow read backward ──► REPORT step
                                               │
                                      Owning job: STOCKRPT
```

The step is a direct dependent. The owning job is an entry point associated with the result, presented separately from the path. If a program includes a copybook and another program calls it, following the edges backward can show the first program at one hop and its caller at two.

```text
Root                 Depth 1                 Depth 2
Copybook    ← includes ── Program A  ← calls ── Program B
                           Direct                Indirect
```

The engine counts distinct items and keeps the edge evidence for each shown path. A loop cannot inflate the count. Missing and dynamic targets appear as caveats. The screen can show a smaller list than the query explored, and the query itself can stop at its depth or visit limit. If the search was capped, read its count as a lower bound within the explored scope.

## What a boundary candidate shows

Boundary exploration is a stretch feature. Start from a job, transaction or reviewed business label. The tool follows supported code links for a short distance and shows a group worth investigating.

```text
           PROPOSED GROUP
       ┌────────────────────┐
       │  Seed → Program A   │ ── calls ──► Outside program
       │           │         │
       │       Program B    │ ── writes ─► Shared dataset
       └────────────────────┘                    ▲
                                     Outside writer ─┘
```

Look at the internal links, the links that cross the boundary and the data that other components still use. A shared writer, missing ownership or an unresolved target gives the reviewer a specific question. An analyst can compare an edited membership to the proposed group. The proposed group remains a planning hypothesis. [Impact and boundary algorithms](../plans/01-core-analysis-engine.md#graph-indices-and-dependency-algorithms)

## A new review is different from a new run

```text
              SAME INPUTS                     ADD OR CHANGE INPUTS

Run A, machine result ── preserved      Run A ── preserved
            │                               │
            ├─ Review revision 1            └─ Run B, linked to Run A
            └─ Review revision 2                    + fresh review state
```

A review records who checked a finding, which evidence they used and why they confirmed or challenged it. It can update the reviewed assessment while keeping the original machine result. A reviewer working from an older revision must inspect the latest revision before applying the decision. A changed package creates its own linked run. [Persistence and review behavior](../plans/01-core-analysis-engine.md#persistence-failure-and-repeatability)

## How the graph counts links

```text
Program A ─┬─ includes at line 12 ──► COPYBOOK
           └─ includes at line 48 ──► COPYBOOK
Program B ─── includes at line 30 ──► COPYBOOK

3 source occurrences  →  2 distinct programs that use the copybook
```

The evidence panel retains all three locations. The graph can show one visible link per program and copybook. A count has a scope, a method and an evidence basis. A partial count records the items the analyzer could find and leaves room for others.
