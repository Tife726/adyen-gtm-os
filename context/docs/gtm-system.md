# Adyen outbound GTM system (v0.2, 2026-11-13)
- Design built from `context/icp/` (icp.md, personas.md, signal-matrix.md) and the research in `context/docs/research/`.
- Tools: Claude Code, Clay, HubSpot, n8n, PlusVibe, GitHub.
- INFERRED = a design choice not backed by research or outbound data.
- Status: work in progress. Validate with sales, then run workflow W1 before scaling.

**Miro board:** [Adyen GTM System](https://miro.com/app/board/uXjVEOU-iKg=/)

**Flow:** Context OS → signals → find companies → enrich → ICP fit → route by segment → find people → find and verify emails → sequence → replies → HubSpot → weekly loop → back to the Context OS

**Rules for the whole system:**
- Start from the signal, then filter by ICP (signal-matrix-framework.md).
- Prove every paid Clay column on one real row before running it at scale (`import-clay-table`).

## Who does what
| Tool | Role |
|---|---|
| **GitHub** | Source of truth: ICP, personas, signal matrix, research, copy specs, learnings, work log |
| **Claude Code** | Builds and maintains everything: research, Clay table JSON (`design-clay-table-json`, `import-clay-table`), prompts (`prompt-builder`), copy (`build-workflow-spec`, `campaign-copy`), QA (`campaign-qa`), the weekly review. Runs paid lookups through the treg CLI when Clay can't |
| **n8n** | The clock and the pipes: schedules signal pulls, moves rows between Clay, PlusVibe and HubSpot, runs the reply-classification step, exports weekly metrics |
| **Clay** | The data work: companies, enrichment, ICP fit, routing, people, email waterfall and validation |
| **PlusVibe** | Sends the email steps, and holds sending metrics and the reply inbox |
| **HubSpot** | CRM: companies, contacts, deals, suppression, LinkedIn task steps, reporting |

---

## Step 0. Context OS
**What happens:** every workflow reads its filters, angles and facts from the repo. **Tools:** GitHub, Claude Code.

| Decision | Answer → what happens |
|---|---|
| Is the fact or claim in `context/` with a source? | Yes → it may be used. No → Claude Code researches it and adds it to `research/` first, or it is left out |
| Did the ICP or matrix change this week? | Yes → Claude Code updates the Clay filter and routing columns (Steps 4 and 5) and commits. No → no change |

## Step 1. Buying signals (always on)
**What happens:** scheduled pulls detect the Matrix 2 events and write one row per event: company, signal ID, date, evidence URL. **Tools:** n8n (scheduler, calling the providers) → Clay (webhook into the Signals table).

Start with the four "build first" signals:
- **S1** hiring for payments, fraud or treasury roles
- **S2** SaaS platform launching its own payments
- **S5** expansion into a new country
- **S3** newly adopted commerce platform

Cadence: job signals weekly, technology signals monthly.

| Decision | Answer → what happens |
|---|---|
| Is the signal VERIFIED in Matrix 2? | Yes → runs on schedule. UNTESTED (S3, S5) → pilot on 50 companies and check by hand. Too noisy → KILL it in the matrix with the reason |
| Is the event fresh? | Posted within 30 days (INFERRED window) → continue. Older → drop |
| Contacted in the last 90 days? (INFERRED) | Yes → log the signal in HubSpot and don't send. A new, different signal can re-enter after the window. No → continue |

## Step 2. Find companies
**What happens:** turn signal rows into unique companies keyed on domain, and merge companies that show several signals. **Tool:** Clay.

| Decision | Answer → what happens |
|---|---|
| Domain resolved? | Yes → continue. No → drop |
| Seen in more than one signal? | Yes → one row with all signals stacked (used in routing). No → single signal |
| In HubSpot as a customer or open deal? | Customer → exclude. Open deal → n8n alerts the owner with the new signal and does not sequence. Not in HubSpot → continue |

## Step 3. Enrichment
**What happens:**
- Cheap firmographics run on every company: employees, revenue, industry, HQ, tech.
- Stack data runs only after the fit check: PSPs named in job posts, fraud tools, newly adopted platforms.

**Tools:** Clay enrichment columns. Where Clay lacks a provider, Claude Code or n8n calls it through treg (companyenrich, PredictLeads) and writes the value back with its source recorded.

| Decision | Answer → what happens |
|---|---|
| Did enrichment match the wrong company? (3 of 54 in the test) | Yes → flag BAD and drop, or fix the domain. No → continue |
| Headcount clearly undercounted for a known brand? | Yes → mark SUSPECT and treat it as "Stretch" at the fit check |

## Step 4. ICP fit check
**What happens:** score each company against icp.md Layer 1 and the "Not a fit" list. **Tools:** Clay formulas (free) plus one Clay AI column for segment classification. Claude Code writes the prompt with `prompt-builder`.

| Decision | Answer → what happens |
|---|---|
| On the "Not a fit" list (below ~$10M volume, wants self-serve, a platform's sub-merchant)? | Yes → reject and store the reason. No → continue |
| Platform or marketplace that takes payments for, or pays out to, its own merchants? | Yes → B1 or B2, at 10+ employees. No → merchant path |
| Merchant size? | 201-10K employees and $50M+ revenue → Core. 51-200 employees and $50M+ → Stretch (sent after Core). Otherwise → reject |
| Public sector? | Yes → hand to the account team, no outbound |
| HQ in the EU or UK? | Yes → GDPR handling in Step 7. No → standard |

## Step 5. Route by segment
**What happens:** assign a segment, an angle and a proof story from Matrix 1. **Tool:** Clay formula.

| Segment | Angle | Proof | Lead product |
|---|---|---|---|
| A1 Retail and consumer brands | Consolidate PSPs and local acquirers | Moose Knuckles (4 to 1), Louboutin, AS Colour | Unified Payments |
| A2 Hospitality, F&B and venues | Speed and terminals in new markets | FC Bayern (3s faster), HEYTEA | In-person payments |
| A3 Subscriptions, travel and gaming | Authorization rate, transparent pricing | DAZN +5.19%, Travian, Rebtel | Online payments, Uplift |
| B1 Vertical SaaS | Payments as revenue | Teamfront, ROLLER, Tessitura | Adyen for Platforms |
| B2 Marketplaces | One funds flow for pay-ins and payouts | Vinted, Expedia | Intelligent Money Movement |

| Decision | Answer → what happens |
|---|---|
| Signal is S5 (new country)? | Yes → expansion variant ("as you open in {country}"). It overrides the base angle |
| 2 or more PSPs, or a competitor PSP, in job posts (S4)? | Yes → consolidation variant. No → the base angle for the segment |
| Classifier confidence low? | Yes → manual review view in Clay. No → auto-route |

## Step 6. Find people
**What happens:** find 1 to 3 contacts per company using the persona titles. **Tool:** Clay (Find People, with a Claygent fallback).

- **Champion:** VP, Head or Director of Payments.
- **Economic buyer:** CFO, Treasurer or VP Finance.
- **Influencer:** CTO, CIO, or VP Engineering or eCommerce Technology.

| Decision | Answer → what happens |
|---|---|
| Is a champion found? | Yes → champion first and CFO second. Add the CTO only for replatform (S3) rows. No → CFO first (A1, B2), or the product lead or CPO first for B1 (INFERRED) |
| No persona at all? | Try the Claygent fallback. Still none → drop the company |

## Step 7. Find and verify emails
**What happens:** a waterfall finds work emails, and validation runs before export. **Tool:** Clay (the waterfall template in `tools/rex/templates/`, then a validation column).

| Decision | Answer → what happens |
|---|---|
| Valid? | Yes → email plus LinkedIn |
| Catch-all? | LinkedIn only, or the lowest-risk PlusVibe inboxes (INFERRED) |
| Invalid or not found? | LinkedIn only (HubSpot tasks) |
| EU or UK contact? | Business email only. Record the source and the legitimate-interest basis, and include an opt-out. Needs legal sign-off (INFERRED, not in research) |

## Step 8. Sequence (email and LinkedIn)
**What happens:**
- Claude Code writes the copy from the approved spec. The gates are `build-workflow-spec`, then `campaign-copy`, then `campaign-qa`.
- The spec and copy live in GitHub at `output/campaigns/`.
- n8n pushes approved rows from Clay into PlusVibe for the email steps, and creates the LinkedIn steps as HubSpot tasks.

**Shape (INFERRED):**

| Day | Channel | Step |
|---|---|---|
| 1 | Email | Signal plus proof |
| 2 | LinkedIn | Connect |
| 4 | Email | Second proof |
| 7 | LinkedIn | DM |
| 10 | Email | CFO angle to the second contact |
| 14 | Email | Breakup |

| Decision | Answer → what happens |
|---|---|
| `campaign-qa` passes against the spec? | Yes → launch. No → fix in Clay or the copy, then re-run QA |
| PlusVibe sending health OK (bounce under the threshold on a test batch)? | Yes → full volume. No → pause the inboxes and fix them before scaling |
| Did they accept on LinkedIn? | Yes → DM task. No → email steps continue |

## Step 9. Reply handling
**What happens:** a PlusVibe reply webhook goes to n8n. n8n classifies the reply with an AI step (Claude Code writes the prompt) and updates HubSpot. An SDR acts within 1 business day (INFERRED).

| Reply | Action |
|---|---|
| Positive or meeting | Book it. Create the HubSpot deal. Stop the sequence for the whole account |
| Price or minimums | Send the interchange++ explainer (company.md). Below the volume floor → polite disqualify |
| Happy with current PSP | Ask whether they run more than one PSP. Send the consolidation proof. Snooze 90 days |
| Migration effort | Proof: Teamfront "less than three months", AS Colour "record time" |
| Referral | Add the named person in HubSpot with the referrer. Start their sequence |
| Not now | Snooze to the stated date. Re-enter if a new signal fires |
| Not interested or unsubscribe | Suppress the contact permanently, and the company for 180 days (INFERRED) |
| Out of office | Pause, then resume after the return date |
| Bounce | Mark the email invalid and count it in Step 11 |

## Step 10. HubSpot (CRM)
**What happens:** HubSpot is the record for companies, contacts, deals and suppression. Clay syncs records in, and n8n syncs PlusVibe events.

**Required properties:**
- `signal_id`
- `signal_date`
- `segment`
- `angle_variant`
- `workflow_id`
- `persona`
- `sequence_step_at_reply`
- `reply_class`
- `meeting_booked`
- `opportunity_value`

| Decision | Answer → what happens |
|---|---|
| Account has an owner or an open deal? | Yes → don't sequence. Notify the owner. No → create the records |
| Meeting held and qualified? | Yes → deal stage moves, tied to `workflow_id`. No → nurture, with the reason |

## Step 11. Weekly feedback loop
**What happens:**
1. Every Monday, n8n exports HubSpot and PlusVibe metrics per workflow: found, ICP pass rate, email found, bounce, reply, positive reply, meetings, cost per meeting.
2. Claude Code reviews them, writes `knowledge-base/learnings/<week>.md`, and updates `signal-matrix.md`, `icp.md` and `personas.md`.
3. Claude Code checks Clay spend with `calculate-costs`, logs everything in `work-log/` and commits to GitHub.

| Decision | Answer → what happens |
|---|---|
| Bounce above 2%? (INFERRED) | Pause the inboxes in PlusVibe and tighten validation in Clay |
| Reply rate below the bar? (Thresholds are set after W1's first 2 weeks) | Change one thing: the angle first, then the persona, then the signal filter. Still low after 2 tries → KILL the signal in Matrix 2 |
| Positive replies from a signal? | Raise its priority. Clone the Clay build to the next segment with a keyword swap |
| Same objection seen 3 or more times? | Add it to personas.md, and add proof to research if it exists |
| ICP rejects cluster on one reason? | Fix the enrichment source or the bands in icp.md |
| Cost per meeting over budget? | Move expensive columns later, or cut them |

---

## Four-pillar check
| Pillar | Where it's met | Gaps |
|---|---|---|
| **Clean data** | Domain key and de-duplication (Step 2), BAD and SUSPECT flags (Step 3), waterfall plus validation (Step 7), required HubSpot properties (Step 10), cited facts only (Step 0) | Known enrichment errors (3 of 54). PSP detection about 50% recall. GDPR not reviewed |
| **Always-on signals** | n8n runs S1, S2 and S5 weekly and S3 and S4 monthly, with suppression and re-entry on a new signal | Only S1 is verified at volume. S3 and S5 need pilots. No Worldpay detection |
| **Composable workflows** | 7 of 11 signals share one Clay build with keyword swaps. Enrichment, fit, routing, people and email are shared blocks. Angles are swapped by routing, not rebuilt | LinkedIn runs as manual HubSpot tasks, so it caps volume |
| **Tight feedback loops** | Every outcome carries `signal_id`, `segment` and `angle_variant`. Weekly n8n export, then Claude Code review, then a GitHub commit. Explicit kill and promote rules | No outbound data yet, so every threshold is INFERRED until W1 runs for 2 weeks |

## Decisions the team still owns
1. Thresholds for bounce, reply and cost per meeting.
2. Suppression windows (90 and 180 days are placeholders).
3. GDPR and legal sign-off for the EU and UK.
4. Who owns replies.
5. Whether LinkedIn stays manual in HubSpot.

## Miro board
- Board: [Adyen GTM System](https://miro.com/app/board/uXjVEOU-iKg=/) (created 2026-11-13).
- Spec: `diagrams/adyen-gtm-system.json`. Built with `.claude/skills/miro-gtm-flowchart/scripts/build_flowchart.py`.
- The board has step IDs (1A to 3E) that match this doc.
