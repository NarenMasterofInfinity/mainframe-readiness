# How the tool works

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
