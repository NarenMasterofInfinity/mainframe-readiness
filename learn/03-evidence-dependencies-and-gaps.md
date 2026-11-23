# Evidence, dependencies and gaps

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
