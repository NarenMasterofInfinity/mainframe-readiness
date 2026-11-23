# Understanding the three assessments

Harbor Stock writes a file that a report consumes. The team can see a dependency in the supplied code while still being unsure when the reporting job runs. The workbench asks three separate questions about this situation.

```text
                    EVIDENCE IN THIS RUN
           ┌─────────────────┼─────────────────┐
           ▼                 ▼                 ▼
       COMPLEXITY        CONFIDENCE         READINESS
       What makes        How strong is      Can detailed
       migration         our evidence?      planning begin?
       harder?

       Plan around       Collect or         Resolve open
       constraints       review evidence    planning questions
```

Each result uses the same evidence for a different decision. Readiness uses explicit planning requirements, called **gates**. It does not follow automatically from the confidence band.

## Complexity describes observed challenges

The tool looks for signals in code, data, batch work, transactions and screens, integrations, security and operations. A **signal** is an observed characteristic that the rubric explains. A writer and a reader linked to the same dataset are a batch signal because the migration plan must account for how the file reaches the reader.

For Harbor Stock, assume that this is the strongest known batch signal. The proposed rubric gives it **Moderate, provisional** while schedule or recovery evidence is incomplete. **Provisional** means the evidence already supports a result that may change with further investigation. **Unknown** applies when prerequisites are incomplete and available evidence supports no qualifying signal. **Low** requires sufficient reviewed coverage of the dimension. **Not applicable** requires a reviewed reason, such as an agreed batch-only scope for the transactions and screens dimension.

The overall complexity band uses the strongest demonstrated dimension. A material gap in any applicable dimension keeps the aggregate provisional. The bands help teams compare concerns. They do not estimate weeks or cost.

## Confidence describes the support for a result

The tool asks four questions for each applicable dimension. Are the required materials available? Is the relevant extraction or manual evidence usable? Have material references been resolved? Has a suitable person reviewed the interpretation?

Suppose a data dimension has usable artifacts and extraction, its material references are resolved and the independent review is pending. That is **3 of 4 checks passed**, which gives **Moderate** under the proposed rubric. A documented review can make it **4 of 4**, or **High**, if the interpretation and evidence hold. A reviewed inapplicable check leaves the denominator. An unknown check remains in the denominator and does not count as a pass. When usable evidence is absent or a denominator cannot be established, the dimension shows **Unknown**. A file count can describe the export. It cannot show the fraction of the entire estate that was supplied.

## Readiness answers the planning question

```text
Did an applicable gate fail?
  ├─ Yes → Not ready
  └─ No  → Is an applicable gate still unknown?
             ├─ Yes → More evidence required
             └─ No  → Ready for detailed planning
                       when all applicable gates pass
```

Gates cover scope, dependencies, business and data, operations, security and interfaces, and review completion. If the schedule and recovery procedure are missing in Harbor Stock, the operations gate is **Unknown**. If there is no confirmed failure in another gate, the overall result is **More evidence required**. A reviewed schedule can answer the order question while the missing recovery procedure keeps that gate open.

A complex system can be ready for detailed planning once the relevant evidence and reviews are in place. A quiet-looking graph may still leave a team without enough information to plan. [Assessment rubric](../plans/03-assessments-and-recommendations.md)
