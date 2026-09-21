# Testing guide — AB-620 v2 labs

**Audience:** anyone validating the AB-620 v2 lab set (internal lab tester, Skillable publishing prep, or later reviewer).
**Read time:** ~10 minutes. Read this once end-to-end before you touch the labs, then keep it open as a reference.

---

## What you have

Five hands-on labs for **AB-620 v2 — Design and build integrated AI agent solutions in Microsoft Copilot Studio**, targeting the GitHub Copilot (GHCP) authoring harness.

| Folder | What it holds |
|---|---|
| `lab-1-design-and-configure-agent/` through `lab-5-evaluate-publish-and-manage/` | Learner-facing exercise markdown, one folder per lab. Each lab folder has a `README.md` (learner orientation) and numbered exercise files. |
| `environment-setup/` | Everything about **getting the environment lab-ready** — starter solution zips (primary path), Dataverse manual provisioning (fallback), SharePoint list fallback. |
| `environment-setup/starter-solutions/build-plan.md` | **The sequenced playbook for testing + producing the starter zips at the same time.** This is your day-to-day testing companion. |
| `sample-data/` | Shared seed CSVs (Orders, Customer Records, Products, Returns schema, FAQ content). Imported into Dataverse tables during setup — you never type these rows by hand. |

---

## Important context before you start

### 1. Labs are **cumulative**, not modular

Each lab builds on the previous one's end-state (same solution, same tables, same agent). You complete Lab 1 → Lab 2 → Lab 3 → Lab 4 → Lab 5 in **the same environment**, in order. State persists across labs — you don't re-import between them.

Modular delivery (jumping in at Lab 3, for example) is designed to be supported by **starter solution zips** that snapshot each lab's end-state. **Those zips don't exist yet.** Producing them is a lower-priority secondary goal of your testing pass — see priority #3 below.

### 2. The zips don't exist yet → use the build plan

Because the starter zips don't exist yet, every "Exercise 0 — Set up starter state" step that says "import `lab-N-starter.zip`" needs a substitute. That substitute is:

- Sequential testing (Lab 1 → 2 → 3 → 4 → 5 in one environment): **skip the import steps.** State persists from the previous lab.
- Anything else: hand-provision using [`environment-setup/dataverse-manual-provisioning.md`](environment-setup/dataverse-manual-provisioning.md), importing seed rows from [`sample-data/`](sample-data/) via **Get data → From text/CSV**.

The full end-to-end recipe — what to hand-provision when, when to skip an Ex 0 import step, and when to export a zip — is [`environment-setup/starter-solutions/build-plan.md`](environment-setup/starter-solutions/build-plan.md). **Read it once before starting Lab 1**, then follow its phase-by-phase instructions as you work through each lab.

### 3. What Bre hasn't tested yet

Bre has not made it fully through all labs end-to-end. Areas that need the most attention:

- **Lab 3 Ex 3 — Connector-as-tool.** Author-time flow works; multi-tool orchestration in Preview is not fully validated.
- **Lab 5 Ex 4 — ALM (env variables, pipelines, dev → test → prod).** Recently rewritten for realism (single solution across three environments, not solution-hopping); the mechanics are documented from Microsoft Docs but haven't been walked in a live environment.
- **Dataverse scenario specifics.** Column casing, choice-vs-text fallbacks, and workflow references to logical names. The manual-provisioning doc calls out where variance is expected; flag anything you see drift.

**What "walked in a live environment" means here.** Most of the rest has been tested for **core capability + Copilot Credits behavior** — Bre confirmed the primitives work in the environment and burn credits as expected. What has **not** been fully validated is:

- **Scenario end-to-end coherence.** Whether the way the labs instruct learners to build the agent, step-by-step, actually produces an agent that behaves correctly and produces the right outputs at the scenario level. Scenario cohesion has been reviewed at the **content** level (does the story hold together on paper) but not at the **runtime** level (does the finished agent actually do the thing).
- **UI references.** Bre has updated many but not all UI-drift issues she encountered. Expect more.

So Priority 1 below is really two things: verify UI references, **and** verify the scenario holds up when you actually build and run it end-to-end.


---

## Your priorities (in order)

### Priority 1 — Verify UI references and end-to-end functionality

- Do the click paths in each exercise match the current Copilot Studio UI? Menu names, button labels, panel positions.
- Do the workflows, knowledge sources, and tools all wire together the way the exercises describe?
- Does the scenario hold up across labs?
- Do the Preview test prompts return the behavior the exercise claims they will?

**Update or log every UI drift, broken step, or scenario break.** These are the highest-value findings.

### Priority 2 — Style and formatting improvements (lower priority)

Style issues can be handled after Bre's handoff to Skillable during their publishing prep pass. If you spot something obvious (typos, awkward phrasing, missing screenshot placeholder), feel free to fix it — but reliable steps are more important initially than the prose and style of the content.

### Priority 3 — Produce the starter solution zips (if time allows)

This is the "modular delivery unlock" — once these zips exist, learners can jump into Lab 2, 3, 4, or 5 without having done the prior labs. Full instructions for producing them are in [`environment-setup/starter-solutions/build-plan.md`](environment-setup/starter-solutions/build-plan.md).

Producing the zips fits naturally into your testing pass: at the end of each lab, export the current solution as an unmanaged zip. The build plan tells you exactly what to export and where to save it.

---

## The recommended testing flow

The short version:

1. **Read this guide, then read [`environment-setup/starter-solutions/build-plan.md`](environment-setup/starter-solutions/build-plan.md).** The build plan is the phase-by-phase testing playbook and takes ~10 minutes to skim.
2. **Do the one-time "publisher and prefix" check** at the top of the build plan (before Phase 0). Every downstream zip depends on it.
3. **Phase 0 — produce `additive-components.zip`** in a separate clean environment. Follow the build plan. This is the one course-level artifact — everyone gets it before Lab 2.
4. **For each lab, in order:**
   - Follow the "Pre-lab hand-provision" step in the build plan for that lab (if any).
   - Run the lab's exercises end-to-end using the learner-facing markdown in the lab folder.
   - Skip the "Import the starter zip" step in Exercise 0 — the state carries from the previous lab.
   - At the end, follow the build plan's "Export step" to produce that lab's cumulative starter zip (`lab-N-starter.zip`).
5. **Make necessary updates and/or log findings as you go** 

Expected setup work per lab boundary is ~15 minutes (mostly Dataverse table creation and CSV import). Everything else is exercise execution.

---

## How the pieces fit together

```
┌──────────────────────────────────────────────────────────────────────┐
│  environment-setup/starter-solutions/build-plan.md                   │
│  = the sequenced testing playbook. Follow top-to-bottom.             │
│                                                                      │
│  ┌──────────────────────────┐  ┌───────────────────────────────────┐│
│  │  sample-data/*.csv       │  │  environment-setup/               ││
│  │  = seed rows.            │  │  dataverse-manual-provisioning.md ││
│  │  Import via Dataverse    │  │  = click-by-click table schema.   ││
│  │  "Get data → From CSV".  │  │  Follow when zips aren't          ││
│  │  You never type these.   │  │  available (which is: today).     ││
│  └──────────────────────────┘  └───────────────────────────────────┘│
│                                                                      │
│  Each lab folder = learner-facing exercise markdown. Follow the      │
│  exercises as written. When Exercise 0 says "import the zip,"        │
│  the build plan tells you what to do instead.                        │
└──────────────────────────────────────────────────────────────────────┘
```

Rule of thumb: if you're wondering where to start, open the build plan. It's the tester's single entry point.

---

## Fictitious content standard

Learner-facing content uses `contoso.com` with first-name-only usernames (`priya@contoso.com`, `manager@contoso.com`). This matches Microsoft's CELA-approved fictitious-content guidance. If you add or change any storyline persona while testing, keep the same pattern — don't invent new domains or use `firstname.lastinitial@` formatting.

---

