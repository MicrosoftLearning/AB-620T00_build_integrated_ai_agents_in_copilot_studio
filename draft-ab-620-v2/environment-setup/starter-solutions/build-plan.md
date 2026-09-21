# Starter solution build plan (testing companion)

**Purpose.** This is the testing playbook for building the shared starter solution zips **while** testing the labs. Learners never see this file — it's the sequenced set of "hand-provision this before you test the next lab, then export this zip after" instructions that keep the export flow attached to the natural test flow.

**Audience.** Whoever is doing a testing pass and can produce the zips at the same time. Read [`../../TESTING-GUIDE.md`](../../TESTING-GUIDE.md) first for context on priorities and known gaps, then work through this doc top-to-bottom as you test each lab.

**The zip set.** Two formats, one shared set that every learner uses regardless of delivery mode.

- **`additive-components.zip`** — course-level, produced once. Contains only the Dataverse tables that Labs 2–5 need but no lab teaches learners to build by hand. No publisher, no solution, no learner-produced work. Sequential learners import this once at Lab 2 Ex 0 and never touch a zip again.
- **`lab-2-starter.zip` through `lab-5-starter.zip`** — cumulative catchup zips for learners starting fresh at that lab. Each contains everything a prior-lab sequential learner would have plus the additive components.

All zips are unmanaged and use publisher `Support` / prefix `sup`. Once every zip is exported and committed, this doc becomes redundant. Delete or archive at that point.

## How to use this doc

Work top-to-bottom. Each **phase** matches one lab you're about to test, plus a Phase 0 for the course-level additive zip.

Each phase has up to three sub-sections, always in the same order:

1. **Pre-lab hand-provision** — the setup step(s) that *should* be in the lab's Ex 0 but aren't yet (because the zip doesn't exist yet). Do these before you start the lab exercises.
1. **Test the lab** — pointer to the exercises you're running. No new instructions.
1. **Export step** — what to export at the end of this phase.

If a phase says "**No pre-lab hand-provision needed**," it means the exercises as written are self-contained for that lab.

## Naming and prefix invariants (do this once, before Phase 0)

Every starter zip depends on the publisher being **Support** / prefix **sup**. If the environment defaults to a different publisher (common — many tenants default to `cr_`), everything downstream breaks. Confirm before you touch any component.

1. In Copilot Studio, in the left navigation, select the ellipsis (**...**) next to your account name and then select **Solutions**. (Note: this may reroute through the Classic Power Apps experience once — repeat the selection if so.)
1. If a **Customer Support Rep Assistant** solution already exists, open it and confirm publisher = **Support**, prefix = **sup**. If not, either create it now per Lab 1 Ex 2, or change the tenant default publisher.
1. Confirm the tenant identity you're testing under is the same one whose email you'll use as `customerEmail` / `supervisorEmail` in Lab 2 Preview tests.

---

## Phase 0 — Build `additive-components.zip`

This is a course-level one-time export. It's the package that sequential learners import at Lab 2 Ex 0 Task 1 to pick up everything Labs 2–5 need beyond what they build in Lab 1.

**Contents:**

- **Returns** table (empty).
- **Orders** table (seeded from `../../sample-data/orders.csv`; contains order **9876**).
- **Customer Records** table (seeded from `../../sample-data/customer-records.csv`; contains `priya@contoso.com`).
- **Products** table (seeded from `../../sample-data/products.csv`; contains SKU **BLD-100**).
- **No publisher, no solution wrapper metadata, no orchestrator agent, no specialist agent, no workflows, no knowledge sources, no MCP tool, no connector-as-tool.** Sequential learners already have publisher `Support`/`sup` and the orchestrator agent from Lab 1, and they create the Fulfillment specialist live in Lab 4 Ex 1.

**How to produce:**

Produce this in a **second, clean environment** (not the environment you're using to test the labs sequentially). Doing it in a clean env keeps this zip minimal — no chance of accidentally including in-progress orchestrator work.

1. In the clean env, create the `Support` publisher (prefix `sup`) and a scratch solution to house the components (name doesn't matter — you're extracting the components, not shipping the solution wrapper). Set as preferred.
1. Add all four tables per [`../dataverse-manual-provisioning.md`](../dataverse-manual-provisioning.md). Seed Orders, Customer Records, and Products from the CSVs. Leave Returns empty.

    > [!NOTE]
    > The manual-provisioning doc is written from a learner's perspective and references the Lab 1 **Customer Support Rep Assistant** solution — substitute your scratch solution wherever it appears. Publisher and prefix (`Support`/`sup`) still apply.
1. Publish all customizations.
1. Now build a **second solution in the same env**, this one named to become the shipping artifact. Use publisher `Support`/`sup`.
1. Inside the shipping solution, use **Add existing** to reference all four tables. Don't include the publisher itself — you want sequential learners' existing `Support` publisher to be reused on import, not overwritten.
1. Publish all customizations on the shipping solution.
1. Export the shipping solution as **Unmanaged** → save as `additive-components.zip` in `AB-620 Labs/environment-setup/starter-solutions/`.

**Verification:**

- Import into a fresh third environment that has publisher `Support`/`sup` and the Lab 1 end-state (Customer Support Rep Assistant solution with the orchestrator agent). Confirm the import succeeds without publisher conflicts, all four tables land in the solution, and the orchestrator agent is untouched. This simulates the sequential learner path.

---

## Phase 1 — Test Lab 1

**Pre-lab hand-provision:** none. Lab 1 starts from a fresh environment by design.

**Test the lab:**

- Complete Lab 1 Ex 1 through Ex 4 as written (`lab-1-design-and-configure-agent/`).

**After Lab 1: import `additive-components.zip` into the testing environment.**

This is what a sequential learner does at Lab 2 Ex 0 Task 1. Doing it during your testing pass keeps the environment honest for Phases 2–5 — the tables that Labs 3–5 assume exist have to actually be there.

1. In the testing env, in **Solutions**, select **Import solution**.
1. Browse to `additive-components.zip` and import. Wait 2–3 minutes.
1. Open the **Customer Support Rep Assistant** solution and select **Publish all customizations**.
1. Confirm the four tables are now in the solution.

**Export `lab-2-starter.zip`:**

The cumulative zip for fresh learners jumping in at Lab 2. It needs the Lab 1 end-state (publisher, solution, orchestrator agent) plus everything from `additive-components.zip`.

1. Confirm the solution now contains: publisher `Support`/`sup`; the orchestrator agent (identity, instructions, skills, memory, safety); all four tables.
1. Publish all customizations on the solution.
1. Export the solution as **Unmanaged**: **Solutions** → **Customer Support Rep Assistant** → ellipsis (**...**) → **Export solution** → **Unmanaged**.
1. Save the file as `lab-2-starter.zip` in `AB-620 Labs/environment-setup/starter-solutions/`.

**Sanity check before moving to Phase 2:** the testing env has the orchestrator agent + all four tables. The `lab-2-starter.zip` zip contains that full state.

---

## Phase 2 — Test Lab 2

**Pre-lab hand-provision:** none — you imported `additive-components.zip` at the end of Phase 1, so the Returns table (and the rest of the additive set) is already in the testing env.

> If you're jumping straight into Phase 2 without having tested Lab 1 in this environment, import `lab-2-starter.zip` first. That single import gives you Lab 1's end-state plus the additive components.

**Test the lab:**

- Complete Lab 2 Ex 0 through Ex 3 as written (`lab-2-add-workflows-to-agent/`).
- Ex 0 Task 1 ("Import the components you need") — skip; you already have the state from Phase 1's post-lab import.

**Export `lab-3-starter.zip`:**

The cumulative zip for fresh learners jumping in at Lab 3. No new hand-provisioning — the additive tables Orders and Customer Records are already in the solution from Phase 1's additive import. Lab 2 just added two workflows on top.

1. After Lab 2 Ex 3 passes, publish all customizations.
1. Export as **Unmanaged** → save as `lab-3-starter.zip` in `AB-620 Labs/environment-setup/starter-solutions/`.

**Sanity check:** solution now contains everything in `lab-2-starter.zip` + Create return authorization workflow + Approve refund request workflow (both attached to the agent as tools).

---

## Phase 3 — Test Lab 3

**Pre-lab hand-provision:** none — Orders and Customer Records were in the additive import at end of Phase 1.

> If jumping in, import `lab-3-starter.zip`.

**Test the lab:**

- Complete Lab 3 Ex 0 through Ex 3 as written (`lab-3-integrate-knowledge-and-tools/`).
- Ex 4 (Learn MCP) is optional. Skip it for the initial zip build so the zip stays minimal — you can layer it back in when demoing.
- Ex 0 Task 1 (starter import) — skip.

**Export `lab-4-starter.zip`:**

No new hand-provisioning — the Products table is already in the solution from Phase 1's additive import. Lab 3 added the MCP tool, connector-as-tool, and Dataverse knowledge sources on top.

1. After Lab 3 Ex 3 passes, publish all customizations.
1. Export as **Unmanaged** → save as `lab-4-starter.zip`.

**Sanity check:** solution now contains everything in `lab-3-starter.zip` + MCP tool + Outlook connector-as-tool + Dataverse knowledge sources.

> **Design decision (2026-09-13, revised).** Learners create the Fulfillment specialist from scratch in Lab 4 Ex 1. Providing it pre-built in the starter zip added zero pedagogical value (creating an empty agent shell is trivial mechanics from Lab 1) and forced sequential learners to carry an unused shell through Labs 2–3. Removing the shell from `additive-components.zip` simplifies the additive import, makes Lab 4 self-contained, and reinforces the Lab 1 agent-creation skill on the way to configuring a specialist. (Prior 2026-09-13 decision was to ship the shell in `additive-components.zip`; this revision reverses that. Even-earlier design carried Product info + Returns and refunds specialists, replaced 2026-09-13 by a single cross-functional Fulfillment specialist.)

---

## Phase 4 — Test Lab 4

**Pre-lab hand-provision:** none — Products was in the additive import at end of Phase 1.

> If jumping in, import `lab-4-starter.zip`.

**Test the lab:**

- Complete Lab 4 Ex 0 through Ex N as written (`lab-4-configure-multi-agent-solution/`).
- Ex 0 Task 1 (starter import) — skip.

**Export `lab-5-starter.zip`:**

The zip needs Lab 4's end-state. That means the Fulfillment agent is wired as a **connected agent** with Products knowledge and the **Create shipment request** workflow attached, and the `product-troubleshooting` skill is enriched with the agent-agnostic triage outcomes from Lab 4 Ex 4.

1. After Lab 4's last exercise passes, publish all customizations.
1. Export as **Unmanaged** → save as `lab-5-starter.zip`.

**Sanity check:** solution contains the full stack — orchestrator with skills/memory/safety/knowledge/MCP/connector-as-tool + both return/refund workflows attached + Fulfillment agent (connected, with Products knowledge + Create shipment request workflow attached) + all four Dataverse tables.

---

## Phase 5 — Test Lab 5

**Pre-lab hand-provision:** none.

**Test the lab:**

- Complete Lab 5 Ex 0 through Ex 4 as written (`lab-5-evaluate-publish-and-manage/`).
- Ex 0 Task 1 (starter import) — skip.

**No export needed after Phase 5.** `lab-5-starter.zip` was already produced at the end of Phase 4, and that's the last zip in the shared-starter set. Lab 5 has no downstream lab to onboard.

---

## Export conventions

**Unmanaged is the shipping format.** All four zips ship as **unmanaged**. Reasons: (a) learners can inspect and edit imported components inline during downstream labs, matching how they'd edit their own work; (b) unmanaged avoids solution-layering complexity that can subtly change UI options in Copilot Studio; (c) it matches maker-time authoring, which is what learners are otherwise doing. Managed solutions are for release/production distribution, which these labs aren't targeting.

**Publisher and prefix are always `Support` / `sup`** — the same publisher learners create in Lab 1 Ex 2. This keeps all schema names (`sup_returns`, `sup_orders`, etc.) identical whether a component was built by hand or imported, so learners see one consistent world.

**Where to save.** `AB-620 Labs/environment-setup/starter-solutions/` — commit the zips there. Update `environment-setup/starter-solutions/README.md` status line from "pending" to "available" once the first set lands.

**Verification.** After each export, import the zip into a clean second environment and run through **that lab's** exercises end-to-end. If they pass, the zip is good. If they don't, the export was incomplete or the environment lost publisher/prefix alignment (see the invariants section above).

**Rebuild if the storyline references change.** If the scenario email (`priya@contoso.com`), order ID (`9876`), or SKU (`BLD-100`) changes in the course narrative, regenerate `orders.csv` / `customer-records.csv` / `products.csv` in `sample-data/` first, then rebuild every zip that contains those seed rows (Phases 2, 3, 4).

---

## What to update after all zips are built

Once all five zips exist and verify cleanly, do these to close out the pre-release TODO:

- **`environment-setup/starter-solutions/README.md`** — flip the status line from "pending" to "available" and add the actual file names (`additive-components.zip` plus `lab-2-starter.zip` through `lab-5-starter.zip`).
- **Each lab's Ex 0 Task 1** — no changes needed; the exercises already assume the zips exist.
- **`environment-setup/README.md`** — no changes needed.
- **This doc** — archive or delete. It only exists for the testing pass that produces the zips.
