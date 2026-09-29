# Agent 365 Labs

Hands-on labs for [**Microsoft Agent 365**](https://learn.microsoft.com/microsoft-agent-365/). Start by building a Copilot Studio agent with built-in observability, prepare the SDK requirements, then register and instrument a custom .NET web agent.

The labs use a Microsoft documentation research assistant. You follow its tool calls and inspect the resulting activity in Microsoft Defender and the Microsoft 365 admin center.

Each lab is designed to be completed in a single sitting. Copilot Studio stays in browser surfaces; the SDK labs use a terminal and a browser.

## Main lab flow

**Copilot Studio → SDK requirements → .NET**

Complete these three steps in order. The main flow ends with .NET; Python and JavaScript are optional follow-on labs.

<div class="cc-cards">
<cc-card-grid>
  <cc-card
    title="1. Copilot Studio — A365-01"
    description="GitHub Copilot runtime harness in the Copilot Studio new experience. ~60-90 minutes, beginner to intermediate."
    href="01-copilot-studio/"
    target="_self">
  </cc-card>
  <cc-card
    title="2. SDK requirements"
    description="After Copilot Studio, prepare the tools, tenant permissions, .NET sample, and Azure OpenAI resource for the SDK lab."
    href="00-prerequisites/"
    target="_self">
  </cc-card>
  <cc-card
    title="3. .NET — A365-02A"
    description="Complete the main flow with Agent Framework on Blazor Server. ~3 hours, intermediate."
    href="02a-web-obo-dotnet/"
    target="_self">
  </cc-card>
</cc-card-grid>
</div>

### 1. Copilot Studio agent with the GitHub Copilot runtime harness

Start with this lab. Create an agent in Copilot Studio using the GitHub Copilot harness. Add the Microsoft Learn MCP server and a native skill in the builder, confirm tool use in Preview, and inspect its activity. Copilot Studio emits the telemetry automatically; you do not add instrumentation.

This lab has its own tenant and Defender prerequisites. Check Activity availability in your tenant before relying on that surface.

| Lab | Runtime and authoring surface | Duration | Level |
| --- | --- | --- | --- |
| [A365-01](01-copilot-studio.md) | Copilot Studio new experience with the GitHub Copilot runtime harness | ~60-90 minutes, plus admin prework and indexing | Beginner to intermediate |

### 2. SDK requirements

After A365-01, complete the [SDK requirements](00-prerequisites.md): install the tools and Agent 365 Skills, check tenant licences and permissions, clone the .NET sample, and prepare Azure OpenAI. Then continue to the .NET lab.

### 3. .NET web app agent with user on-behalf-of

In A365-02A, a user opens a web page, signs in, and drives the agent. Everything the agent does is attributed back to that person. This is the final lab in the main flow.

| Lab | Stack | Framework and host | Duration | Level |
| --- | --- | --- | --- | --- |
| [A365-02A](02a-web-obo-dotnet.md) | .NET 8 | Agent Framework on Blazor Server | ~3 hours | Intermediate |

## Optional labs

After completing the main flow, you can explore the same Agent 365 concepts in Python or JavaScript. Neither lab is required to complete the main flow. The JavaScript ecosystem lab runs on Node.js and uses TypeScript source.

<div class="cc-cards">
<cc-card-grid>
  <cc-card
    title="Optional: Python — A365-02B"
    description="Explore the SDK with LangChain on FastAPI. ~3 hours, intermediate. Not required for the main flow."
    href="02b-web-obo-python/"
    target="_self">
  </cc-card>
  <cc-card
    title="Optional: JavaScript / Node.js — A365-02C"
    description="Explore the SDK with LangChain on Express, in TypeScript. ~3 hours, intermediate. Not required for the main flow."
    href="02c-web-obo-nodejs/"
    target="_self">
  </cc-card>
</cc-card-grid>
</div>

!!! warning "Work IQ availability"
    Work IQ is not yet available for Python with LangChain. Optional lab A365-02B covers Exercise 5 as an explanation of the gap instead of an implementation. The main .NET lab and optional JavaScript lab include the Microsoft 365 data access implementation.

## What the labs cover

The first lab, Copilot Studio, moves through five stages:

| Stage | What you do | What you have at the end |
| --- | --- | --- |
| 1 | Create the agent in the new experience | A Copilot Studio agent in the runtime harness, with recorded environment and bot identifiers |
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
| **1. Copilot Studio** | [Lab A365-01 prerequisites](01-copilot-studio.md#prerequisites) |
| **2. SDK requirements, before .NET** | [SDK requirements](00-prerequisites.md) |

Copilot Studio needs browser access and the tenant setup described in its prerequisites. The SDK labs need the CLI, .NET SDK, Azure OpenAI, and a sample agent checkout. Python, uv, and Node.js are needed only for their respective optional labs.

## How the labs are written

Each lab is a sequence of **exercises**, and each exercise is a sequence of **steps**. Every exercise ends with a checkpoint stating what should be true before you move on, so you can stop between exercises and pick the lab up later.

The Copilot Studio lab creates a native skill in the product and pairs it with the Microsoft Learn MCP server. Browser steps use the same exercise, step, and checkpoint structure as the SDK labs.

The SDK labs do most of the Agent 365 work by asking an AI coding assistant to do it for you, using the [Agent 365 Skills](https://github.com/microsoft/agent365-skills). Those steps show four things:

1. **What you type**, the prompt you give your coding assistant
2. **What the skill does**, the changes it makes
3. **Behind the scenes**, the CLI command or code underneath
4. **How to verify**, how to confirm it worked before moving on

## Where the SDK sample agents come from

The starting points for these labs are the three sample agents in the [Agent 365 runbook repository](https://github.com/qmatteoq/agent365-runbook), under `01-scenarios/Web-App-Agent-User-OBO/0.Resources/Starting-point/`. They are the same agent in three stacks: a research assistant that answers questions about Microsoft products by searching the [Microsoft Learn MCP server](https://learn.microsoft.com/api/mcp) and citing what it found. None of them contains any Agent 365 code.

You can bring your own agent instead. The labs assume it runs as a web app with one HTTP request per turn, and that there is a single place in the code where a turn begins and ends, which is where the instrumentation goes.

!!! info "First draft"
    The sample agents are still referenced from the runbook repository instead of being vendored into this one, so cloning them is currently a manual step described in each lab.

Start with a new Copilot Studio agent in the product. After completing the SDK requirements, take the same documentation-assistant idea through the .NET web-app lab. Python and JavaScript are optional follow-on labs.

## Related

| Resource | What it is for |
| --- | --- |
| [Agent 365 runbooks](https://github.com/qmatteoq/agent365-runbook) | Reference guidance for onboarding your own agent, organized by scenario and pattern |
| [Agent 365 Skills](https://github.com/microsoft/agent365-skills) | The six skills the labs drive from your coding assistant |
| [Agent 365 documentation](https://learn.microsoft.com/microsoft-agent-365/) | The official product documentation |

Use these labs to learn the onboarding path end to end on a known sample. Use the runbooks when you are applying it to an agent of your own.

## Feedback

These labs are under active development. [Open an issue](https://github.com/qmatteoq/agent365-labs/issues) if a step does not work, if a portal has moved, or if a warning would have saved you time.

## Disclaimer

This repository is a community resource and is not an official Microsoft product. Agent 365 is evolving, and commands, scopes and identifiers change. Verify against the [official Agent 365 documentation](https://learn.microsoft.com/microsoft-agent-365/) before relying on anything here in production.

<cc-next label="Start with Lab A365-01" url="01-copilot-studio/"></cc-next>
