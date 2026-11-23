# External dependencies

A **dependency** in this guide is something outside the project that it would use to build, run or demonstrate the workbench. That includes a parser, framework, build tool, language runtime or sample application. It also includes the host environment when the license restriction applies to that environment. This meaning differs from the job and data connections discussed in the dependency graph guide.

The project excludes **all third-party licensed items**, including software and sample applications offered under open-source licenses. The plan calls for original analyzers, UI assets, documentation and demo fixtures. The execution environment is still undecided. Existing installation by itself does not settle whether a tool can be used.

## What the plan depends on

| Item | Current position | What to establish |
|---|---|---|
| Host operating system, language runtime, build tools and browser | The execution boundary is unresolved. | Record the exact environment allowed for development, packaging and the demo. |
| React, Streamlit, graph libraries and external parsers | Excluded under the current restriction. | Use original parser logic and UI assets in the permitted environment. |
| Local storage | SQLite remains a candidate. The project also has a local file storage design. | Identify the exact SQLite distribution, any build tooling or bundled components and whether they fit the boundary. SQLite describes the deliverable library as public domain and distinguishes it from some licensed build scripts. [SQLite's copyright statement](https://www.sqlite.org/copyright.html) |
| Third-party sample applications and copied material | Excluded. | Create and review the original Harbor Stock and Cedar Permits fixtures, including source, schedules and report examples. |
| Tachyon client, endpoint and model | Conditional. It is optional for the assessment. | Confirm the client, model and execution environment fit the restriction. Establish whether the specified endpoint can run within the network boundary. |
| Commercial references reviewed during planning | Background research. | Use original code, copy, visuals and examples in the deliverable. |

## What the offline requirement means

```text
WITHIN THE PERMITTED LOCAL ENVIRONMENT

  Application export  →  Assessment engine  →  HTML and JSON
                             │
                             └─→ Local UI and evidence view


OUTBOUND SERVICES

  Remote downloads, external assets, telemetry and a remote
  model endpoint are outside the deterministic workflow.
```

An offline report opens locally with the information it needs in the file. The assessment engine and local UI should also complete the workflow with outbound access blocked after setup. The validation plan calls for observing attempted requests as well as successful ones. A remotely hosted Tachyon endpoint would need an explicit change to the network requirement before use.

The team can continue original fixture authoring, rubric design and UI specification while it resolves the execution environment. That decision is the first work package for a runnable build. The storage choice and optional model can be evaluated separately. [Implementation sequence](../plans/06-implementation-sequence.md) · [Offline validation](../plans/05-validation-and-demo.md)
