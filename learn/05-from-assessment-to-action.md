# Plan 03 · Assessments and recommendations

[Implementation plan](../plans/03-assessments-and-recommendations.md)

## From assessment to action

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

To hand off the initial result, open the generated reports. After a review, export the selected revision. Use the HTML report for a walkthrough and the JSON file for further analysis. Share the scope, critical gaps, action owners and evidence locations so another reviewer can continue from the same decisions. [Recommendations plan](../plans/03-assessments-and-recommendations.md)

## Why a hotspot appears near the top

A **hotspot** groups related findings around one subject or cause. Its row shows why the team should investigate it, who or what may be affected and what action is available.

```text
START WITH THE PLANNING DECISION

Failed critical gate
        ↓
Unknown critical gate
        ↓
Other material finding
        ↓
Informational item

Within a tier, compare the importance of the issue, the number of
affected critical entry points and whether evidence is unresolved.
```

The affected entry-point count comes from the typed impact query. A capped query can only give a lower bound. A widely used utility can have many links while its business importance still needs review. The ranking explains where to investigate first. It does not measure the probability of a problem.

## The Evidence Gap Planner shows how to close a question

```text
Finding: writer and reader share STOCK.OUT
                         │
                 What is still open?
                   /             \
        Job sequencing          Recovery after partial output
              │                           │
      Find applicable rule       Find restart and reconciliation
      and success condition           procedure
              │                           │
       Batch SME review          Operations SME review
                   \             /
            Re-evaluate the operations gate
```

One request can cover multiple gaps. Each gap keeps its own completion condition. An unrelated schedule document can be present in the inventory and still leave the schedule gap open. Conflicting records create a review item. Closing the action itself does not pass the gate.

## What the comparison can show

```text
BASE RUN                         CHILD RUN
No schedule supplied             Applicable schedule supplied
Recovery missing                 Recovery still missing
Handoff supported                Same handoff supported
        │                                 │
        └────────── Compare ──────────────┘
                    │
                    ├─ Changed: schedule evidence
                    ├─ Open: recovery question
                    └─ Still provisional: batch complexity
```

Both runs remain available. The richer change table can also surface a new finding or a contradiction. Compare the same application and notice if the rules, parser versions or review revisions differ. If several inputs or methods changed, the comparison labels the mixed change. An optional preview can stage a real input addition in a child run. A hypothetical file description only tells you which checks could be affected.

## From a risk to a planning option

```text
Evidence shows two critical flows writing the same store
                         │
                         ▼
      Confirm write paths, ownership and consistency needs
                    /          \
     Keep the shared store     Investigate a single owner
     in the initial phase      before separating the flows
                    \          /
         Compare what each option must preserve
```

A candidate consideration states the trigger, the behavior to preserve and the evidence still needed. The reviewer can use those details in planning. The finding or candidate does not choose the architecture or revise a readiness gate. [Hotspot, gap and candidate rules](../plans/03-assessments-and-recommendations.md#risk-hotspots-and-evidence-gap-ranking)
