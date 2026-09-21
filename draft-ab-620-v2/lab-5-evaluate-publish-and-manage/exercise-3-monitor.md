---
lab:
  title: '5.3: Monitor KPIs, credits, and transcripts for a published agent'
  description: In this exercise, you tour the Copilot Studio monitoring surfaces — the Monitor dashboard, Copilot Credits consumption, and transcripts — without spending any additional credits.
  duration: 20 minutes
  level: 300
  islab: true
  primarytopics:
    - Microsoft Copilot Studio
---

# Monitor KPIs, credits, and transcripts for a published agent

After you publish an agent, you need visibility into how often it's used, what it costs, and how well it's performing. In this exercise, you tour the monitoring surfaces available in Copilot Studio.

You will complete the following tasks:

- Review the Monitor dashboard KPIs.
- Review Copilot Credits consumption for the agent.
- Review recent transcripts and log observations.

This exercise should take approximately **20** minutes to complete.

## Before you start

This exercise builds directly on the previous exercise in this lab. Complete [Exercise 2 — Publish and deploy an agent to end-user surfaces](exercise-2-publish-and-deploy.md) first. The agent should be published and deployed to at least one surface.

## Task 1 — Review the Monitor dashboard

The **Monitor** view is where Copilot Studio surfaces aggregate usage KPIs for a published agent — sessions, engagement, resolution outcomes, and delegation patterns. Take a minute to notice which KPIs Copilot Studio surfaces out of the box; these are the numbers you'd be looking at every week once the agent is in production.

1. Open the **Customer Support Rep Assistant** agent.

1. In the top navigation menu within the agent, select **Monitor**.

1. Review the default KPIs:

    - Sessions and engaged users.
    - Resolution / escalation ratio.
    - Top intents and top delegations.

1. Note where these numbers come from and how they'd shift as more reps come online.

## Task 2 — Review Copilot Credits consumption

Copilot Credits track the metered cost of every agent turn, tool invocation, and evaluation run. Reconciling actual consumption against your per-exercise expectations is how you catch over-eager tool calls or runaway delegations before they surface as billing surprises.

1. Open the environment's Copilot Credits view (or the equivalent capacity report).

1. Locate the Customer Support Rep Assistant's consumption over the lab run.

1. Break down by:

    - Model calls (orchestrator vs. connected agents).
    - Tool invocations (workflows, connector, MCP).
    - Evaluation runs.

1. Compare against the per-exercise `> **Note:** ... uses Copilot Credits` callouts in the lab exercises. Flag any exercise that came in materially higher than planned as a hardening TODO.

## Task 3 — Review transcripts

Transcripts are the ground-truth record of what actually happened in each conversation — which agent handled which turn, which tools fired, and where the agent recovered or fell back. This is where you spot patterns the aggregate KPIs hide.

1. In the **Monitor** view (which surfaces recent transcripts), open a recent transcript from Preview, Teams, or M365 Copilot testing.

1. For at least two transcripts:

    - Identify which agent handled which turn (orchestrator vs. connected agent).
    - Identify which tools fired.
    - Identify any point at which the agent hallucinated or fell back to a scope refusal.

1. Log observations in `assets/monitoring-notes.md` for delivery of continuous-improvement guidance.

You've toured the three surfaces Copilot Studio provides for tracking a published agent — aggregate KPIs on the Monitor dashboard, credit consumption reconciled against your per-exercise expectations, and transcripts that show what actually happened turn by turn. In the next exercise, you close with ALM — environment variables and Power Platform Pipelines — plus one final candidate-model evaluation.
