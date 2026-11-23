# What the LLM adds

A large language model, or **LLM**, can turn a selected set of technical findings into a draft that a business or operations reviewer can use. Its job is to explain a particular flow, suggest what behavior may matter and ask questions that help the team investigate.

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
