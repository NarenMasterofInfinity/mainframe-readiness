# Plan 02 · Ingestion and evidence

[Implementation plan](../plans/02-ingestion-and-evidence.md)

## How the tool works

An **artifact** is an input file or a supplied record. Harbor Stock's artifacts might include a COBOL program, instructions for running it and a text export of the scheduler. Select loose files, a local folder or a ZIP. The **scope** of the first report is the material you selected. Whether those files cover the whole business application is a question the report can leave open.

| Input | What it helps explain |
|---|---|
| COBOL programs and copybooks | Programs contain instructions. Copybooks provide shared source definitions, such as the layout of an inventory record. |
| JCL jobs, steps and DD statements | Job Control Language describes what to run. A job contains steps, and a DD statement can identify the dataset a step supplies to a program. |
| SQL, CICS definitions and screen maps | Where supported, these help link programs to data tables, online transactions and screens. |
| Scheduling, recovery, security and interface evidence | These describe how people or systems operate the application. The team can supply structured evidence linked to source documents for review. |

## From files to a report

```text
1  Select files, a folder or a ZIP
             ↓
2  Workbench lists the files and detects type and workload
             ↓
3  Read supported statements and keep original locations
             ├──→ Record partial, failed or unsupported inputs
             ↓
4  Connect jobs, programs, screens and data
             ├──→ Record missing, dynamic or ambiguous targets
             ↓
5  Calculate three assessments and identify next actions
             ↓
6  Open the results with the HTML and JSON already saved
```

The workbench first makes an **inventory**, a list of the files it received. It then tries to extract information from each file. These steps answer different questions. A scheduler file can appear in the inventory even when the workbench cannot interpret its format. The reviewer can attach an excerpt and record what it supports. A missing scheduler file appears as a gap in expected evidence. Each outcome leads to a different action.

An analyzer of JCL can find that a step names the program `POSTSTK`. A resolver then looks for that program in the supplied scope. If one eligible definition matches, the connection can point to both source files. If two definitions match at the same priority, the workbench lists both candidates in the result with a follow-up question about library order.

## Find the result you need

- **Overview** shows the three results, which questions affect planning and where to start.
- **Inventory** shows which inputs were analyzed and which need attention.
- **Dependencies** shows how supplied components connect. The same information is available in a searchable table.
- **Findings and recommendations** show the source, interpretation, unanswered question and next action.
- **Export** produces a local HTML report for reading and JSON for processing. Both carry the run's scope, rule version and evidence references.

A **run** captures one set of inputs and settings. New evidence creates a new run, so a reviewer can reopen the earlier assessment and see why it reached its original result. Cedar Permits uses the same steps, starting from an online transaction and its program or screen instead of an overnight job. [Ingestion plan](../plans/02-ingestion-and-evidence.md)

## Where the settings come from

A **manifest** is the workbench's record of what it analyzed. You can read it afterwards. The workbench fills it from the selection and its built-in defaults. Here is what it can learn from Harbor Stock.

```text
What you give it             What it records
jobs/STOCKRUN.jcl        →   The file declares a job
src/POSTSTK.cbl          →   The job names this program
copy/STOCKREC.cpy        →   A local copybook definition is available
Those three files        →   Scope is the submitted export
No owner record          →   Business ownership remains unknown
```

An extensionless job with clear control records can still be recognized. When a filename and its contents disagree, the report lists the uncertainty. An expert can correct the type and reassess later.

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

A schedule table can arrive just as the batch team exported it. A supported reader locates the job names and the condition in the row. The reviewer can open that row directly. For a runbook written in prose, the workbench can instead point to a paragraph that mentions the job and invite someone to check its meaning.

```text
Scheduler CSV row  →  STOCKRUN before STOCKRPT on success  →  Source row
Runbook paragraph  →  Location that may explain restart    →  Review lead
Unrelated memo     →  Inventory entry                      →  Gap stays open
```

A typed declaration is an optional way for a reviewer to explain an unfamiliar source after seeing a gap. The first report needs only the available resources.

The same pattern works for recovery procedures, data ownership, access and audit rules, interface contracts and evidence of baseline tests. Two conflicting statements stay visible to the reviewer. A file with no relevant claim increases the inventory count while the targeted question remains open.

## A run and its files have separate states

```text
Created → Inventorying → Analyzing → Resolving → Assessing → Publishing → Completed
                            │
                            ├─ A file is partial → record it and continue
                            └─ Run stops         → failed or cancelled
```

A completed run can contain partial files and unknown results. A failed or cancelled run retains diagnostics and has no normal completed report. A retry creates a linked run. [Core run lifecycle](../plans/01-core-analysis-engine.md)
