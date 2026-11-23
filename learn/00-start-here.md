# Start here

Imagine a warehouse team preparing to move its inventory system. A nightly program writes the day's stock totals. A report program reads those totals. Before planning the move, the team needs to know what links the programs, when the report starts and what happens if the overnight job stops halfway through.

The assessment workbench helps a team answer those questions from material it already has. It reads a local application export, finds the pieces and their connections, and shows which planning questions need more evidence. Detailed migration planning begins with a review of those results. Moving or rewriting the application is a separate project.

```text
WHAT THE TEAM BRINGS           WHAT THE WORKBENCH RETURNS

Programs and job instructions  →  A map of programs, jobs and data
Schedules and runbooks         →  Findings with links to their sources
A description of the scope     →  Complexity, confidence and readiness
                                 + a list of next actions
```

## The example we will follow

**Harbor Stock** is an original, fictional application for the proposed demo. Its job `STOCKRUN` produces an inventory dataset called `STOCK.OUT`. Another job, `STOCKRPT`, reads it to produce a report. The first export is missing schedule and recovery evidence. The example follows the team as it investigates the handoff and learns what remains uncertain.

**Cedar Permits** is the second proposed demo. A person looks up or updates a permit through an online transaction. That gives the same tool a different starting point: a screen, a program and a database table. Both examples are teaching scenarios. File excerpts, names and results in these guides illustrate the design.

## Choose a starting point

| If you want to understand… | Read… |
|---|---|
| Which files go in and what comes out | [How the tool works](01-how-the-tool-works.md) |
| Why a complex system can still be ready for planning | [The three assessments](02-understanding-the-three-assessments.md) |
| How to check a connection against the original files | [Evidence, dependencies and gaps](03-evidence-dependencies-and-gaps.md) |
| What a model contributes and what a reviewer checks | [What the LLM adds](04-what-the-llm-adds.md) |
| How to act on a finding and reassess | [From assessment to action](05-from-assessment-to-action.md) |
| Which technologies and materials can be used | [External dependencies](external-dependencies.md) |

A **subject matter expert**, or SME, is someone familiar with the behavior being reviewed. A batch operations SME can confirm restart procedures. A business SME can explain what counts as an accepted permit update. The person reviewing the evidence depends on the question.

The guides describe the proposed workbench. For build order and acceptance criteria, see the [implementation sequence](../plans/06-implementation-sequence.md).
