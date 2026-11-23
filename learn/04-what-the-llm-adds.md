# Plan 01 · Core analysis engine

[Implementation plan](../plans/01-core-analysis-engine.md)

## What the LLM adds

A large language model, or **LLM**, can turn a selected set of technical findings into a draft that a business or operations reviewer can use. The saved assessment and its plain-language flow summary already exist before a model is considered. An optional brief can explain a particular flow, suggest what behavior may matter and ask questions that help the team investigate.

For example, Harbor Stock's graph can show that a reporting job reads a dataset written by the nightly build. A draft can ask what the new platform should do if a batch run writes only part of the dataset. A batch SME can then check whether that situation is possible and what current recovery procedures require.

```text
                  COMPLETED ASSESSMENT
                           │
             Select one flow and its evidence
                           │
                           ▼
                   Optional model draft
                           │
                 Check format and source IDs
                    │                  │
              Check fails          Check passes
                    │                  │
           Show a draft error           ▼
                              Reviewer checks meaning
                               │         │         │
                             Confirm   Correct   Leave open
```

The workbench still provides the inventory, graph, three results and recommended actions when the model is unavailable. The model's wording has no authority over those results. A reviewer can record a supported interpretation through the same evidence review process used elsewhere.

## What a reviewer gets

| Draft item | Example | Why it helps |
|---|---|---|
| Plain-language flow | The overnight build produces stock totals that the report can consume. | A reviewer can discuss the flow without reading every source file first. |
| Candidate rule | The report may require a complete build before it runs. | An SME can check the rule against schedule and recovery evidence. |
| Modernization risk and question | Could a partial build leave a file that the report reads? | Operations can identify missing recovery evidence. |
| Candidate validation scenario | Investigate what happens if the build stops before the final write. | The team can identify what future validation would need to observe. |

A source ID lets the tool check that a cited location exists. The reviewer still checks whether the source supports the sentence. For example, a JCL DD statement can identify a file binding. It does not establish that the report waits for a successful build. If schedule evidence is missing from the selected context, the draft should leave the scheduling question open.

The proposed workflow offers one optional brief from a selected flow. Tachyon is conditional on the execution and licensing decision. The model cues in the core plan cover model size, reasoning effort, output length and a human-reviewed comparison of candidate settings. [Core engine plan](../plans/01-core-analysis-engine.md) · [External dependencies](external-dependencies.md)

## What the model can use

```text
Selected Harbor Stock flow
   ├─ Evidence locations the tool can open
   ├─ Interpreted read/write links and their basis
   └─ Known gaps, including missing schedule or recovery
                          │
                          ▼
              Draft with references and open questions
                          │
             Check citations and context, then review meaning
```

The workbench gives the model a selected flow and its relevant evidence. If part of the context had to be omitted, the draft records that limit. A draft that says “the report always waits” needs evidence for the scheduling rule. A citation to a valid file binding can pass a reference check while failing the review of that claim.

## How to choose a starting model setting

```text
Summarize one flow         ─→ Try a smaller code-capable model
                              with low reasoning, where supported
Interpret several sources ─→ Try medium reasoning and compare results
                                           │
                                           ▼
                         Check supported claims and omissions
                         before selecting settings for the demo
```

These are trial settings. The plan suggests structured output, a starting allowance of up to 1,200 output tokens and temperature 0–0.2 where supported. The team will test the available model and its parameters. A brief stays optional under the project’s licensing and network constraints. [Model selection cues](../plans/01-core-analysis-engine.md#optional-model-cues)
