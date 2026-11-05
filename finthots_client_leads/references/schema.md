# Output workbook schema

The deliverable is an **.xlsx**, not a document. A tracker is something people filter, sort
and type into.

Tabs, in this order: **READ_ME**, **TRACKER**, **MESSAGES**, **COMPANY_PLAN**,
**CREDIT_REPORT**, **PATTERN_QUEUE**, **VERIFY_QUEUE**.

## TRACKER — the sheet people actually work in

Touch | Company | Tier | AUM_Cr | Client | Name | Designation | Persona | Why_this_person |
Message_angle | Reach | Email | Email_status | Pattern_email_UNVERIFIED | Phone |
Phone_status | LinkedIn | Source | Status | Touch_date | Response | Next_action | Owner |
Intel_and_flags | Notes

- **Touch** is the sequence position within the company, 1 upward. Filtering Touch = 1 gives
  the whole first-contact campaign. Shade touch 1 so it is findable.
- **Persona**: L&D / Credit-Business / Compliance / HR-Gate / Executive / Nodal / Influencer.
- **Email_status**: `VERIFIED A+ (provider reveal)` / `verified (regulator directory)` /
  `from your sheet` / `pattern-derived - MUST VERIFY` / `available in <provider> - N credits
  to reveal` / `not available`. The last value is the one-click next step — make it explicit.
- **Pattern_email_UNVERIFIED** is a separate column, shaded red. It never merges into Email.
- **Reach**: `Email + call` / `Email` / `LinkedIn` / `CALL - alias inbox` / `Reveal needed`.
  Flag alias inboxes in red; a pitch there gets deleted.
- **Source** names the provider or document per row, so any row can be re-verified.
- **Intel_and_flags** carries tenure, prior employer, follower counts, and — critically —
  `HAS LEFT <company>` where a reveal showed the person has moved. Shade those rows red.
- **Status** gets a dropdown: Not started / Called / Emailed / LinkedIn sent / Responded /
  Meeting set / Demo done / Proposal sent / Negotiation / Won / Lost / Parked.
- **Client = Y** rows shaded yellow across the whole row.
- Turn on autofilter and freeze panes. Wrap text, top-align.

## MESSAGES

Touch | Company | Name | Designation | Persona | Subject | Full message

The full copy-paste text for every contact, keyed so it can be matched to the tracker row.
Not a template with merge fields — the written message.

## COMPANY_PLAN

Rank | Company | Tier | AUM_Cr | Client | Contacts | Touch_1 | Touch_2 | Touch_3 | Has_L&D |
Has_Credit | Has_Compliance | Verified_contact | Note

Shade the Has_* cells red where N — that is the research backlog at a glance.

## CREDIT_REPORT

Per provider: opening balance, used this run, closing balance, **renewal or expiry date**,
what the credits bought, and what the plan refused. Then: where enrichment stopped if a
balance ran out, and the queue to resume from.

List the free sources used alongside, with URLs, so the next run knows what cost nothing.

## PATTERN_QUEUE

One domain per line, for companies where fewer than two confirmed addresses exist so the
email pattern cannot yet be derived safely. The user runs Hunter.io Domain Search on each
(~50 free credits a month) and returns the pattern. Columns: Company | Domain |
Confirmed_addresses_held | Pattern_found.

## VERIFY_QUEUE

**Plain one address per line, no headers, no other columns.** This is the format every bulk
verifier accepts as a paste — GetProspect (~50 emails + 100 verifications free monthly),
ZeroBounce or BounceZero (~100 on signup). Everything in `Pattern_email_UNVERIFIED` goes
here.

The user runs the tool, exports the CSV and attaches it. Merge the results back: a verified
address is promoted into `Email` with `Email_status = verified (<tool>)`; anything rejected
or inconclusive stays out of `Email` and the row is marked. Never promote an unverified
address.

## READ_ME

How to work the tracker, the sequence and why it is in that order, where each source came
from, what the yellow and red shading mean, what is still missing, and the exact credit
position. Written for someone opening the file cold in three weeks.

## Rules

- `Existing_Client` is a **column**, never cell colour alone. Colour does not survive CSV
  export, filtering or a CRM import.
- Blank beats invented in every contact column.
- Record a source URL or provider name per row.
- Never ship a workbook whose formulas error. Recalculate and check.
