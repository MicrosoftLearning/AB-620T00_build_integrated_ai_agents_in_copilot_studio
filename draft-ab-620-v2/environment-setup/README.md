# Environment setup for AB-620 v2 labs

This folder contains everything a learner or trainer needs to bring a Copilot Studio environment to lab-ready state **when a pre-provisioned Skillable Valorem instance isn't available**.

## When you need these docs

Use the docs in this folder if any of the following is true:

- **Modular / self-paced delivery.** The learner is jumping into Lab 2, 3, 4, or 5 without having completed the prior labs in the same environment, and the starter solution zip described in `starter-solutions/README.md` isn't available yet.
- **Bring-your-own-tenant delivery.** A trainer is running ILT on their own Copilot Studio dev environment (not Skillable) and needs to hand-provision the Dataverse tables.
- **Dataverse is blocked in the target environment.** Very rare, but if it happens, use the SharePoint list fallback documented in `sharepoint-fallback.md`.
- **Recovery.** A learner's solution import failed and they need to build the same schema by hand to unstick.

If **none** of the above applies (you're in a pre-provisioned Skillable Valorem instance or you're importing the starter solution zip), you can skip this folder entirely. Lab 2 Ex 1 tells you when to visit it.

## Files

| File | Purpose |
|---|---|
| `starter-solutions/README.md` | The primary path. Points at the solution zips that create the Dataverse tables in one click, plus a status note if the zips are pending. |
| `dataverse-manual-provisioning.md` | Click-by-click steps for creating the Returns, Orders, Customer Records, and Products Dataverse tables by hand. Fallback path when the starter zip is unavailable. |
| `sharepoint-fallback.md` | Click-by-click steps for creating SharePoint lists that mirror the Dataverse tables. Fallback path when Dataverse itself is unavailable. Compromises the Lab 5 ALM story — see the doc for what to expect. |

## Recommended path (in order of preference)

1. **Skillable Valorem pre-provision.** The instance already has the tables. Nothing to do.
2. **Starter solution zip.** `Solutions → Import solution → lab-N-starter.zip`. ~2 min. This is the intended path for modular delivery and the pattern used by every published Power Platform lab (see PL-400 Lab 01 as the archetype).
3. **Hand-provision (Dataverse).** Only when the zip is unavailable. See `dataverse-manual-provisioning.md`.
4. **Hand-provision (SharePoint).** Only when Dataverse is unavailable. See `sharepoint-fallback.md`. This path breaks the Lab 5 ALM narrative — flag it for the learner.

## Why Dataverse is the primary path (and why we don't just use SharePoint)

Dataverse is the canonical Power Platform primitive for structured data inside a Copilot Studio solution. Every published Copilot Studio lab uses it. It matters here for three reasons:

- **Solution-scoped.** Dataverse tables live inside the `Customer Support Rep Assistant` solution and travel with it across dev → test → prod in Lab 5's pipeline exercise. SharePoint lists don't.
- **Native knowledge grounding.** Copilot Studio's `Add knowledge` picker treats Dataverse tables as a first-class source (Lab 3 Exercise 2). SharePoint requires the Copilot connector, a slightly different UX path.
- **Realistic to production.** Reps in the field would consume from a real Dataverse table, not a SharePoint list. The scenario is more coherent when the labs mirror that.

SharePoint remains a documented fallback because some Skillable configurations block Dataverse table creation. When we take that path, we lose part of the ALM story in Lab 5 — the fallback doc calls this out.
