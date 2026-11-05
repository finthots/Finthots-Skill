# Finthots-Skill

Agent skills for FinThots Interactive Learning Solutions LLP — BFSI training and eLearning.

Each folder is a self-contained skill: a `SKILL.md` with YAML frontmatter (`name`,
`description`) plus reference files loaded on demand.

## Skills

| Skill | Stage | What it does |
|---|---|---|
| [`finthots_client_leads`](./finthots_client_leads/) | Find the buyer | Builds and enriches outbound B2B lead lists for BFSI training sales. Harvests free regulator sources first, audits provider credits and plan entitlements before spending, maps the full buying committee, and sequences outreach L&D → credit → compliance. |
| [`finthots_sales_collateral`](./finthots_sales_collateral/) | Pitch them | Decides the pitch angle for a given buyer and builds the client-facing assets — case studies, capability decks, workshop menus, demo reel scripts, skill assessments, cost models. Thirteen angles with proof status; no invented outcome numbers. |

The two are a pipeline: `finthots_client_leads` finds the buyer and writes the per-contact
message, `finthots_sales_collateral` builds the reusable assets that message points at. The
collateral is segment-agnostic — the same case study serves housing finance, vehicle finance
and microfinance, so it is built once and not rebuilt per campaign.

## Installing

Clone into your agent's skills directory, or point the agent at this repo:

```bash
git clone https://github.com/finthots/finthots-skill.git
```

## Conventions

- `SKILL.md` frontmatter carries only `name` and `description`. The description decides when
  the skill fires, so it names its triggers explicitly.
- Detail lives in `references/`, loaded on demand rather than up front.
- No company lists are hardcoded — lender universes are discovered from the live regulator
  register at run time, because Indian lenders merge, rebrand and enter resolution constantly.
- No credentials, API keys or customer contact data in this repo.

## Changelog

**2026-11-05** — Added `finthots_sales_collateral`: thirteen pitch angles (six from Manish,
seven added) each carrying a READY / NOT YET proof status, the angle-by-buyer mapping, specs
for eight client-facing assets, the seven-step CTA ladder, and the documented proof points in
`references/proof.md`. Built as a separate skill rather than merged into the lead-gen skill
because the triggers and outputs differ and the assets serve every segment, not one campaign.
The governing rule: never invent an outcome number — "made mandatory for every new joiner"
beats a fabricated percentage and cannot be challenged.

**2026-11-05** — Rewritten after a live 49-company run. Added `references/data-sources.md`
(free regulator sources, real per-action credit costs, verified plan entitlements) and
`references/messaging.md` (sequence rationale, per-persona openers, real proof points).
Key lessons folded in: harvest free sources before spending; search broad and reveal narrow;
resolve company IDs before filtering; exclude group entities from title sweeps; treat every
reveal as a correction to the current employer; do not cap the primary persona.

**2026-11-04** — Initial version. Removed hardcoded lender lists in favour of run-time
discovery.
