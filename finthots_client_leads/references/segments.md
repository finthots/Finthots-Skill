# Lending segments and how to discover each universe

FinThots sells to Indian BFSI lenders. Each segment has a different buying
pattern, a different wedge product and a different company universe.

**This file deliberately contains no company lists.** Any list written down
here would be stale within months and would cause misses — lenders merge, get
acquired, rebrand, enter resolution and launch constantly, and a memorised list
silently drops whoever joined the market after it was written. Discover the
universe at run time, every time, using the procedure below.

## Segment definitions

| Code | Segment | What it covers | FinThots wedge |
|---|---|---|---|
| HFC | Housing Finance | NHB-registered HFCs plus the housing arms of NBFCs and banks | Mortgage underwriting, property visit, technical/legal |
| AFF | Affordable Housing | HFCs lending to informal-income borrowers, typically sub-₹25 lakh tickets | Informal income assessment, field credit |
| BL | Business Loan / Unsecured | NBFCs and bank verticals doing unsecured SME and merchant lending | Cash-flow underwriting, bureau reading, early warning signals |
| LAP | Loan Against Property | Secured-against-property lending; usually a vertical inside HFCs and NBFCs | Valuation, title, LTV discipline, exit routes |
| SME | SME / MSME Credit | Bank and NBFC SME verticals, secured and unsecured | Financial statement analysis, credit assessment |
| VF | Vehicle Finance | CV, CE, car, used car, two-wheeler and tractor financiers | Sourcing, asset quality, used-asset valuation, collections |
| MFI | Microfinance | NBFC-MFIs and the microfinance arms of banks and SFBs | Field discipline, group lending, customer protection, collections conduct |
| GOLD | Gold Loan | Gold-loan NBFCs and bank gold verticals | Appraisal, LTV compliance, branch ops, auction process |
| BANK | Banks | PSU, private, small finance, payments and co-operative banks | Full credit cycle, mandatory compliance |
| AGRI | Agri & Rural | Agri verticals of banks and rural arms of NBFCs | Agri appraisal, KCC, FPO lending, rural relationship skills |

Segments overlap. LAP and SME sit inside HFCs and NBFCs; affordable housing is
a subset of HFC that behaves differently. Ask which vertical the user means
when a company could sit in two segments, because it changes which persona
buys.

## Discovering the universe — run this every time

1. **Start with what the user already has.** Ask for, or read, their existing
   sheet. It is the only source that knows which companies they have already
   worked, pitched or lost. Dedupe against it before adding a single new name.

2. **Pull the regulator or association list for the segment.** This is the
   authoritative universe and it is published and current:
   - HFC / AFF — National Housing Bank's list of registered HFCs
   - BL / LAP / SME / VF / GOLD — RBI's list of registered NBFCs, filtered by
     category (NBFC-ICC, NBFC-MFI, HFC) and by business line
   - MFI — MFIN and Sa-Dhan member lists, plus RBI's NBFC-MFI register
   - BANK — RBI's lists of scheduled commercial banks, small finance banks,
     payments banks and scheduled co-operative banks
   - AGRI — NABARD-linked lenders plus the agri verticals of the bank list

   Fetch these live with a web search or fetch tool. Do not reproduce them
   from memory.

3. **Fill the gaps with a company search tool.** `crustdata_company_search_db_v2`
   or `prospecting_company_search`, filtered to India and the segment's
   industry, catches lenders the regulator list names differently and surfaces
   recent entrants.

4. **Add AUM, headcount and HQ.** Rating-agency sector reports (CRISIL, ICRA,
   CARE), annual reports and company sites. These figures drive the tiering,
   so a missing AUM means a company cannot be tiered and should be flagged
   rather than guessed.

5. **Verify each company still operates as described.** Check for mergers,
   acquisitions, rebrands, insolvency, resolution and administrator control
   before adding it. This sector consolidates constantly, and a list built
   from stale knowledge will contain companies that no longer exist under that
   name, and will miss ones that launched recently.

6. **Record the source URL per company** so the list can be re-verified later
   without redoing the research.

## Rules

- Never present a company list drawn from memory as the segment universe. Say
  where each name came from.
- When the user names a segment not in the table, treat it the same way: ask
  what it covers, find the regulator or association that governs it, and build
  from there.
- Group structures matter. A housing finance subsidiary, its NBFC parent and
  the parent's other lending arms are separate target companies with separate
  L&D functions — but often a shared group L&D team. Note the relationship, and
  flag where pitching the group opens a larger door than the subsidiary.
