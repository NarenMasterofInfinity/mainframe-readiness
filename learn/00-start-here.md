# Plan 00 · Product scope and roadmap

[Implementation plan](../plans/00-product-scope-and-roadmap.md)

## Start here

Imagine a warehouse team preparing to move its inventory system. A nightly program writes the day's stock totals. A report program reads those totals. Before planning the move, the team needs to know what links the programs, when the report starts and what happens if the overnight job stops halfway through.

Select the files the team already has. The assessment starts as soon as the selection is committed. It reads a local application export, finds the pieces and their connections, and shows which planning questions need more evidence. Detailed migration planning begins with a review of those results. Moving or rewriting the application is a separate project.

```text
WHAT THE TEAM BRINGS           WHAT THE WORKBENCH RETURNS

Programs and job instructions  →  A map of programs, jobs and data
Schedules and runbooks         →  Findings with links to their sources
Any other resource files      →  Complexity, confidence and readiness
                                 + a list of next actions
```

## The example we will follow

**Harbor Stock** is an original, fictional application for the proposed demo. Its job `STOCKRUN` produces an inventory dataset called `STOCK.OUT`. Another job, `STOCKRPT`, reads it to produce a report. The first export is missing schedule and recovery evidence. The example follows the team as it investigates the handoff and learns what remains uncertain.

**Cedar Permits** is the second proposed demo. A person looks up or updates a permit through an online transaction. That gives the same tool a different starting point: a screen, a program and a database table. Both examples are teaching scenarios. File excerpts, names and results in these guides illustrate the design.

## Who uses the workbench

A **subject matter expert**, or SME, is someone familiar with the behavior being reviewed. A batch operations SME can confirm restart procedures. A business SME can explain what counts as an accepted permit update.

```text
Analyst      → Selects the local resource files
                    │
                    ▼
Domain SME   → Checks a finding against files and experience
                    │
                    ▼
Architect    → Investigates dependencies, risk and possible boundaries
                    │
                    ▼
Planning lead → Reviews the open gates and decides whether to plan
```

A reviewer can inspect the same source as the analyst. The resulting report gives them a shared record of what they supplied, what they found and what still needs an answer.

## What the end-to-end delivery includes

```text
REQUIRED
  Local intake → Evidence and graph → Impact
               → Three assessments → Hotspots and gap actions
               → Automatic HTML and JSON → Reuse on a second app

INCLUDED IN THE WALKTHROUGH
  Add evidence → Reassess → Compare the results
  Show contradictions and candidate planning considerations
  Inspect a boundary and compare the two application scopes

CONDITIONAL
  One optional model brief if the execution boundary permits it
```

The delivery includes a real reassessment and a comparison of the evidence on both runs. The workbench operates on a local application export, one active run at a time. If the team records OpenShift as a target, that is a planning assumption. [End-to-end acceptance checklist](../todo.md#the-single-acceptance-endpoint)

## Match every plan to a guide

Start with [one selection to a saved result](09-files-to-results.md) for the complete user journey. The guides explain the proposed workbench. Each opens with the name of its implementation plan.

| Plan | Illustrated guide |
|---|---|
| 00 Product scope and roadmap | This page and [external dependencies](external-dependencies.md) |
| 01 Core analysis engine | [Evidence and graph](03-evidence-dependencies-and-gaps.md) and [optional model brief](04-what-the-llm-adds.md) |
| 02 Ingestion and evidence | [How the tool works](01-how-the-tool-works.md) |
| 03 Assessments and recommendations | [The three assessments](02-understanding-the-three-assessments.md) and [from assessment to action](05-from-assessment-to-action.md) |
| 04 UI and user journeys | [A walkthrough of the workbench](06-ui-walkthrough.md) |
| 05 Validation and demo | [How we test and demonstrate it](07-validation-and-demo.md) |
| 06 Implementation sequence | [How the build fits together](08-implementation-sequence.md) |
| 07 Files to results | [One selection to a saved result and lessons from existing tools](09-files-to-results.md) |
