# Data sources, real costs, and what each plan actually allows

Everything here was verified in live runs. Limits and entitlements change — re-check with the
free tools rather than trusting these numbers.

## Free sources — always first

### NHB nodal officer directory (the single most valuable source)
The National Housing Bank publishes a consolidated directory of first Points of Contact and
Nodal Officers for **every registered HFC**, carrying name, designation, **direct email
address, phone number and office address**.

Search for: `NHB Annexure Details of first Point of Contacts PoCs Nodal Officers of HFCs`.
It is republished periodically as Annexure 1 / 2 / 3 — fetch the newest, never a remembered
URL. One live fetch returned usable records for 90 HFCs covering 49 of a 50-company target
list, including mobile numbers for roughly 80% of them.

Two caveats that matter:
- These are grievance and nodal roles, published so borrowers can escalate. They are **not**
  L&D heads. Treat them as senior named people who know which process is failing, and as the
  cheapest route to an internal referral.
- Roughly a quarter have only a **role alias** (`grievance@`, `nodalofficer@`, `cgro@`). A
  pitch into a complaints inbox gets deleted. Call those and ask for the person by name.
- The designation given is the nodal role, which may be a second hat. Confirm on the call.

### NHB asset-size list
`NHB list of HFCs as on <date>` — every registered HFC with its asset size. This is the only
honest basis for tiering. Without it, tiers are guesses.

### Stock-exchange filings (NSE / BSE)
Senior Management Personnel change filings name people with exact titles and effective dates:
Chief Risk Officer, Chief Compliance Officer, CISO, Company Secretary. Search
`<company> change in SMP` or `<company> senior management`. Free, authoritative, current.

### Company policy documents
KYC/AML policy PDFs name the **Principal Officer** — a statutory role. Grievance redressal
policies name the officer and often a direct line.

### HR trade press
hrkatha.com, peoplematters.in, thepeoplesboard.com report L&D and HR appointments. Useful for
large lenders only; small ones never make the news.

### Regulator registers by segment
NHB for HFCs; RBI for NBFCs, banks, SFBs and payments banks; MFIN and Sa-Dhan for
microfinance; NABARD-linked lists for agri.

## Free balance and entitlement checks — cost nothing, run them all

| Provider | Check tool | Returns |
|---|---|---|
| Apollo | `apollo_users_search` | Name, credit balance, any MCP bonus grant |
| Lusha | `account_usage` | Balance, rate limits, **price of every action** |
| Crustdata | `crustdata_credits_check_v2` | Balance, wallet type, **expiry date** |
| Crustdata | `crustdata_account_endpoints_v2` | **Which endpoints the plan allows** — check this |
| Crustdata | `crustdata_credit_costs` | Full rate card |
| RocketReach | `account` | Lookup credits **and monthly search quota** |
| Clay | `get-current-workspace` | Workspace and plan |
| Crustdata | `crustdata_company_identify_v2` | Company IDs — free, and required before people search |
| Lusha | `prospecting_contact_filters` | Valid filter values including seniority IDs |

## Verified plan realities (free tiers, Nov 2026)

**Lusha** — the best value for this work on a free plan.
- Contact search: **1 credit per 25 results.** One call accepts up to 100 company names and
  100 job titles, so an entire segment sweep costs 2–3 credits.
- Email reveal: 1 credit. **Phone reveal: 5 credits.**
- Always pass `reveal: ["emails"]`. The default reveals everything in `canReveal`, including
  phones at 5× the cost.
- Pass `waterfallEnabled: false` to avoid third-party vendor charges.
- Filters are **flat top-level fields** — `jobTitles`, `companyNames`, `countries`,
  `seniority`. There is no `include` or `filters` wrapper. `seniority` takes numeric IDs from
  the filters tool; title words like "head" are not seniority levels.
- `countries` takes ISO-2 codes (`IN`), not country names.
- Free plan: 40 credits, renews monthly.

**Crustdata** — cheap search, but check entitlements.
- People search: ~0.03 base + 0.1 experience filter + 0.1 experience response ≈ **0.25
  credits per result.**
- **Company-name filters do not match stored names.** Resolve to IDs with
  `company_identify_v2` (free) and filter on `experience.employment_details.current.company_id`.
  A failed filter returns zero results, charges nothing, and names the offending condition.
- Filter leaves use `{field, type, value}` — the key is `field`, not `column`.
- **Contact enrich may be disabled on the free plan**, in which case the credits cannot buy
  emails or phones at all, only names, titles and profile data. Verify with
  `account_endpoints_v2` before planning around them.
- Grants can be one-off with a hard expiry date. Surface that date prominently.

**Apollo** — large balance, heavily gated on free plans.
- Agent people search and agent bulk enrichment are **blocked on the free plan** and return a
  plan-limit message. Relay that message verbatim; do not retry or work around it.
- `apollo_email_validation_validate` **does** work: up to 10 addresses per call, ~1 credit
  each, async — the verdict is read by polling the webhook result tool. This is the right use
  of Apollo credits: validating pattern-derived addresses.
- It mandates an exact confirmation sentence before each batch. Use its wording.

**RocketReach** — the monthly search quota is the binding limit, not the credits. A free
account can show lookup credits remaining while `person_search` is already 50/50 used for the
month. Check both.

## The free stack — renewing monthly, and bigger than any single paid tier

**Search the plugin catalogue before assuming these are browser-only.** They are absent from
the *connector* registry, which is a different catalogue from *plugins*. Verified available as
plugins, each shipping its own MCP server:

- **Hunter** — partner-published by Hunter.io. Skills: `domain-search` (confirms a company's
  email pattern), `email-finder`, `email-verifier`, `person-enrichment`, `check-usage`. This
  is the single most useful install for this work.
- **ClearBounce Email Verification** — `verify_email`, `verify_bulk`, `check_bulk_status`,
  `get_bulk_results`, `find_email`, `check_credits`. Bulk verification as a direct call.
- **FullEnrich** — waterfall enrichment across 25+ providers, with CSV export.
- **VibeLeads**, **DataKart**, **GIA Prospecting Engine** — further prospecting options.

Installing a plugin turns the handoff below into a direct tool call. Check first; fall back to
the handoff only when nothing is installed. Snov, GetProspect, Prospeo and Skrapp remain
browser-only at the time of writing.

| Tool | Free per month | The job it is best at |
|---|---|---|
| **Hunter.io** | ~50 credits | **Confirming a company's email pattern.** One Domain Search returns the pattern plus sample addresses — this is what makes derivation safe |
| **GetProspect** | ~50 emails + 100 verifications | **Bulk verification** — the largest free verification allowance of the set |
| **ZeroBounce / BounceZero** | ~100 on signup | Final deliverability check before a send |
| Snov.io | ~50 credits | Second-pass finding on misses; bulk upload of name + domain |
| Prospeo | ~50–75 | Topping up finds |
| Skrapp | ~50–75 | Chrome extension, finds from a profile page |
| Reoon | varies | Alternative verifier when others are exhausted |

Stacked, roughly 150–250 finds plus 200 verifications a month, **renewing** — more than any
single paid tool's free tier, and it covers precisely the gap that paid credits are usually
wasted on.

### The handoff — when no plugin is installed

With Hunter or ClearBounce installed, call them directly and skip this. Without them, prepare
the input and consume the output, which is most of the work anyway. Build two extra tabs:

**PATTERN_QUEUE** — one domain per line, for companies where fewer than two confirmed
addresses exist, so the pattern cannot yet be derived safely. The user runs Hunter Domain
Search on each and returns the pattern.

**VERIFY_QUEUE** — plain one address per line, no headers, no other columns. This is the
format every bulk verifier accepts as a paste. Everything in the
`Pattern_email_UNVERIFIED` column goes here.

The user runs the free tool, exports the CSV, and attaches it. The skill then merges: a
verified address is promoted into the `Email` column with `Email_status = verified
(<tool>)`; anything rejected or inconclusive stays out and the row is marked.

### Order of operations

1. **Hunter Domain Search** on every company with fewer than two confirmed addresses — this
   gives the pattern, and the pattern is what makes derivation legitimate rather than guessing.
2. **Derive** addresses only for companies whose pattern is now confirmed.
3. **Bulk verify** the derived set through GetProspect or ZeroBounce.
4. **Promote only verified addresses** into the Email column. Never the unverified ones.
5. **Snov / Prospeo / Skrapp** for whoever is still unfound.

Doing step 1 before step 2 is the whole point. Verified live: pattern derivation without a
confirmed pattern produced wrong addresses at three companies — a numbered alias, a group
domain, and an entirely different registered domain. A Hunter domain search would have caught
all three for one credit each.

## India-specific notes

- Provider coverage for BFSI mid-management is patchy. Expect 40–70% email fill, lower on
  mobile — but check the NHB directory first, which often beats both.
- Check mobiles against the **TRAI DND registry** before bulk dialling and call from a number
  that identifies as a business.
- Group employees often carry the parent's domain rather than the lending entity's. This is
  not an error — flag it, because pitching the group can open a larger door.
- Email patterns vary within a single group. Confirm per entity, never per group.
