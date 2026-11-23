# External dependencies and the current constraint

The current instruction excludes **all third-party licensed items**. Accordingly, no third-party parser, framework, library, model, sample application or copied code is approved for the product or its demonstrations. Public availability, open-source status and free pricing do not make an item license-free.

This creates a genuine implementation blocker: software runtimes, browsers, operating systems, databases and model services typically have license terms too. We cannot honestly promise a working software tool with no third-party licensed components simply by writing our own application code. We need the user to establish what underlying environment, if any, is outside this restriction before choosing a stack or building a runnable product.

| Previously suggested item | Current status |
|---|---|
| Streamlit, React, Cytoscape.js, external parsers | Excluded; no framework or parser selected. |
| CardDemo and GenApp | Excluded. Create original synthetic fixtures, not derivative copies. |
| Python, web browser and operating system | Not approved under the literal constraint; availability does not establish permission. |
| SQLite | SQLite states its core code is public domain, but the exact distribution and bundled dependencies still need review; its status under the user’s restriction is unresolved. See [SQLite copyright](https://www.sqlite.org/copyright.html). |
| Tachyon, its client and model | Not approved; user ownership of a client does not establish model, service or runtime licensing. Endpoint location and network behavior also remain unknown. |
| Documentation and commercial product research | Background references only, not dependencies, source material to copy, or input artifacts. Verify any future use against the restriction. |

## Runtime and network decisions

1. Identify which pre-existing execution environment, if any, the user considers permitted, including runtime, operating system and browser.
2. Obtain an explicit decision on the previously requested SQLite and Tachyon preferences; neither is assumed to be an exception.
3. Select no dependency until its exact distribution, transitive components and applicable terms have been reviewed against that decision.
4. Independently verify the deterministic assessment runs with outbound access blocked. A network-accessible Tachyon endpoint would additionally need an explicit exception to the no-network requirement.

A lack of dependencies in our application source is not proof that the execution stack has no third-party licenses. Until the environment is resolved, the plans specify interfaces and behavior rather than an approved runnable stack.
