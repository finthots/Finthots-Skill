# Finthots-Skill

Agent skills for FinThots Interactive Learning Solutions LLP — BFSI training and eLearning.

Each folder is a self-contained skill: a `SKILL.md` with YAML frontmatter (`name`,
`description`) plus reference files loaded on demand.

## Skills

| Skill | What it does |
|---|---|
| [`finthots_client_leads`](./finthots_client_leads/) | Builds and enriches outbound B2B lead lists for BFSI training sales. Harvests free regulator sources first, audits provider credits and plan entitlements before spending, maps the full buying committee, and sequences outreach L&D → credit → compliance. |

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

**2026-11-05** — Rewritten after a live 49-company run. Added `references/data-sources.md`
(free regulator sources, real per-action credit costs, verified plan entitlements) and
`references/messaging.md` (sequence rationale, per-persona openers, real proof points).
Key lessons folded in: harvest free sources before spending; search broad and reveal narrow;
resolve company IDs before filtering; exclude group entities from title sweeps; treat every
reveal as a correction to the current employer; do not cap the primary persona.

**2026-11-04** — Initial version. Removed hardcoded lender lists in favour of run-time
discovery.
