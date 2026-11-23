# From assessment to action

A recommendation turns an unanswered question into something a person can do. In Harbor Stock, the workbench finds that the overnight build can supply data to the report. The initial export leaves the schedule and recovery behavior open. The next action is to ask for specific evidence.

| Part of the action | Harbor Stock example |
|---|---|
| Trigger | The build writes `STOCK.OUT` and the report reads it. Schedule evidence is missing. |
| Why it matters | A migration plan needs to account for when the report can safely read the output. |
| What to request | Obtain the schedule rule for both jobs and ask how restart works after a partial write. |
| Suggested owner | Batch operations SME. |
| Completion evidence | A source-linked schedule rule and a reviewed description of restart and reconciliation. |
| Expected effect | The inputs may improve batch confidence and help satisfy the operations readiness gate. |

## Follow the evidence through a second run

```text
FIRST RUN
  Static file handoff found
  Schedule and recovery still unknown
           │
           ▼
  Add scheduler evidence and run again
           │
           ▼
SECOND RUN
  Same static file handoff
  Job order can be reviewed
  Recovery still unknown
           │
           ▼
  Ask how partial output is handled on restart
```

The second run can link to an order rule once the analyst supplies usable material. An SME can then review what the rule says. The confidence profile gives the team credit for stronger evidence where the rubric supports it. The batch band can stay **Moderate, provisional**, and the operations gate can stay **Unknown** while recovery is unresolved. Marking a task complete only records its disposition. The gate is evaluated using the evidence and review it requires.

## Evidence requests and design choices

| Situation | Next step |
|---|---|
| A required copybook cannot be found | Obtain the exact member and rerun. Check whether the target resolves. |
| A scheduler export is present in an unsupported format | Attach a supported export or link a relevant excerpt for a named reviewer. |
| Two critical flows write the same store | Ask the data owner to confirm ownership and consistency requirements. Compare candidate boundaries once those are understood. |

A design option should say what it aims to preserve and what remains uncertain. The team might preserve Harbor Stock's file handoff initially, or investigate a different handoff once order and recovery are understood. The assessment supplies the evidence and questions for that choice.

To hand off the work, export the current run and review revision. Use the HTML report for a walkthrough and the JSON file for further analysis. Share the scope, critical gaps, action owners and evidence locations so another reviewer can continue from the same decisions. [Recommendations plan](../plans/03-assessments-and-recommendations.md)
