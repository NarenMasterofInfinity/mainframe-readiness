# Plan 07 · Files to results

[Implementation plan](../plans/07-files-to-results.md)

## One selection to a saved result

Imagine the batch team hands you a folder and says, “These are the files we have. Can we start planning?” Open the workbench and choose the folder. You can also choose several files at once or a ZIP. You can leave the keyboard alone until the answer appears. Everything below describes the planned behavior.

```text
YOU                 WORKBENCH                         WHAT YOU GET
Choose resources ─► List the files and read them
                    Connect jobs, programs and data
                    Check support for each conclusion
                    Save both reports ──────────────► Results on screen
                                                      HTML for people
                                                      JSON for other tools
```

The workbench gives the run a name and records which files it used. That is enough to report what was found in the export. The business owner may know about another production job, a different copybook library or an undocumented restart step. The result shows those limits.

## What the first answer looks like

Suppose Harbor Stock contains the program and job statements for a nightly build and report. The following is an illustrative result for the **batch dimension**. Other dimensions have their own evidence.

```text
BUILD step writes STOCK.OUT       REPORT step reads STOCK.OUT
                └───────────┬────────────┘
                         Possible handoff
                               │
        ┌──────────────────────┼────────────────────────┐
        ▼                      ▼                        ▼
How difficult?          How well supported?        Can planning start?
Moderate, provisional   Both bindings have lines.  More evidence required.
Two jobs share data.    Scheduling is unknown.     Recovery is still open.
```

Select the handoff to see the JCL that binds each dataset and the COBOL statements that read or write it. That explains why moving the jobs independently deserves investigation. The code cannot show that the report always waits for a successful build. The next action asks the batch owner for the schedule rule covering these specific jobs.

## What a business reviewer can open

```text
A Cedar program contains a supported condition and assignment
        │
        ├─► “When this condition holds, the program sets this field.”
        │    Open the condition and assignment in the original program.
        │
        └─► Open the field dictionary to inspect its declared layout.
             A business reviewer can explain what the status means.
```

The first statement comes from the code. The reason a status exists may live in a policy or someone’s experience. The report keeps that question beside the evidence, even with the model disabled.

## When the files leave a question open

```text
A job is clear, a copybook has two possible definitions
   └─► Show the job, list both candidates and save the report.

A readable export contains only unsupported binaries
   └─► Save an inventory, unknown results and the text export needed.

A ZIP cannot be opened safely or the selection is empty
   └─► Explain what stopped intake and which selection would work.
```

Review is available after a result. You may close the app and read the saved HTML. If the report leaves an important question open, you can bring the evidence back.

## Add one file or assess another application

```text
Harbor Stock result         Add evidence → choose schedule.csv
        │                                   │
        │                           Same app, new run
        │                           Rule now documented at row 2
        └─────────────────────────► Recovery still unknown

Choose Cedar Permits        New assessment → choose its resources
                                            │
                                    Different app, same rules
                                    Transaction → program → table
```

Adding the schedule changes what we know about order. It can leave the readiness decision unchanged. Selecting Cedar separately keeps its resources, results and reviews independent. You can compare the two scopes in the local application list. [Scoring guide](02-understanding-the-three-assessments.md) · [Evidence and impact](03-evidence-dependencies-and-gaps.md)

## Where the design ideas came from

| Question the reviewer asks | Example that informed the design | How to see it in our planned demo |
|---|---|---|
| What did I send? | Google’s Mainframe Assessment Tool organizes discoveries after a source upload. | Open the inventory, including the unsupported member. |
| What happens along one flow? | AWS Transform exposes data paths. IBM Z Understand presents an application blueprint. CAST Imaging follows transaction paths. | Choose Cedar’s transaction and follow it to the program and table. Each link opens a source location. |
| Why does this deserve a planning action? | OpenText Enterprise Analyzer connects high-level investigation to program and field detail. CAST Highlight connects findings to actions. | Open a layout finding, the copybook field behind it and the requested validation. |
| How does the tool know what a name means? | ProLeap distinguishes source syntax from relationships. Che4z uses library context for copybooks. | Inspect the two copybook candidates. Their filenames alone cannot tell us which one production uses. |
| Will the same process work on another app? | Konveyor separates rules from its analysis providers. CardDemo illustrates the breadth of a mixed workload. | Select new, originally authored Cedar resources. The same analyzers and rules run unchanged. |

The source-linked research and its implementation decisions are in the plan. These projects supply ideas for how an analyst works. The shipped tool, fixtures and visuals are first-party. [Research comparison](../plans/07-files-to-results.md#what-existing-tools-teach-us-about-the-workflow) · [External dependencies](external-dependencies.md)
