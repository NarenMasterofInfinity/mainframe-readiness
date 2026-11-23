# Plan 02 · Ingestion and evidence

[Implementation plan](../plans/02-ingestion-and-evidence.md)

## How the tool works

An **artifact** is an input file or a supplied record. Harbor Stock's artifacts might include a COBOL program, instructions for running it and a text export of the scheduler. Start with a local folder or ZIP and a short description of the application. That description sets the **scope**, which tells the workbench which parts of the application the team is assessing.

| Input | What it helps explain |
|---|---|
| COBOL programs and copybooks | Programs contain instructions. Copybooks provide shared source definitions, such as the layout of an inventory record. |
| JCL jobs, steps and DD statements | Job Control Language describes what to run. A job contains steps, and a DD statement can identify the dataset a step supplies to a program. |
| SQL, CICS definitions and screen maps | Where supported, these help link programs to data tables, online transactions and screens. |
| Scheduling, recovery, security and interface evidence | These describe how people or systems operate the application. The team can supply structured evidence linked to source documents for review. |

## From files to a report

```text
1  Open local ZIP or folder
             ↓
2  List files and confirm type, scope and expected evidence
             ↓
3  Read supported statements and keep original locations
             ├──→ Record partial, failed or unsupported inputs
             ↓
4  Connect jobs, programs, screens and data
             ├──→ Record missing, dynamic or ambiguous targets
             ↓
5  Calculate three assessments and identify next actions
             ↓
6  Inspect evidence → record a review → export the report
```

The workbench first makes an **inventory**, a list of the files it received. It then tries to extract information from each file. These steps answer different questions. A scheduler file can appear in the inventory even when the workbench cannot interpret its format. The reviewer can attach an excerpt and record what it supports. A missing scheduler file appears as a gap in expected evidence. Each outcome leads to a different action.

An analyzer of JCL can find that a step names the program `POSTSTK`. A resolver then looks for that program in the supplied scope. If one eligible definition matches, the connection can point to both source files. If two definitions match at the same priority, the workbench lists both candidates and asks the analyst to confirm which library applies.

## Find the result you need

- **Overview** shows the three results, which questions affect planning and where to start.
- **Inventory** shows which inputs were analyzed and which need attention.
- **Dependencies** shows how supplied components connect. The same information is available in a searchable table.
- **Findings and recommendations** show the source, interpretation, unanswered question and next action.
- **Export** produces a local HTML report for reading and JSON for processing. Both carry the run's scope, rule version and evidence references.

A **run** captures one set of inputs and settings. New evidence creates a new run, so a reviewer can reopen the earlier assessment and see why it reached its original result. Cedar Permits uses the same steps, starting from an online transaction and its program or screen instead of an overnight job. [Ingestion plan](../plans/02-ingestion-and-evidence.md)

## A manifest describes the export

A **manifest** is a small settings file. The workbench can also collect these settings from the intake screen. For Harbor Stock, the settings say which folders are in scope, where to look for copybooks, what encoding to use and which kinds of evidence the team expects.

```text
LOCAL PACKAGE                        EFFECTIVE MANIFEST

src/POSTSTK.cbl      ─┐              Application: Harbor Stock
copy/STOCKREC.cpy     │              Workload: batch
jobs/STOCKRUN.jcl     ├─→ Preview ←─  Search: APP, then COMMON
evidence/runbook.txt ─┘              Scheduler evidence: required
                                            ↑
                                     Defaults + analyst choices
```

Intake first checks whether the bundle can be admitted, then reads the files within the permitted limits. Preview shows what the workbench thinks each file is and why. For example, an extensionless member with a clear JCL signature can be recognized. A conflicting extension and source signature prompts a correction. Nested archives stay in the inventory as unsupported.

```text
Bundle fails admission             → Reject with a diagnostic
Bundle passes admission             → Inventory each file
   ├─ Supported and readable        → Analyze the supported syntax
   ├─ Some statements unsupported   → Keep supported facts and gaps
   ├─ Read or decoding fails        → Record the failure
   └─ Unsupported or excluded      → Keep the status and reason
```

An expected export can also be missing from the package entirely. The inventory answers “what did we receive?” The coverage view answers “what can we use to assess this scope?”

## A parser can tidy a statement and keep its original location

An analyzer reads the supported statements in one artifact. A **source map** connects its cleaned-up text to the lines in the original file. The numbers below are illustrative.

```text
PHYSICAL SOURCE           ANALYZER VIEW           EVIDENCE LOCATION

Line 10: statement starts
                          → one logical statement → original lines 10–11
Line 11: continuation

Line 12: comment          → ignored as code       → no dependency
```

This lets an analyst inspect both lines of a continued call. It also keeps a program name inside a comment from appearing as a dependency. An unsupported continuation or an ambiguous fixed/free format is a coverage issue. The tool can still report the other independently supported statements.

## Non-code evidence follows a second path

A typed evidence record says what kind of claim a document supports, which subject it covers and where to look. An analyst can say that a schedule excerpt concerns `STOCKRUN` and `STOCKRPT`, then attach the actual line. A subject matter expert checks whether the rule applies.

```text
Schedule excerpt           Typed declaration               Review

File + original line  →   Jobs + success condition   →   Check scope,
                          + source locator               meaning and date
                                                               │
                                          Usable / incomplete / disputed
```

The same pattern works for recovery procedures, data ownership, access and audit rules, interface contracts and evidence of baseline tests. Two conflicting statements stay visible to the reviewer. A file with no relevant claim increases the inventory count while the targeted question remains open.

## A run and its files have separate states

```text
Created → Inventorying → Analyzing → Resolving → Assessing → Completed
                            │
                            ├─ A file is partial → record it and continue
                            └─ Run stops         → failed or cancelled
```

A completed run can contain partial files and unknown results. A failed or cancelled run retains diagnostics and has no normal completed report. A retry creates a linked run. [Core run lifecycle](../plans/01-core-analysis-engine.md)
