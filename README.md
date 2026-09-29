# Agent 365 Labs

📖 **Read the labs at [microsoft.github.io/agent365-labs](https://microsoft.github.io/agent365-labs)**

Hands-on labs for [**Microsoft Agent 365**](https://learn.microsoft.com/microsoft-agent-365/). Start by building a Copilot Studio agent with built-in observability, prepare the SDK requirements, then register and instrument a custom .NET web agent.

The labs use a Microsoft documentation research assistant. You follow its tool calls and inspect the resulting activity in Microsoft Defender and the Microsoft 365 admin center.

Each lab is designed to be completed in a single sitting. Copilot Studio stays in browser surfaces; the SDK labs use a terminal and a browser.

## Main lab flow

Complete these steps in order. Python and JavaScript are optional extensions, not required steps or replacements for .NET in this flow.

1. [Copilot Studio - Lab A365-01](./docs/01-copilot-studio.md)
2. [SDK requirements](./docs/00-prerequisites.md)
3. [.NET - Lab A365-02A](./docs/02a-web-obo-dotnet.md)

### 1. Copilot Studio agent with the GitHub Copilot runtime harness

Start here. Create an agent in Copilot Studio using the GitHub Copilot harness. Add the Microsoft Learn MCP server and a native skill in the builder, confirm tool use in Preview, and inspect its activity. Copilot Studio emits the telemetry automatically; you do not add instrumentation.

This lab has its own tenant and Defender prerequisites. Check Activity availability in your tenant before relying on that surface.

| Lab | Runtime and authoring surface | Duration | Level |
| --- | --- | --- | --- |
| [A365-01](./docs/01-copilot-studio.md) | Copilot Studio new experience with the GitHub Copilot runtime harness | ~60-90 minutes, plus admin prework and indexing | Beginner to intermediate |

### 2. SDK requirements

After A365-01, complete the [SDK requirements](./docs/00-prerequisites.md): install the tools and Agent 365 Skills, check tenant licences and permissions, clone the .NET sample, and prepare Azure OpenAI. Then continue to the .NET lab.

### 3. .NET web app agent with user on-behalf-of

In A365-02A, a user opens a web page, signs in, and drives the agent. Everything the agent does is attributed back to that person. This is the final lab in the main flow.

| Lab | Stack | Framework and host | Duration | Level |
| --- | --- | --- | --- | --- |
| [A365-02A](./docs/02a-web-obo-dotnet.md) | .NET 8 | Agent Framework on Blazor Server | ~3 hours | Intermediate |

## Optional labs

After completing the main flow, you can explore the same Agent 365 concepts in Python or JavaScript. Neither lab is required to complete the main flow. The JavaScript ecosystem lab runs on Node.js and uses TypeScript source.

| Lab | Stack | Framework and host | Duration | Level |
| --- | --- | --- | --- | --- |
| [A365-02B - Optional](./docs/02b-web-obo-python.md) | Python 3.12 | LangChain on FastAPI | ~3 hours | Intermediate |
| [A365-02C - Optional](./docs/02c-web-obo-nodejs.md) | JavaScript / Node.js 20.10 | LangChain on Express, in TypeScript | ~3 hours | Intermediate |

> Work IQ is not yet available for Python with LangChain. Optional lab A365-02B covers Exercise 5 as an explanation of the gap instead of an implementation. The main .NET lab and optional JavaScript lab include the Microsoft 365 data access implementation.

## What the labs cover

The first lab, Copilot Studio, moves through five stages:

| Stage | What you do | What you have at the end |
| --- | --- | --- |
| 1 | Create the agent in the new experience | A Copilot Studio agent in the GitHub Copilot runtime harness, with recorded environment and bot identifiers |
| 2 | Add a real MCP tool and a native runtime skill | A Microsoft Learn tool connection and a `learn-research` skill created in the builder |
| 3 | Prove tool use in Preview | An activity trace with a real `microsoft_docs_search` call |
| 4 | Generate an authenticated same-tenant run and check Activity | Evidence for the test period in the Microsoft 365 admin center, or partial-completion notes if that surface stays incomplete |
| 5 | Hunt the traces in Defender | Matching invocation and Microsoft Learn tool events for the same conversation and time window |

After the SDK requirements, the .NET lab follows these six exercises. The optional Python and JavaScript labs follow the same structure, with the Python Work IQ limitation noted above.

| # | Exercise | What you have at the end |
| --- | --- | --- |
| 1 | Run the agent as it is | A working agent that answers questions and cites its sources, with no Agent 365 in it |
| 2 | Sign users in with Microsoft Entra | A registered sign-in application, and the code that acquires a user token |
| 3 | Register the agent with Agent 365 | An agent blueprint and identity in the tenant, and a user token addressed to the blueprint |
| 4 | Instrument the agent for observability | OpenTelemetry and the Agent 365 exporter emitting semantic spans per turn |
| 5 | Give the agent access to Microsoft 365 data | Mail, Calendar and more through the Work IQ MCP servers, as the signed-in user |
| 6 | Verify it end to end | Activity confirmed in Microsoft Defender and the Microsoft 365 admin center |

## Before you start

Check the prerequisites at the relevant point in the flow:

| Step | Start here |
| --- | --- |
| **1. Copilot Studio** | [Lab A365-01 prerequisites](./docs/01-copilot-studio.md#prerequisites) |
| **2. SDK requirements, before .NET** | [SDK requirements](./docs/00-prerequisites.md) |

| | |
| --- | --- |
| **Copilot Studio** | Copilot Studio new experience, tenant licensing and credits, Defender connector readiness, and browser access to the right environment |
| **SDK labs** | CLI, SDK, Azure OpenAI, local code, sample agent clone, and admin consent handoffs |

The .NET 8 SDK and Agent 365 CLI are required for the **SDK labs** because the CLI is distributed as a .NET global tool. **The Copilot Studio lab does not use the CLI, PAC, local code, or Azure OpenAI.** Python, uv, and Node.js are needed only for their respective optional labs.

## How the labs are written

Each lab is a sequence of **exercises**, and each exercise is a sequence of **steps**. Every exercise ends with a checkpoint stating what should be true before you move on, so you can stop between exercises and pick the lab up later.

The Copilot Studio lab creates a native skill in the product and pairs it with the Microsoft Learn MCP server. Browser steps use the same exercise, step, and checkpoint structure as the SDK labs.

The SDK labs do most of the Agent 365 work by asking an AI coding assistant to do it for you, using the [Agent 365 Skills](https://github.com/microsoft/agent365-skills). Those steps show four things:

1. **What you type**, the prompt you give your coding assistant
2. **What the skill does**, the changes it makes
3. **Behind the scenes**, the CLI command or code underneath
4. **How to verify**, how to confirm it worked before moving on

## Repository structure

| Path | Contents |
| --- | --- |
| [`docs/index.md`](./docs/index.md) | Landing page of the documentation site |
| [`docs/01-copilot-studio.md`](./docs/01-copilot-studio.md) | Lab A365-01, Copilot Studio with the GitHub Copilot harness |
| [`docs/00-prerequisites.md`](./docs/00-prerequisites.md) | SDK requirements, between Copilot Studio and .NET |
| [`docs/02a-web-obo-dotnet.md`](./docs/02a-web-obo-dotnet.md) | Lab A365-02A, .NET, the final lab in the main flow |
| [`docs/02b-web-obo-python.md`](./docs/02b-web-obo-python.md) | Optional lab A365-02B, Python |
| [`docs/02c-web-obo-nodejs.md`](./docs/02c-web-obo-nodejs.md) | Optional lab A365-02C, JavaScript / Node.js (TypeScript) |
| [`docs/99-sample-prompts.md`](./docs/99-sample-prompts.md) | Prompts chosen to produce specific, checkable telemetry |
| `mkdocs.yml` | Documentation site configuration |
| `docs/stylesheets`, `docs/javascripts` | Site theme and widgets |

## Running the site locally

The site is built with [MkDocs](https://www.mkdocs.org/) and [Material for MkDocs](https://squidfunk.github.io/mkdocs-material/).

```bash
pip install -r requirements.txt
mkdocs serve
```

The site is served at `http://127.0.0.1:8000/agent365-labs/`. Pushing to `main` builds the site and deploys it to GitHub Pages through the `Deploy Documentation` workflow, which uploads the build as an artifact rather than committing it to a branch.

The theme and the `cc-card` and `cc-next` widgets are adapted from the [Copilot Developer Camp](https://github.com/microsoft/copilot-camp), used under the MIT License.

## Where the SDK sample agents come from

The starting points for these labs are the three sample agents in the [Agent 365 runbook repository](https://github.com/qmatteoq/agent365-runbook), under `01-scenarios/Web-App-Agent-User-OBO/0.Resources/Starting-point/`. They are the same agent in three stacks: a research assistant that answers questions about Microsoft products by searching the [Microsoft Learn MCP server](https://learn.microsoft.com/api/mcp) and citing what it found. None of them contains any Agent 365 code.

You can bring your own agent instead. The labs assume it runs as a web app with one HTTP request per turn, and that there is a single place in the code where a turn begins and ends, which is where the instrumentation goes.

> This is a first draft. The sample agents are still referenced from the runbook repository instead of being vendored into this one, so cloning them is currently a manual step described in each lab.

Start with a new Copilot Studio agent in the product. After completing the SDK requirements, take the same documentation-assistant idea through the .NET web-app lab. Python and JavaScript are optional follow-on labs.

## Related

| Resource | What it is for |
| --- | --- |
| [Agent 365 runbooks](https://github.com/qmatteoq/agent365-runbook) | Reference guidance for onboarding your own agent, organized by scenario and pattern |
| [Agent 365 Skills](https://github.com/microsoft/agent365-skills) | The six skills the labs drive from your coding assistant |
| [Agent 365 documentation](https://learn.microsoft.com/microsoft-agent-365/) | The official product documentation |

Use these labs to learn the onboarding path end to end on a known sample. Use the runbooks when you are applying it to an agent of your own.

## Feedback

These labs are under active development. Open an issue if a step does not work, if a portal has moved, or if a warning would have saved you time.

## Disclaimer

This repository is a community resource and is not an official Microsoft product. Agent 365 is evolving, and commands, scopes and identifiers change. Verify against the [official Agent 365 documentation](https://learn.microsoft.com/microsoft-agent-365/) before relying on anything here in production.
