---
name: finthots_client_leads
description: Build and enrich an outbound B2B lead list for FinThots BFSI training and eLearning sales. Use when asked to find leads, find decision makers, build a prospect list, target a lending segment (HFC, business loan, LAP, SME, vehicle finance, microfinance, gold loan, bank, NBFC), enrich contacts with emails or phone numbers, or audit outbound data before a campaign. Always exhausts free regulator sources before spending a credit.
---

# FinThots Client Leads

Builds outbound lead lists for FinThots Interactive Learning Solutions LLP, which sells
functional training, eLearning and blended learning to Indian BFSI lenders.

The job is never "find contacts". It is: pick a lending segment, discover who is actually
in it right now, map every person who buys or influences training at those companies, and
get their contact details at the lowest possible cost.

**The governing lesson from live runs: the free sources are better than the paid ones for
roughly half of what you need.** A regulator directory gave 90 companies' named officers
with direct emails and mobile numbers for nothing, after the paid-tool plan had been built
on the assumption that phone numbers could only be bought. Always run Step 2 before Step 3.

## Step 1 — Confirm the segment

Ask which client type to target. Do not assume. The segment table, the definition of each,
and the live register that governs it are in `references/segments.md`. Ask in the same turn:
how many companies, pan-India or specific states, which vertical where a segment sits inside
larger lenders, and which companies are already FinThots clients.

**Never work from a company list held in memory.** Indian lenders merge, rebrand, enter
resolution and launch constantly. A remembered list silently drops new entrants and keeps
companies that no longer exist under that name — both failures are invisible in the output.
Build the universe fresh from the regulator register every run.

## Step 2 — Harvest the free sources FIRST

Do this before touching any paid provider. Full detail in `references/data-sources.md`.

| Source | What it gives | Cost |
|---|---|---|
| **NHB nodal officer directory** | Name, designation, **direct email and mobile** for a named officer at **every registered HFC** | Free |
| NHB asset-size list | Every HFC with AUM — the only honest basis for tiering | Free |
| Stock-exchange SMP filings (NSE/BSE) | CRO, CCO, CISO, Company Secretary appointments with exact titles and effective dates | Free |
| Company KYC/AML policy PDFs | Principal Officer (a statutory named role) | Free |
| HR trade press (hrkatha, People Matters) | L&D appointments at larger lenders | Free |
| The user's own sheet | Who has already been worked, pitched or lost | Free |

Search for the NHB directory as "NHB Annexure Details of first Point of Contacts PoCs Nodal
Officers of HFCs" — it is republished periodically, so fetch the newest edition, never a
remembered URL.

A nodal officer is not an L&D head. They are a senior, named, reachable person who sees every
complaint and therefore knows which process is failing — which makes them the cheapest
internal referral to whoever owns the training budget.

## Step 3 — Credit audit before any spend

Check every provider's balance **and renewal or expiry date**, show one table, ask go/no-go.
The free check tools and the real per-action costs are in `references/data-sources.md`.

Classify each balance, because it changes the advice: **monthly renewing** (use it or lose
it), **one-off grant** (gone forever, note the expiry date prominently), **annual pool
granted monthly** (steady rate).

Then state what the run will cost by provider and **ask.** If declined, build the list with
names, titles and LinkedIn URLs and hand back an enrichment queue.

**Plan entitlements differ from credit balances.** A provider can hold a large balance and
still refuse the tool you need. Verified live: Apollo's free plan blocks agent people search
and bulk enrichment entirely while showing 175 credits; Crustdata's contact-enrich endpoint
is disabled on the free plan, so its credits cannot buy emails at all; RocketReach's monthly
person-search quota exhausts long before its lookup credits do. Check entitlements, not just
balances — Crustdata exposes them free via its account-endpoints tool.

## Step 4 — Tier the companies

| Tier | Definition | Personas to cover |
|---|---|---|
| 1 | AUM > ₹25,000 cr or headcount > 5,000 | L&D + digital learning + credit/business + risk + compliance + TA + procurement |
| 2 | AUM ₹5,000–25,000 cr | L&D or HR head + credit head + compliance officer |
| 3 | AUM < ₹5,000 cr | MD or CBO + HR head + credit or business head |

If AUM is unavailable, tier on headcount, or on whether a dedicated L&D function exists at
all — its absence is itself a Tier 3 signal.

## Step 5 — Map the buying committee

Target buckets, not individuals. Titles, the tiering rule and who to skip are in
`references/personas.md`.

**Do not cap the primary persona.** Take every L&D person found at a company, not two. The
size of a company's L&D team is itself sales intelligence: 11–18 training people means a real
department with a budget and a content pipeline to feed — a library buyer. One person means a
part-time trainer who needs the whole thing built for them. Capping the list destroys that
signal. Cap the secondary personas at two and the rest at one.

**Where no L&D role exists, that is the answer, not a gap.** At smaller lenders HR or the
business head owns training. Pull the CHRO instead and move the executive up the sequence.

## Step 6 — Search broad, reveal narrow

This is where the money is saved. Searching is cheap; revealing is not.

- Lusha contact search costs **1 credit per 25 results**, so one call with 49 company names
  and a title list sweeps an entire segment for 2 credits. Email reveal is 1 credit, **phone
  is 5** — always pass `reveal: ["emails"]`, or the default reveals phones and costs 5× more.
- Crustdata people search is ~0.25 credits per result. Its company-name filter does not
  match stored names: **resolve companies to IDs first with company-identify, which is free**,
  then filter on company_id. A name filter returns zero and tells you so at no charge.
- **Exclude group and conglomerate entities from title sweeps.** Verified live: including a
  parent conglomerate returned 88 of 172 results from the group's own L&D team, crowding out
  every mid-size target. Sweep the lending entities, then the parents separately.
- Paginate. A first page is not the answer; check the total count.

Reveal order when credits are short: named L&D heads → CHRO at Tier 1-2 companies with no
L&D contact → MD/CBO at Tier 3 only. Never reveal a mobile for a large-company CEO.

### The free stack — use it before paid reveals

Hunter.io (~50/mo, best for **confirming a company's email pattern**), GetProspect (~50
emails + 100 verifications, the largest free verification allowance), ZeroBounce/BounceZero
(~100 on signup), Snov.io (~50), Prospeo and Skrapp (~50-75 each). Stacked and renewing
monthly, this exceeds any single paid tool's free tier.

**Check for plugins before assuming a handoff.** These are not in the *connector* registry,
but several are published as **plugins** with their own MCP servers — search the plugin
catalogue, not just connectors. Verified available: **Hunter** (partner-published, with
`domain-search`, `email-finder`, `email-verifier` and `check-usage` skills),
**ClearBounce** (`verify_email`, `verify_bulk`, `find_email`, `check_credits`) and
**FullEnrich** (waterfall across 25+ providers). A plugin turns the handoff below into a
direct call — always look first.

Where no plugin is installed, fall back to the handoff. Build two extra tabs:

- **PATTERN_QUEUE** — one domain per line, for companies with fewer than two confirmed
  addresses. Hunter Domain Search returns the pattern, by plugin or by browser.
- **VERIFY_QUEUE** — plain one address per line, no headers. Everything in
  `Pattern_email_UNVERIFIED` goes here, ready to paste into any bulk verifier.

With a plugin, run these directly. Without one, the user runs the tool and attaches the CSV;
either way the skill merges results, promoting only verified addresses into `Email`.

**Order matters:** Hunter domain search *first* to confirm the pattern, *then* derive, *then*
bulk verify, *then* promote. Verified live: deriving without a confirmed pattern produced
wrong addresses at three companies — a numbered alias, a group domain, and a different
registered domain. A Hunter domain search would have caught all three for one credit each.

## Step 7 — Treat every revealed record as a correction, not just an addition

Reveals carry the current employer. **Check it against the target company every time.**
Verified live: two L&D heads in the pipeline had left their companies months earlier, one to
an insurer and one to a different lender. Flag them, record where they went, and mark the
company as having no L&D head again.

**Pattern-derived emails fail in ways the pattern cannot see.** Verified live failures:
a numbered alias (`shreya.shah2@`), a group domain instead of the entity domain
(`@herofincorp.com`, not `@herohousingfinance.com`), and an entirely different registered
domain (`@sgrlimited.in`, not `@sitaara.in`). Derive only where two or more confirmed
addresses agree, keep derived addresses in a separate clearly-marked column, never in the
Email column, and verify before sending. A bounce run damages the sending domain.

**"Chief Manager" and "Deputy Chief" are Indian banking grades, not C-suite.** Seniority
sorting that treats them as CXO will rank a Chief Manager above an actual Head of L&D.

## Step 8 — Output

An .xlsx. Schema in `references/schema.md`. Tabs: READ_ME, TRACKER (the working sheet, one
row per contact, sequenced by touch), MESSAGES (full copy-paste text per contact),
COMPANY_PLAN, and a credit report.

Sequence per company: **touch 1 = L&D** (holds the library budget), **touch 2 = credit or
business head** (feels the pain, creates the demand that forces L&D to spend), **touch 3 =
compliance** (annual, non-discretionary, shortest cycle), then HR, then the verified nodal
officer, then the executive. At Tier 3 the executive moves up. **No Tier 1 or 2 company leads
with its CEO** — assert this and verify it before shipping.

Message guidance and the real FinThots proof points to use are in `references/messaging.md`.

**The angle comes from `finthots_sales_collateral`, not from here.** That skill holds the
thirteen pitch angles with their proof status, the angle-by-buyer mapping and the CTA ladder.
Use it to pick the angle per persona before writing the MESSAGES tab, and never write a claim
it marks NOT YET — above all a before/after conversion percentage, which no FinThots
engagement has ever measured. Lead L&D with *we measure what changed*, credit with *we
already know your problem*, compliance with *obligation, not preference*.

## Hard rules

1. Exhaust the free regulator and filing sources before spending a credit.
2. Never spend a credit without showing the balance and asking first.
3. Report actual credits consumed after every batch, with the running balance.
4. Never present a company list from memory as the segment universe. Record a source URL.
5. Never fabricate an email, phone or LinkedIn URL. Blank beats invented.
6. Keep pattern-derived emails out of the Email column and verify before sending.
7. Check the revealed current employer against the target company; flag every mismatch.
8. Check mobiles against the TRAI DND registry before handing over a calling list.
9. Tag existing clients, open their messages as a follow-up, and confirm the list with the
   user before anything goes out cold to a live account.
10. Flag and exclude companies in insolvency, resolution or under administrator control.
11. A tracker is Excel, not a document. Deliverables people work in are spreadsheets.
