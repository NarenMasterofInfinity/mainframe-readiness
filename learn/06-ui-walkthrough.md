# Plan 04 · UI and user journeys

[Implementation plan](../plans/04-ui-and-user-journeys.md)

## A walkthrough of the workbench

The planned interface lets the analyst move from a planning question to the source that answers it. The graph, table, evidence panel and export all refer to the selected run and review revision. A run header shows that context on every view.

```text
Choose resources → Automatic analysis → Saved assessment
                         │                    │
                   See progress          Pick a question
                                              │
              ┌────────────┬─────────────────┼───────────┐
              ▼            ▼                 ▼           ▼
          Inventory    Dependencies     Findings/gaps   Export
                           │                 │
                           └──── Evidence ───┘
                                    │
                                  Review
```

## Read the overview

```text
┌────────────────────────────────────────────────────────────────┐
│ Harbor Stock  /  Base run  /  Review revision 0                │
│ Scope: submitted resources, business perimeter unreviewed      │
├──────────────────────┬────────────────────┬────────────────────┤
│ Complexity           │ Confidence         │ Readiness          │
│ Open signals and     │ Open applicable    │ Open critical      │
│ provisional reasons  │ checks             │ planning gates     │
├──────────────────────┴────────────────────┴────────────────────┤
│ Risk hotspots  ·  Evidence requests  ·  Domain details         │
└────────────────────────────────────────────────────────────────┘
```

This is a layout sketch. Each card opens a different explanation. Complexity opens the strongest contributing rules. Confidence opens the scope and check counts. Readiness opens the gate, affected subjects and the action that can resolve it.

## Open the evidence behind an impact path

```text
Control row:  STOCK.OUT   /   Dependents   /   Reads
                         │
        ┌────────────────┴─────────────────┐
        ▼                                  ▼
      Graph                          Searchable table
        └────────────────┬─────────────────┘
                         ▼
           REPORT reads STOCK.OUT
                         │
                         ▼
           Original DD and program spans
           Status, execution context and review
```

On a small screen or when using a keyboard, the analyst can reach the same evidence through the table. Text labels distinguish missing, ambiguous and dynamic references. The interface names the result scope and any exploration or display cap. An empty filtered result and an unsupported component get different explanations. If an excerpt is unavailable, its original locator stays visible.

## Recover from an incomplete or changing result

```text
A file type is uncertain  → Finish with a visible gap for that file
Run is still processing   → Reopen the run and restore progress
Run failed or cancelled   → Show diagnostics and offer a new run
Review revision changed   → Show the latest result before retrying
New evidence added        → Open the linked child run
```

The gap card shows the evidence needed to close the question. **Add evidence** lets you choose just the new resource and starts a child run using the base inputs. **Record review** attaches a person, rationale and evidence to the selected revision. The comparison shows changed and unchanged results side by side. The analyst can inspect a crossing edge from the candidate screen or compare this run with another application from the local application list.

## Export the same reviewed result

```text
Selected run + review revision
           │
           ▼
     One report model
           │
     ┌─────┴─────┐
     ▼           ▼
 Local HTML     JSON
 For reading    For processing
     │           │
     └─────┬─────┘
           ▼
 Scope, three results, evidence locators, gaps and review
```

HTML and JSON already exist for revision zero. When saving another copy, the export preview identifies the chosen revision and whether source excerpts are included. Excerpts are off by default. The report still carries relative paths, locations and evidence IDs. HTML opens locally and the JSON carries the structured records for analysis or another tool. [Report specification](../plans/04-ui-and-user-journeys.md#report-and-stable-export-contract)
