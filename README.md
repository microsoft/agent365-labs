# Agent 365 Labs

📖 **Read the labs at [microsoft.github.io/agent365-labs](https://microsoft.github.io/agent365-labs)**

Hands-on labs for [**Microsoft Agent 365**](https://learn.microsoft.com/microsoft-agent-365/). Build a Copilot Studio agent with built-in observability, discover a Salesforce agent through Connected Platforms, prepare the SDK requirements, then register and instrument a custom .NET web agent.

The Copilot Studio and SDK labs use a Microsoft documentation research assistant. You follow its tool calls and inspect the resulting activity in Microsoft Defender and the Microsoft 365 admin center. The Salesforce lab uses a simple training agent to demonstrate third-party inventory discovery.

Each lab is designed for a single sitting of hands-on work, with additional time for provisioning and indexing. Copilot Studio and Salesforce use browser surfaces; Salesforce includes an optional CLI workaround. The SDK labs use a terminal and a browser.

## Main lab flow

Complete these steps in order. Python and JavaScript are optional extensions, not required steps or replacements for .NET in this flow.

1. [Copilot Studio - Lab A365-01](./docs/01-copilot-studio.md)
2. [Salesforce Connected Platforms - Lab A365-01B](./docs/01b-salesforce-connected-platforms.md)
3. [SDK requirements](./docs/00-prerequisites.md)
4. [.NET - Lab A365-02A](./docs/02a-web-obo-dotnet.md)

### 1. Copilot Studio agent with the GitHub Copilot runtime harness

Start here. Create an agent in Copilot Studio using the GitHub Copilot harness. Add the Microsoft Learn MCP server and a native skill in the builder, confirm tool use in Preview, and inspect its activity. Copilot Studio emits the telemetry automatically; you do not add instrumentation.

This lab has its own tenant and Defender prerequisites. Check Activity availability in your tenant before relying on that surface.

| Lab | Runtime and authoring surface | Duration | Level |
| --- | --- | --- | --- |
| [A365-01](./docs/01-copilot-studio.md) | Copilot Studio new experience with the GitHub Copilot runtime harness | ~60-90 minutes, plus admin prework and indexing | Beginner to intermediate |

### 2. Salesforce Agentforce with Connected Platforms

Create a free Salesforce developer org, enable Agentforce and Data 360, configure an external client app, and connect it to Agent 365. Create and activate a Salesforce agent, then manually synchronize the connection to find it in the Microsoft 365 admin center. The lab includes the supported CLI workaround for runtime-user provisioning and optional session Activity verification.

| Lab | Authoring and administration surfaces | Duration | Level |
| --- | --- | --- | --- |
| [A365-01B](./docs/01b-salesforce-connected-platforms.md) | Salesforce Agentforce and Microsoft 365 admin center | ~60-90 minutes, plus provisioning and synchronization | Beginner to intermediate |

### 3. SDK requirements

After A365-01B, complete the [SDK requirements](./docs/00-prerequisites.md): install the tools and Agent 365 Skills, check tenant licences and permissions, clone the .NET sample, and prepare Azure OpenAI. Then continue to the .NET lab.

### 4. .NET web app agent with user on-behalf-of

In A365-02A, a user opens a web page, signs in, and drives the agent. Everything the agent does is attributed back to that person. This is the final lab in the main flow.

| Lab | Stack | Framework and host | Duration | Level |
| --- | --- | --- | --- | --- |
| [A365-02A](./docs/02a-web-obo-dotnet.md) | .NET 10 | Agent Framework on Blazor Server | ~3 hours | Intermediate |

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

The Salesforce lab then covers developer-org signup, capability and OAuth setup, connection creation, agent creation, and manual synchronization. Its completion evidence is a Salesforce-sourced agent in the Microsoft inventory; session Activity is an optional extension.

After the SDK requirements, the .NET lab follows these seven exercises. The optional Python and JavaScript labs follow the same structure for exercises 1 to 6, with the Python Work IQ limitation noted above.

| # | Exercise | What you have at the end |
| --- | --- | --- |
| 1 | Run the agent as it is | A working agent that answers questions and cites its sources, with no Agent 365 in it |
| 2 | Sign users in with Microsoft Entra | A registered sign-in application, and the code that acquires a user token |
| 3 | Register the agent with Agent 365 | An agent blueprint and identity in the tenant, and a user token addressed to the blueprint |
| 4 | Instrument the agent for observability | OpenTelemetry and the Agent 365 exporter emitting semantic spans per turn |
| 5 | Give the agent access to Microsoft 365 data | Mail, Calendar and more through the Work IQ MCP servers, as the signed-in user |
| 6 | Verify it end to end | Activity confirmed in Microsoft Defender and the Microsoft 365 admin center |
| 7 | See Defender react to a risky agent | A Defender alert for a prompt-injection attack, and an email notification when it happens again |

## Before you start

Check the prerequisites at the relevant point in the flow:

| Step | Start here |
| --- | --- |
| **1. Copilot Studio** | [Lab A365-01 prerequisites](./docs/01-copilot-studio.md#prerequisites) |
| **2. Salesforce Connected Platforms** | [Lab A365-01B prerequisites](./docs/01b-salesforce-connected-platforms.md#prerequisites) |
| **3. SDK requirements, before .NET** | [SDK requirements](./docs/00-prerequisites.md) |

| | |
| --- | --- |
| **Copilot Studio** | Copilot Studio new experience, tenant licensing and credits, Defender connector readiness, and browser access to the right environment |
| **Salesforce Connected Platforms** | A free Agentforce Developer Edition org, Salesforce System Administrator access, and a Microsoft administrator who can create platform connections |
| **SDK labs** | CLI, SDK, Azure OpenAI, local code, sample agent clone, and admin consent handoffs |

The Agent 365 CLI requires .NET 8 or later; the local .NET sample targets .NET 10 and requires the .NET 10 SDK. **The Copilot Studio lab does not use the CLI, PAC, local code, or Azure OpenAI.** Python, uv, and Node.js are needed only for their respective optional labs.

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
| [`docs/01b-salesforce-connected-platforms.md`](./docs/01b-salesforce-connected-platforms.md) | Lab A365-01B, Salesforce Agentforce inventory discovery with Connected Platforms |
| [`docs/00-prerequisites.md`](./docs/00-prerequisites.md) | SDK requirements, after Salesforce and before .NET |
| [`docs/02a-web-obo-dotnet.md`](./docs/02a-web-obo-dotnet.md) | Lab A365-02A, .NET, the final lab in the main flow |
| [`docs/02b-web-obo-python.md`](./docs/02b-web-obo-python.md) | Optional lab A365-02B, Python |
| [`docs/02c-web-obo-nodejs.md`](./docs/02c-web-obo-nodejs.md) | Optional lab A365-02C, JavaScript / Node.js (TypeScript) |
| [`docs/99-sample-prompts.md`](./docs/99-sample-prompts.md) | Prompts chosen to produce specific, checkable telemetry |
| [`samples/`](./samples/README.md) | Local .NET and Python starting points, with source provenance |
| `mkdocs.yml` | Documentation site configuration |
| `docs/stylesheets`, `docs/javascripts` | Site theme and widgets |
| `docs/videos` | Narrated Defender setup and Copilot Studio walkthrough MP4s, embedded in A365-01 |

## Running the site locally

The site is built with [MkDocs](https://www.mkdocs.org/) and [Material for MkDocs](https://squidfunk.github.io/mkdocs-material/).

```bash
pip install -r requirements.txt
mkdocs serve
```

The site is served at `http://127.0.0.1:8000/agent365-labs/`. Pushing to `main` builds the site and deploys it to GitHub Pages through the `Deploy Documentation` workflow, which uploads the build as an artifact rather than committing it to a branch.

The theme and the `cc-card` and `cc-next` widgets are adapted from the [Copilot Developer Camp](https://github.com/microsoft/copilot-camp), used under the MIT License.

Shared reading, link, focus, and navigation styles live in `docs/stylesheets/extra.css`; lab card layout lives in `docs/stylesheets/labStyles.css`. Cards follow the site's configured light theme rather than the operating system's color preference. Both widgets use native links for keyboard navigation and opening labs in a new tab.

Lab screenshots live in `docs/images/a365-01` (Copilot Studio) and `docs/images/a365-02a` (.NET). They are browser captures from a demo tenant and local agent, cropped to the relevant controls with account, tenant, and network identifiers and tool-output content redacted where applicable. When refreshing them, preserve the actual portal state, redact sensitive details before adding images to the repository, and update captions when the UI or example time range changes. Distinguish fully configured demos from starter states and historical portal events from newly generated conversations.

Lab videos are self-hosted in `docs/videos`, with poster images in `docs/images/a365-01`. A365-01 embeds the Defender setup video beside administrator prework, the Copilot Studio walkthrough before Exercise 1 (covering Exercises 1-3), and the Activity and Defender walkthrough before Exercise 4 (covering Exercises 4-5). Players use native controls, do not autoplay or preload the MP4s, and include download links. Preserve the sanitized demo screenshots and publication caveats when refreshing videos; the written steps remain the source of exact configuration text.

## Where the SDK sample agents come from

The .NET and Python starting points are included in this repository under [`samples/dotnet`](./samples/dotnet/) and [`samples/python`](./samples/python/). Clone this repository and open the .NET sample for the main flow:

```bash
git clone https://github.com/microsoft/agent365-labs
cd agent365-labs/samples/dotnet
```

For the optional Python lab, open `agent365-labs/samples/python` instead. The Node.js starting point remains external in the [Agent 365 runbook repository](https://github.com/qmatteoq/agent365-runbook/tree/main/01-scenarios/Web-App-Agent-User-OBO/0.Resources/Starting-point/nodejs); its lab includes separate clone instructions.

All three are research assistants that answer questions about Microsoft products by searching the [Microsoft Learn MCP server](https://learn.microsoft.com/api/mcp) and citing what they found. They include dormant Entra sign-in plumbing but no Agent 365 instrumentation. See [`samples/README.md`](./samples/README.md) for the pinned source commit and import details.

You can bring your own agent instead. The labs assume it runs as a web app with one HTTP request per turn, and that there is a single place in the code where a turn begins and ends, which is where the instrumentation goes.

Start with a new Copilot Studio agent in the product, then discover a Salesforce agent through Connected Platforms. After completing the SDK requirements, take the documentation-assistant idea through the .NET web-app lab. Python and JavaScript are optional follow-on labs.

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
