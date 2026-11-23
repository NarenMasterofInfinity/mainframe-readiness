# Product behavior to extend

This checklist defines the required product changes for the single end-to-end delivery. Tachyon is a core part of the assessment and starts automatically from the resources the user selects.

## Use all three Tachyon call modes

| Mode | When it runs | What the user sees |
|---|---|---|
| Queued | Automatically after the engine collects evidence for an initial assessment or a reassessment. Queue explanations and evidence-specific actions for the run, including the top finding when no results screen is open. | The current stage and whether the analysis is queued, running or ready. The user can inspect available findings while the work continues. |
| Streaming | Automatically when the engine has enough evidence to explain an important finding and the user is on the results screen. Stream the explanation in place as it becomes available. | A clearly labeled draft followed by the validated explanation and its evidence links. The saved assessment uses the validated text. |
| Synchronous | When the user follows a focused question such as **Why does the job order matter?** or **Which file should I ask for?** Send only the relevant finding and evidence. | A short answer in context, with a source link or an explicit unknown. |

- [ ] **Start the model automatically.** File selection starts the whole assessment. The workbench chooses the call modes. The initial result includes a plain-language explanation of the top finding and guidance on the next action.
- [ ] **Ground model output in the run's evidence.** Pin each request to the selected run and revision. Check citations and distinguish supported observations, interpretations and unknowns. The assessment engine owns dependency links, scores and readiness gates.
- [ ] **Keep progress and recovery visible.** Distinguish queued, running, validating, ready and failed. On a timeout, validation failure or client error, show which part failed, retain the engine findings and offer **Retry explanation** for that part. A failed required explanation leaves the assessment incomplete. A failed follow-up question shows an error beside that question. Apply responses only to the run and revision that requested them. Use one validated explanation per finding across the screen and saved reports.

## Make the next action clear

- [ ] **Lead with one plain-language next step.** Put it beside the main finding. State what to get or do, who to ask, why it matters and what the workbench will do with the result. When the evidence supports moving into detailed planning, identify that as the next step and provide the saved report.
- [ ] **Explain actions before the user takes them.** Name the expected input and outcome next to the control. Use labels such as **Add scheduler file and check again**, **See the source for this finding** and **Open saved report**. Make investigation available after the main next step is visible.
- [ ] **Show what changed after each action.** After adding evidence, say which question the new material answers and what still needs attention. If a file is irrelevant or unsupported, say what would answer the question and who can supply it.

### Example result for Harbor Stock

> **Get the schedule for STOCKRUN and STOCKRPT.** Ask the batch operations team for a scheduler export showing whether STOCKRPT waits for STOCKRUN to succeed.
>
> **Add scheduler file and check again**
>
> The workbench will read the file, run the assessment again and show what changed.

Once the schedule is supported, the next action can read **Ask batch operations how to restart after a partial write to STOCK.OUT.** Keep the schedule explanation and source available for inspection.

## The single acceptance endpoint

- [ ] Select only Harbor Stock's resource files. Observe the queued model work and the streamed explanation without a separate start action. Open the completed assessment and saved reports with the model guidance included.
- [ ] Give the result to someone unfamiliar with the application. Ask them to identify what to do next, whom to ask, what to supply and what will happen when they use the labeled action. Verify a focused question returns a synchronous, source-linked answer.
- [ ] Add the scheduler export. Verify the new run generates updated model guidance automatically, explains what the file established and points to the remaining recovery question.
- [ ] Assess Cedar Permits through the same journey. Exercise a model timeout or citation failure and confirm the displayed status and retry path. Complete this walkthrough with Tachyon operating within the project's existing execution constraints. [Dependency register](learn/external-dependencies.md)
