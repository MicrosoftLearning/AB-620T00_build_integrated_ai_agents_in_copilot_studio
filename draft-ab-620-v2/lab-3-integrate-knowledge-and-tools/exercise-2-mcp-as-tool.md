---
lab:
  title: '3.2: Add an MCP server as an agent tool'
  description: In this exercise, you attach a Model Context Protocol (MCP) server to an agent as a tool so it can retrieve cross-source workplace context through a single discoverable endpoint.
  duration: 20 minutes
  level: 300
  islab: true
  primarytopics:
    - Microsoft Copilot Studio
---

# Add an MCP server as an agent tool

A **Model Context Protocol (MCP) tool** attaches an entire MCP server to your agent as a tool your agent can invoke as needed. The server publishes a suite of discoverable, self-describing capabilities, and the agent picks from them based on the rep's intent. In this exercise, you register the **Work IQ (preview)** MCP server — Microsoft's intelligence layer that unifies cross-source workplace context (emails, meetings, chats, and files) behind a single MCP endpoint — so the agent can look up support-ops information the rep has on their calendar, like when a policy changed or when a product defect was confirmed.

You will complete the following tasks:

- Seed your calendar with two support-ops events.
- Register the Work IQ MCP server as a tool.
- Review the discovered tools, scope them, and add a description that guides tool selection.
- Test the tool through the agent's **Preview** tab.

This exercise should take approximately **20** minutes to complete.

## Before you start

This exercise builds directly on the previous exercise in this lab. Complete [Exercise 1 — Ground an agent in Dataverse knowledge](exercise-1-ground-in-dataverse.md) first. You also need:

- A tenant admin has enabled Work IQ for your Microsoft 365 tenant. See [Enable your tenant for Work IQ](https://learn.microsoft.com/microsoft-365/copilot/extensibility/work-iq/enable-work-iq) at `https://learn.microsoft.com/microsoft-365/copilot/extensibility/work-iq/enable-work-iq` if it hasn't been enabled yet.
- Awareness that Work IQ is a **preview feature**. Its capabilities and the specific tools it publishes may evolve. If Work IQ isn't available in the Add-a-tool picker in your tenant, you can register any of the other built-in MCP servers instead — the pattern in Tasks 2 through 4 is the same, though you may need to adjust the seed step in Task 1 to fit whichever server you pick.

> [!NOTE]
> Task 4 uses Copilot Credits. Work IQ is powered by the GitHub Copilot harness and uses usage-based billing per [Work IQ in Copilot Studio (preview)](https://learn.microsoft.com/microsoft-copilot-studio/add-work-iq) at `https://learn.microsoft.com/microsoft-copilot-studio/add-work-iq`.

> [!NOTE]
> **Why MCP for this, and not a connector or a knowledge source?** Copilot Studio has three ways to integrate an agent with an external system. **Knowledge sources** ground the agent's generative answers with read-only retrieval — for example a Dataverse table or a SharePoint site connected as a knowledge source. **Connector-based tools** expose a single action from a Microsoft-managed connector, with a hand-written description that gives you tight control over when the agent invokes it. **MCP-based tools** expose a suite of discoverable tools that a server publishes with its own names and descriptions; the maker's control lever is the server-level description that decides when the agent reaches for this MCP at all. Microsoft's guidance ([Use agent tools to extend, automate, and enhance your agents](https://learn.microsoft.com/microsoft-copilot-studio/guidance/agent-tools#integrate-agent-tools-by-using-mcp) at `https://learn.microsoft.com/microsoft-copilot-studio/guidance/agent-tools#integrate-agent-tools-by-using-mcp`) is that MCP is a good fit for **standardized, centrally managed, discoverable** tool suites; connectors are a better fit when you want tight control over a single action. Work IQ fits the MCP pattern: cross-source retrieval spanning email, meetings, chats, and files is much closer to "expose a suite of tools" than "expose one hand-written action."

## Task 1 — Seed your calendar with support-ops events

Seed your calendar with two support-ops events the Preview test in Task 4 will retrieve. Work IQ pulls from the connected account's workplace context, so if nothing on the calendar matches the scenario, the tool has nothing to return.

1. Sign in to Outlook on the web at [https://outlook.office.com/calendar](https://outlook.office.com/calendar) with the tenant account you're using for Copilot Studio.

1. Create the following two calendar events. Use the dates suggested below (all-day events are fine — treat them as milestone markers, not meetings):

    | Event title | Date | Body / notes |
    |---|---|---|
    | `Returns policy update — supervisor approval now required over $200` | ~60 days ago | `As of this date, refunds of $200 or more require supervisor approval. The Approve refund request workflow enforces this routing automatically. Refunds under $200 remain rep-approvable.` |
    | `BLD-100 motor defect confirmed — auto-approve returns, waive restocking fee` | ~30 days ago | `Engineering confirmed the motor-shutoff defect on BLD-100 units shipped January through March. Auto-approve returns for units in this window and waive the standard restocking fee. Offer a replacement from later production runs (serial prefix BLD-100-K or later).` |

    > [!TIP]
    > Add a short body/notes description to each event exactly as shown. Work IQ retrieval is stronger when there's text to match against, not just a title.

    > [!NOTE]
    > Work IQ indexing runs on a short delay. If the Preview test in Task 4 returns nothing, wait a minute and rerun the prompt.

## Task 2 — Add the Work IQ MCP server as a tool

Register the Work IQ MCP server as a tool on the agent. Copilot Studio contacts the server, negotiates the MCP handshake, and pulls in the tool suite the server publishes — no server URL or auth to configure because Work IQ is a built-in Microsoft-provided MCP server.

1. Open the **Customer Support Rep Assistant** agent.

1. On the **Build** tab, in the components panel, select **+** next to **Tools**.

1. In the **Add a tool** dialog, select the **Model Context Protocol (MCP)** filter.

1. Select **Work IQ (preview)** from the list of available MCP servers.

    > [!NOTE]
    > **Work IQ** is a built-in Microsoft-provided MCP server. You don't provide a server URL, credentials, or authentication details — Copilot Studio handles those for built-in MCP servers. To register a non-built-in MCP server (one hosted at your own URL), you'd use the **+ Add** button at the top of the same dialog. That flow is documented at [Add a Model Context Protocol (MCP) server to your agent as a tool (preview)](https://learn.microsoft.com/microsoft-copilot-studio/agents-experience/tools-add-mcp-server) at `https://learn.microsoft.com/microsoft-copilot-studio/agents-experience/tools-add-mcp-server` and follows the same shape, just with URL and auth fields.

1. In the **Select a connection** dialog, expand the **Connection** dropdown and select **Create New Connection**. Sign in with the same tenant account you used to create the calendar events in Task 1, and complete consent. The first time a given account uses any connector or MCP in an environment, Power Automate prompts for consent and creates a named connection; subsequent uses of the same connector reuse it silently.

1. Select **Add and configure**. Copilot Studio contacts the MCP server, negotiates the protocol handshake, and lists the tools the server exposes. You land on the tool detail page.

## Task 3 — Review the discovered tools and describe when to use the server

Review the tools the MCP server published and add a server-level description that helps the agent decide when to reach for this server. Unlike a connector where you hand-tune a single action, MCP gives you two levers: which of the published tools stay enabled, and the server-level description that decides when the agent uses this server at all.

1. In the tool detail page, review the **Tools** section. Each row shows a name and description supplied by the MCP server itself — search and retrieval tools spanning email, meetings, chats, and files.

1. If you don't need every published tool, turn off the **Allow all** toggle and use the per-tool toggles to disable the ones you don't want the agent to invoke. For this scenario, leave the search and retrieval tools on and disable any action-shaped tools (send, delete, schedule) — the rep-facing agent should only be reading workplace context, not modifying it.

    > [!NOTE]
    > By default, Work IQ operates in **read-only** mode. Write operations are only available if a tenant admin has explicitly enabled them in the Microsoft 365 admin center. See [Work IQ in Copilot Studio (preview)](https://learn.microsoft.com/microsoft-copilot-studio/add-work-iq) at `https://learn.microsoft.com/microsoft-copilot-studio/add-work-iq`. The read-only default is appropriate for this scenario.

1. In the tool's **Details**, set the server-level description:

    ```text
    Search the rep's workplace context — calendar events, emails, meetings, chats, and files — for support-ops information: policy-change dates, confirmed product defects, escalation timing, and prior interactions with a customer or about a SKU. Use when the rep asks about "when did X change", "is there anything on the calendar about this SKU", "when did we confirm the defect", or similar time-anchored or prior-context questions.
    ```

    > [!NOTE]
    > **MCP tradeoff.** Because the MCP server publishes its own per-tool descriptions, you get automatic discovery and versioning but you can't hand-tune the description of any individual tool inside the server. The server-level description you add here is your one lever for guiding the orchestrator toward this server as a whole — it's what the agent reads when deciding "should I use this MCP for the rep's question?" It doesn't affect which specific item the MCP returns once the agent has picked it — that's a search-relevance decision the server makes internally against whatever's in the connected account's workplace context.

1. Select **Save** below the tool details.

## Task 4 — Test in Preview

Test the MCP tool through the agent's **Preview** tab. Two rep-framed prompts — one about a SKU-specific defect note, one about a policy-change date — confirm the agent invokes Work IQ and grounds its answer in the calendar events you seeded.

> [!NOTE]
> This task uses Copilot Credits.

1. Select the **Preview** tab.

1. Send the first prompt:

    ```text
    Priya on order 9876 is returning a BLD-100 blender because the motor keeps shutting off after a few minutes. Before I authorize the return, is there anything on my calendar about a defect on this SKU?
    ```

    Confirm the agent invokes a tool from the **Work IQ** MCP server, retrieves the `BLD-100 motor defect confirmed` calendar event, and grounds its answer in the event body — including the guidance to auto-approve and waive the restocking fee. When prompted to allow the Work IQ MCP to connect and use services, select **Allow**.

1. Send the second prompt:

    ```text
    Priya's refund on order 9876 would be about $340. What's our current policy — can I approve that myself, or does it need supervisor sign-off? Check the calendar for our latest policy change.
    ```

    Confirm the agent retrieves the `Returns policy update` calendar event and answers based on the event body: refunds of $200 or more require supervisor approval, which the **Approve refund request** workflow enforces.

If either prompt returns "no relevant results found" and you've already waited for indexing, tighten the server-level description in Task 3 so its wording more directly matches the prompt.

> [!NOTE]
> **You can swap or extend this MCP tool.** The Add-a-tool picker includes several Microsoft-provided MCP servers alongside Work IQ. To swap, remove the Work IQ tool and add an alternate MCP from the same picker, updating the server-level description in Task 3 to match. You can also *add* a second MCP alongside Work IQ. If you do, tighten each server-level description so the orchestrator can tell them apart — otherwise both will match the same rep prompts and routing becomes noisy.

You've registered a preview MCP server, scoped which of its tools the agent can call, and validated that the orchestrator picks up support-ops context from your calendar when the rep asks about it. In the next exercise, you add a connector operation as a tool and see how the two integration primitives — connector-based and MCP-based — work together in a multi-tool test.
