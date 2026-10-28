# SDK requirements

**Main flow: [1. Copilot Studio](./01-copilot-studio.md) → 2. SDK requirements → [3. .NET](./02a-web-obo-dotnet.md)**

Complete this setup after Copilot Studio and before the .NET lab. These requirements also apply to the optional [Python](./02b-web-obo-python.md) and [JavaScript / Node.js](./02c-web-obo-nodejs.md) labs; install their language-specific tools only if you plan to take them.

The first lab has its own [Copilot Studio prerequisites](./01-copilot-studio.md#prerequisites). You do not need the SDK tools to complete that lab.

Every SDK lab in this repository requires the same three things: tools on your machine, licences in your tenant, and permissions on your account.

## The tools you need on your machine

| Requirement | Minimum | Check it with | What needs it |
| --- | --- | --- | --- |
| .NET SDK | 10.0 for the .NET sample; 8.0 or later for the CLI | `dotnet --list-sdks` | The Agent 365 CLI itself, and .NET agents |
| Agent 365 CLI | latest | `a365 -h` | Every SDK lab |
| PowerShell | 7.0 | `pwsh --version` | The consent scripts |
| Azure CLI | latest | `az version` | Signing in to the tenant |
| Az PowerShell module | latest | `Get-Module -ListAvailable Az.Accounts` | Granting permissions |
| Git | any | `git --version` | Cloning the sample agents |
| Python | 3.12 | `python --version` | Optional Python lab only |
| uv | latest | `uv --version` | Optional Python lab only |
| Node.js | 20.10 | `node --version` | Optional JavaScript / Node.js lab only |

> The Agent 365 CLI requires .NET 8.0 or later, even if your agent is written in Python or TypeScript. It is distributed as a .NET global tool; the main .NET lab targets .NET 10 and requires the .NET 10 SDK. These requirements are for the **SDK labs** only; Lab A365-01 does not use the CLI.

### Automated Windows installation

The installer is included in this repository at `docs/scripts/Install-SdkRequirements.ps1`. You can share the script itself or this page with participants; they do not need Git or a clone of the repository to download it.

It runs in Windows PowerShell 5.1 or PowerShell 7 and uses WinGet to install missing tools. WinGet (App Installer) must already be available through your organization's approved software channel. Installers may request administrator approval.

**Download the script**

1. Right-click [Download Install-SdkRequirements.ps1](./scripts/Install-SdkRequirements.ps1) and choose **Save link as** (the wording may vary by browser).
2. Save it in your **Downloads** folder with the name `Install-SdkRequirements.ps1`, not `Install-SdkRequirements.ps1.txt`. If the link opens the script as text, use your browser's **Save as** command.
3. Review the script in a text editor before running it.

Alternatively, open PowerShell and download it from the repository:

```powershell
Set-Location (Join-Path $HOME 'Downloads')
Invoke-WebRequest `
    -Uri 'https://raw.githubusercontent.com/microsoft/agent365-labs/main/docs/scripts/Install-SdkRequirements.ps1' `
    -OutFile '.\Install-SdkRequirements.ps1'
```

**Run the installer**

Open Windows PowerShell or PowerShell 7, switch to the download folder, and preview the changes. If PowerShell blocks the script, follow **If PowerShell blocks the unsigned script** below, then retry.

```powershell
Set-Location (Join-Path $HOME 'Downloads')
.\Install-SdkRequirements.ps1 -WhatIf
```

Then run the installer for the labs you plan to take:

```powershell
# Main .NET lab only.
.\Install-SdkRequirements.ps1

# Main lab plus the optional Python lab.
.\Install-SdkRequirements.ps1 -IncludePython

# Main lab plus the optional JavaScript / Node.js lab.
.\Install-SdkRequirements.ps1 -IncludeNode

# Main lab and both optional labs.
.\Install-SdkRequirements.ps1 -IncludePython -IncludeNode
```

Run only the command that matches your chosen labs. If you already cloned the repository, you can skip the download and run `.\docs\scripts\Install-SdkRequirements.ps1` from the repository root with the same switches.

**If PowerShell blocks the unsigned script**

The lab installer is not digitally signed. Windows may block a downloaded script with an error such as **"is not digitally signed"** or **"running scripts is disabled on this system"**. `-WhatIf` does not bypass this check.

1. Review `Install-SdkRequirements.ps1` in a text editor and confirm that it came from this repository or your trusted lab facilitator. Only continue if your organization's policy permits running this reviewed, unsigned script.
2. In the **same PowerShell terminal** where you will run the installer, inspect the effective policy and its scopes:

   ```powershell
   Get-ExecutionPolicy
   Get-ExecutionPolicy -List
   ```

   If `MachinePolicy` or `UserPolicy` is not `Undefined`, your administrator manages execution policy through Group Policy. Do not try to override it; ask for an approved way to run the installer, such as a signed copy. Also stop if your organization requires signed scripts or another security control blocks execution.

3. If permitted and the effective policy is `Restricted` or `AllSigned`, allow local or explicitly unblocked scripts for **this terminal session only**:

   ```powershell
   Set-ExecutionPolicy -Scope Process -ExecutionPolicy RemoteSigned
   Get-ExecutionPolicy
   ```

   Confirm the policy-change prompt if shown. The effective policy should now be `RemoteSigned`. This does not require an administrator terminal or change the saved `CurrentUser` or `LocalMachine` settings. Closing this terminal removes the session policy. If the effective policy does not change, stop and contact your administrator.

4. With `RemoteSigned`, a browser-downloaded unsigned file may still be blocked because Windows marks it as coming from the internet. After reviewing it, unblock **only this file**, then preview the installer again:

   ```powershell
   Set-Location (Join-Path $HOME 'Downloads')
   Unblock-File -LiteralPath '.\Install-SdkRequirements.ps1'
   .\Install-SdkRequirements.ps1 -WhatIf
   ```

   `Unblock-File` permanently removes the internet-zone mark from that file; it does not sign it or change execution policy. If using a repository clone, stay in the repository root and use `.\docs\scripts\Install-SdkRequirements.ps1` for both commands instead.

5. Run the installation command for your chosen labs from the examples above in this same terminal. Close the terminal when finished.

Do not set a machine-wide `Bypass` or `Unrestricted` policy to run the lab. If execution is still blocked, share the error and the output of `Get-ExecutionPolicy -List` with your administrator rather than disabling additional controls. See Microsoft's [PowerShell execution policy documentation](https://learn.microsoft.com/powershell/module/microsoft.powershell.core/about/about_execution_policies) for policy precedence and unsigned-script behavior.

The script installs .NET SDK 10 for all participants, which also satisfies the Agent 365 CLI requirement. Existing tools that meet the minimum version are left in place; the Agent 365 CLI is installed or updated, and the Az module is installed for the current user in PowerShell 7 if missing. For tools listed as "latest" above, use your organization's normal update process to keep existing installations current.

The script uses the machine's configured WinGet sources, NuGet feeds, and PowerShell repositories. It does not add public feeds, accept source or package agreements automatically, change repository trust, or bypass execution policy. Review any required agreements or trust decisions through your approved setup process before running it. If a package is unavailable or script execution is blocked, ask your administrator for an approved resolution. A failed installation stops the script with an error; rerun it after resolving the cause. Open a new PowerShell 7 terminal afterward.

When the script reports **SDK tools are ready**, close the terminal and open **PowerShell 7** to continue the lab with the updated `PATH`.

This installs **local SDK tools only**. Install the Agent 365 Skills separately for your chosen assistant below. Tenant licences, permissions, Azure sign-in, sample cloning, and Azure OpenAI setup still require the steps on this page.

### Install or update the Agent 365 CLI manually

Install the **`Microsoft.Agents.A365.DevTools.Cli`** package to use the `a365` command. See [Install and use the Agent 365 CLI](https://learn.microsoft.com/microsoft-agent-365/developer/agent-365-cli?tabs=windows) for the CLI reference.

Open PowerShell and run `dotnet tool list --global`. If `Microsoft.Agents.A365.DevTools.Cli` is listed, use the update command below. Otherwise, use the installation command. Package IDs may appear in lowercase.

**First installation**

```powershell
dotnet tool install --global Microsoft.Agents.A365.DevTools.Cli
```

**Update an existing installation**

```powershell
dotnet tool update --global Microsoft.Agents.A365.DevTools.Cli
```

**If installation reports an `a365` command conflict**

Check `dotnet tool list --global`. If `Microsoft.Agents365.CLI` is listed, run the following commands to replace it with `Microsoft.Agents.A365.DevTools.Cli`:

```powershell
dotnet tool uninstall --global Microsoft.Agents365.CLI
dotnet tool install --global Microsoft.Agents.A365.DevTools.Cli
```

Use your organization's configured package feeds. If the package is unavailable, ask your administrator to make it available.

**Verify the installation**

```powershell
dotnet tool list --global
a365 -h
```

Confirm the tool list contains `Microsoft.Agents.A365.DevTools.Cli` and that `a365 -h` displays help. If Windows cannot find `a365`, open a new terminal and check that `%USERPROFILE%\.dotnet\tools` is on your `PATH`.

## Installing the Agent 365 Skills

Most of the Agent 365 work in the **SDK labs** is done by asking an AI coding assistant to do it, using the [Agent 365 Skills](https://github.com/microsoft/agent365-skills). The `agent365` plugin bundles these skills, including `a365-setup`, `instrument-observability`, and `add-workiq-tools`.

**Choose exactly one of the three options below. You do not need to complete all three.** Use Option 1 to install the skills manually through the GitHub CLI, Option 2 to install the plugin in VS Code, or Option 3 to install the plugin in the GitHub Copilot app. Options 2 and 3 do **not** require the GitHub CLI.

Review the skills and plugin contents before installing, because they can guide your assistant to run commands on your machine. If your organization restricts plugins or marketplaces, ask your administrator to approve the source. For Claude Code, Cursor, Windsurf, Codex CLI, Gemini CLI, or another assistant, follow the [upstream installation instructions](https://github.com/microsoft/agent365-skills#getting-started) instead.

> Lab A365-01 does not use these coding-assistant skills. It uses a native skill created inside the Copilot Studio browser authoring experience.

### Option 1: Install the GitHub CLI and add the skills manually

This option installs the skills directly for GitHub Copilot, rather than installing the bundled plugin through a graphical interface. The GitHub CLI (`gh`) is separate from Git and from the GitHub Copilot CLI. The automated SDK tools installer above does not install it.

1. Open PowerShell and install the GitHub CLI using your configured WinGet sources:

   ```powershell
   winget install --id GitHub.cli --exact
   ```

2. Open a **new terminal**, verify the installation, and sign in to GitHub. Skip `gh auth login` if `gh auth status` confirms that you are already signed in with the intended account.

   ```powershell
   gh --version
   gh auth status
   gh auth login
   ```

3. Check that your GitHub CLI supports skill installation:

   ```powershell
   gh skill install --help
   ```

   Skill management is a preview feature. If the command is unavailable, update the GitHub CLI through your organization's approved software channel, or choose Option 2 or 3 instead. See the [GitHub CLI installation guide](https://github.com/cli/cli#installation) for other operating systems and installation methods.

4. Open a terminal in **your agent's project folder** and install the Agent 365 skills:

   ```powershell
   gh skill install microsoft/agent365-skills --all --agent github-copilot --scope project
   ```

   `gh skill add` is an alias for `gh skill install`. The explicit `--all` installs every discovered skill, and `--scope project` places them in the repository's `.agents/skills/` directory. Run this from the checkout you will use for the lab.

5. Verify the installation:

   ```powershell
   gh skill list --agent github-copilot --scope project
   ```

   Confirm that the list includes `a365-setup` and `instrument-observability`. Open that same project in your coding assistant and start a new session before continuing.

See the [GitHub CLI skill installation reference](https://cli.github.com/manual/gh_skill_install) for details.

### Option 2: Install the plugin in VS Code

Use this option if you will complete the lab in VS Code with GitHub Copilot. No `gh` commands are needed.

**Install VS Code and sign in to Copilot**

If VS Code and Copilot are already set up, skip to the plugin steps below.

1. Open the [VS Code download page](https://code.visualstudio.com/download) and select the installer for your operating system and processor. On Windows, choose **User Installer** to install for your account without administrator permissions, unless your organization requires a different installation method.
2. Run the downloaded installer and follow its prompts, then launch **Visual Studio Code** from the Start menu. If your organization manages software installation, use its approved software channel instead. See [Installing VS Code on Windows](https://code.visualstudio.com/docs/setup/windows) for details.
3. In VS Code, open the Copilot menu in the Status Bar and select **Use AI Features** or **Sign in to use Copilot**. Follow the browser sign-in prompts using the GitHub account with your Copilot subscription or organization-provided access. Confirm that you can open Copilot Chat before continuing. See [Set up GitHub Copilot in VS Code](https://code.visualstudio.com/docs/setup/copilot) if sign-in or setup is unavailable.

Installing the separate GitHub Copilot CLI is not required for this option, even if the VS Code installer offers it.

**Install the Agent 365 plugin**

1. Open your agent's project folder in VS Code.
2. Open the Command Palette with **Ctrl+Shift+P**, run **Preferences: Open User Settings (JSON)**, and add the Agent 365 marketplace:

   ```json
   "chat.plugins.marketplaces": [
       "microsoft/agent365-skills"
   ]
   ```

   This is an entry inside your existing `settings.json` object, not a replacement for the whole file. If `chat.plugins.marketplaces` already exists, append `"microsoft/agent365-skills"` to its array and preserve the other entries.

3. Ensure the **Chat: Plugins: Enabled** setting (`chat.plugins.enabled`) is enabled.
4. Open **Extensions** with **Ctrl+Shift+X** and search for `@agentPlugins`.
5. Find **agent365** from the **agent365-skills** marketplace, review its contents, and select **Install**. Review and confirm the marketplace trust prompt if one appears.
6. In Copilot Chat, select **Open Customizations**, then **Skills**. Confirm that skills such as `a365-setup` and `instrument-observability` are available.
7. Start a new Copilot Chat session for the lab.

If these plugin controls are unavailable, update VS Code through your approved software channel or use Option 1. See [Agent plugins in VS Code](https://code.visualstudio.com/docs/agent-customization/agent-plugins) for the current interface and settings.

### Option 3: Install the plugin in the GitHub Copilot app

Use this option if you will complete the lab in the GitHub Copilot desktop app. No GitHub CLI or VS Code installation is needed for this route.

**Install the GitHub Copilot app and sign in**

If the app is already installed and you are signed in, skip to the plugin steps below. This is the **GitHub Copilot app**, not GitHub Desktop or the GitHub Copilot CLI.

1. Ensure **Git** is installed (`git --version`). The automated SDK tools installer above includes it.
2. Open the [official GitHub Copilot app download page](https://github.com/features/ai/github-app) and download the installer for your operating system and processor. Use your organization's approved software channel instead if it manages application installation.
3. Run the downloaded installer and follow its prompts, then launch the GitHub Copilot app.
4. Select **Sign in to GitHub** and complete the browser sign-in using your Copilot-enabled account. Complete onboarding; you can add your agent's local project folder now or after installing the plugin. For organization-provided Copilot access, your administrator must allow use of the GitHub Copilot app.

See [Getting started with the GitHub Copilot app](https://docs.github.com/en/copilot/get-started/quickstart-copilot-app) for installation, account requirements, and connecting a local folder.

**Install the Agent 365 plugin**

1. Open the GitHub Copilot app.
2. Open **Customize**, then **Plugins**.
3. Select **Add marketplace** (it may be under **Install**, depending on your app version).
4. Enter the marketplace repository:

   ```text
   https://github.com/microsoft/agent365-skills
   ```

5. Review the source and confirm adding the marketplace. Expand **agent365-skills**, then select **Install** next to **agent365**. Adding the marketplace alone does not install the plugin.
6. Confirm that **agent365** is installed and enabled. In **Customize**, check **Skills** for entries such as `a365-setup` and `instrument-observability`.
7. Open your agent's project in the app and start a new session for the lab.

Use the app's plugin interface for installation; do not enter terminal commands such as `gh skill install` into the chat composer. See [Plugins for GitHub Copilot](https://docs.github.com/en/copilot/concepts/customizations/plugins) for supported clients and installation options.

## The licences your tenant needs for the SDK labs

| Requirement | What to know |
| --- | --- |
| Microsoft 365 tenant | With Agent 365 enabled |
| Agent 365 licence | **At least one user in the tenant must hold one** |
| Azure subscription | For the Azure OpenAI resource the agent reasons with |

> If nobody in the tenant holds an Agent 365 licence, telemetry is accepted with `HTTP 200` and then discarded. The logs report a successful export, Defender shows no data, and no error is raised. Verify the licence before you start.

## The permissions you need on your account for the SDK labs

| Permission | Who needs it | What it is for |
| --- | --- | --- |
| Application Developer (or higher) | You | Creating app registrations |
| Global Administrator | You *or* a colleague | Granting admin consent to the blueprint |
| Azure OpenAI access | You | The model the agent uses. For the Entra credential path you need the **Cognitive Services OpenAI User** role on the resource; for key auth, access to the resource keys |

> You do not need to be a Global Administrator yourself. The CLI performs every action it is permitted to perform, then prints a PowerShell snippet for an administrator to run for the rest. Identify the person who will run it before you start.

The web on-behalf-of labs contain two of these handoffs: one when the agent is registered, and one when the sign-in app is given permission to call the agent's blueprint. Each takes an administrator about a minute.

## Getting the sample agent

The SDK labs start from a research assistant that answers questions about Microsoft products by searching the Microsoft Learn MCP server. Clone this repository and open the local .NET sample for the main flow:

```bash
git clone https://github.com/microsoft/agent365-labs
cd agent365-labs/samples/dotnet
```

The .NET and Python starters are local to this repository. Node.js remains in the external runbook repository:

| Lab | Folder |
| --- | --- |
| A365-02A (.NET, main flow) | `agent365-labs/samples/dotnet/` |
| A365-02B (Python, optional) | `agent365-labs/samples/python/` |
| A365-02C (JavaScript / Node.js, optional) | External: `agent365-runbook/01-scenarios/Web-App-Agent-User-OBO/0.Resources/Starting-point/nodejs/` |

If you already cloned this repository, open the relevant folder in that checkout rather than cloning it again. For Node.js, follow the separate clone instructions in [Lab A365-02C](./02c-web-obo-nodejs.md).

You can bring your own agent instead. The SDK labs assume two things about it: it runs as a web app with one HTTP request per turn, and there is a single place in the code where a turn begins and ends, which is where the instrumentation goes.

## An Azure OpenAI resource for the SDK labs

The SDK agents call an **Azure OpenAI model deployed in Microsoft Foundry**. You will create a **Foundry project**, which is your workspace in the portal, and a **Foundry resource**, which holds the model deployments, endpoint, and access settings. A model deployment makes a particular model available for your agent to call.

These instructions use the **new Microsoft Foundry portal**. You will create a project, deploy a chat model, test it, and collect the connection settings for the labs. If your facilitator provides a project and deployment, start at Step 4.

> **Costs:** model calls, including playground tests, incur Azure charges. A Microsoft 365 or Agent 365 licence does not include Azure OpenAI usage. Use a subscription approved for the lab and review [Azure OpenAI pricing](https://azure.microsoft.com/pricing/details/cognitive-services/openai-service/) before deploying.

### Step 1: Sign in and check your Azure subscription

1. Open the [Azure portal](https://portal.azure.com) and sign in with the account you will use for the lab.
2. Search for **Subscriptions** in the search bar at the top of the portal and open it.
3. Select the subscription approved for the lab. Confirm its status is **Active**.

### Step 2: Create a Foundry project and resource

Use these Azure settings when creating your project:

| Field | What to enter |
| --- | --- |
| **Subscription** | Your current subscription |
| **Resource group** | Select the group provided by your facilitator, or allow the portal to create a dedicated group for this lab |
| **Location** | An organization-approved region that supports `gpt-6.1-sol`; check [model availability by region](https://learn.microsoft.com/azure/foundry/foundry-models/concepts/models-sold-directly-by-azure-region-availability) |

1. Open [Microsoft Foundry](https://ai.azure.com) and sign in with the same account.
2. Make sure that the **New Foundry** toggle **On**.
3. Click on **Create new project** in the top right corner. If your first sign-in already shows the project creation form, continue with that form.
4. Enter `agent365-labs` as the project name.
5. Expand **Advanced options** and fill the values in the following way:
   1. **Region**: pick up one of the supported regions.
   2. **Subscription**: choose your azure subscription.
   3. Click on **Create new resource group** and create a new one called **rg-agentic-bootcamp**. Once you're back to the Foundry portal, select the resource group you have just created under **Resource group**.
6. Select **Create**, as shown in the form. Foundry creates the project and its parent Foundry resource.
7. Wait for creation to finish. You should be redirected directly to the `agent365-labs` project.

### Step 3: Deploy a chat model

1. In Foundry, confirm your lab project is selected.
2. Select **Discover** in the upper-right navigation, then **Models** in the left pane.
3. Search for **gpt-6.1-sol** and open its model card. This Azure OpenAI chat model supports the tool calls used by the sample agents. If it is unavailable, ask your facilitator for an approved chat model with tool-calling support.
4. Select **Deploy** > **Default settings**.
5. When deployment completes, Foundry opens the model playground.
6. Try any prompt in the chat panel to confirm that the model working.


### Step 4: Collect the connection settings

1. In Foundry, go back to the home page.
2. Record the resource's **Azure OpenAI base endpoint**, in the form `https://<resource-name>.openai.azure.com/`. If the example shows `https://<resource-name>.openai.azure.com/openai/v1/`, keep only `https://<resource-name>.openai.azure.com/` for these samples. They add the API path themselves. See [Foundry model endpoints](https://learn.microsoft.com/azure/foundry/foundry-models/concepts/endpoints#azure-openai-inference-endpoint).
3. In the [Azure portal](https://portal.azure.com), open the Foundry resource that owns this deployment. Record its resource name and resource group; you will use them for the access assignment in Step 5.
4. Open **Microsoft Entra ID** > **Overview** and copy **Tenant ID**. Confirm the directory is the one that owns the Azure subscription used above.
5. Keep the three connection settings in the table below for Exercise 1 of your chosen SDK lab.

| Setting | Example | Used in the samples |
| --- | --- | --- |
| Azure OpenAI endpoint | `https://<resource-name>.openai.azure.com/` | .NET: `AzureOpenAI:Endpoint`; Python/Node.js: `AZURE_OPENAI_ENDPOINT` |
| Deployment name | `gpt-6.1-sol` | .NET: `AzureOpenAI:Deployment`; Python/Node.js: `AZURE_OPENAI_DEPLOYMENT` |
| Tenant ID | The directory's GUID | .NET: `AzureOpenAI:TenantId`; Python/Node.js: `AZURE_OPENAI_TENANT_ID` |

The lab samples use the **Azure OpenAI base endpoint**, not the Foundry project endpoint ending in `/api/projects/<project-name>`. Use the **deployment name** in configuration, even if you chose a name different from the underlying model.

The .NET starter uses the **Responses API** so `gpt-6.1-sol` can combine reasoning with
function tools. Python and Node.js retain their existing API paths; the .NET migration
does not establish tool-plus-reasoning compatibility for those optional labs. Check that
compatibility with your facilitator before using the same deployment in another stack.

### Step 5: Grant your account access to call the model

The labs recommend signing in with Microsoft Entra credentials instead of storing an API key. The samples call the Azure OpenAI API, so your signed-in account needs **Cognitive Services OpenAI User**, or another role that includes that API's Entra inference access, on the **parent Foundry resource**. Project access and permission to create resources alone do not grant this resource-level model access.

1. In the Azure portal, open the Foundry resource you recorded in Step 4.
2. Select **Access control (IAM)** > **Add** > **Add role assignment**.
3. Search for and select **Cognitive Services OpenAI User**, then select **Next**.
4. Under **Assign access to**, select **User, group, or service principal**.
5. Select **Select members**, find the account you will use for the SDK lab (if you're using a MDX tenant, it should be **MOD Administrator**), select it, and confirm the selection.
6. Select **Review + assign** to finish the assignment.

Allow a few minutes for the role to take effect. See the [Cognitive Services OpenAI User role](https://learn.microsoft.com/azure/role-based-access-control/built-in-roles/ai-machine-learning#cognitive-services-openai-user) if an administrator needs to check your existing access

## Next steps

Once the requirements above are ready, continue to [Lab A365-02A: .NET](./02a-web-obo-dotnet.md), the final step in the main flow.

The [Python](./02b-web-obo-python.md) and [JavaScript / Node.js](./02c-web-obo-nodejs.md) labs are optional extensions after the main flow.

<cc-next label="Continue to .NET - Lab A365-02A" url="../02a-web-obo-dotnet/"></cc-next>
