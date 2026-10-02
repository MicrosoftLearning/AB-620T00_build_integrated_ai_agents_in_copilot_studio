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

1. Review the default KPIs and metrics for the selected date range:

    - Total sessions and total users.
    - Average session duration and success rate.
    - User reactions and total estimated credits used.

1. Note where these numbers come from and how they'd shift as more reps come online.

## Task 2 — Review estimated Copilot Credits consumption

Copilot Credits track the metered cost of every agent turn, tool invocation, and evaluation run. Reconciling actual consumption against your per-exercise expectations is how you catch over-eager tool calls or runaway delegations before they surface as billing surprises. In this exercise, the goal is to know where the numbers live, not to match a specific set of values.

1. In the **Monitor** view, confirm the date range selector in the upper right covers your lab activity.

1. Review **Total estimated credits used** for the Customer Support Rep Assistant. The value is an estimate and may lag behind your most recent test conversations.

1. Note what the card shows:

    - If a total appears, record it. A category breakdown may or may not be shown — if the card states that a breakdown isn't available for this agent, the total alone is enough.
    - If the card shows **No credits used**, record that no usage was reported for the selected period and continue.

1. If you have access to an environment-level capacity report, optionally compare its aggregate Copilot Credits usage for the same date range. Reporting data can be delayed and can include activity from other agents or users.

1. Review the note callouts in the previous exercises to identify which tasks were flagged as using Copilot Credits, and confirm the usage you recorded is consistent with those tasks. If an exercise used noticeably more credits than expected, add it to your hardening TODO list for further review.

## Task 3 — Review transcripts

Transcripts are the ground-truth record of what actually happened in each conversation — which agent handled which turn, which tools fired, and where the agent recovered or fell back. This is where you spot patterns the aggregate KPIs hide.

1. On the **Monitor** view, in the **Sessions** table, select a session from your Microsoft Teams or Microsoft 365 Copilot testing to open its transcript. Use the **Channel** filter or select **See all** if you need to find a specific session.

1. For at least two transcripts:

    - Identify which agent handled which turn (orchestrator vs. connected agent).
    - Identify which tools fired.
    - Identify any point at which the agent hallucinated or fell back to a scope refusal.

1. Log observations in `assets/monitoring-notes.md` for delivery of continuous-improvement guidance.

You've completed Lab 5 by evaluating, publishing, and monitoring the Customer Support Rep Assistant. You reviewed aggregate KPIs, estimated Copilot Credits, and transcripts to identify where the agent needs improvement.
