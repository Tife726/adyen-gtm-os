# Adyen outbound GTM system (v0.1, 2026-11-13)
- Design built from `context/icp/` (icp.md, personas.md, signal-matrix.md), the research in `context/docs/research/`, and the tools and skills in this repo.
- INFERRED = a design choice not backed by research or outbound data. Status: work in progress.
- Validate it with sales, then run the first workflow (W1) before scaling.

**Flow:** Context OS → signals → find companies → enrich → ICP fit check → route by segment → find people → find and verify emails → sequence → replies → CRM → weekly feedback → back to the Context OS

**Two operating rules for the whole system:**
- Start from the signal, then filter by ICP (signal-matrix-framework.md).
- Prove every paid column on one real row before running it at scale (`import-clay-table` skill).

---

## Step 0. Context OS (source of truth)
| | |
|---|---|
| What happens | Every workflow reads its targeting, angles and facts from the repo, never from memory |
| Tool | This repo (GitHub). Notion is for publishing only (CLAUDE.md) |
| Inputs it holds | `icp.md` (filters), `personas.md` (titles, pains), `signal-matrix.md` (signals, angles), `research/` (citable facts), `knowledge-base/learnings/` (results) |

| Decision | Answer → what happens |
|---|---|
| Is the fact or claim in `context/`? | Yes → it may be used in copy. No → it is not used. Add it to research with a source first (rules: cite every fact, never invent numbers) |
| Has the ICP or matrix changed this week? | Yes → re-run the affected workflows' ICP filter and routing (Step 4/5). No → no change |

## Step 1. Buying signals (always on)
| | |
|---|---|
| What happens | Scheduled jobs detect the signal events from Matrix 2 and write one row per event (company, signal, date, evidence URL) |
| Tool | treg calls, then a Clay Companies table. Build first: S1 hiring (`harvestapi.linkedin.search.jobs`), S2 SaaS payments launch, S5 new country, S3 newly adopted commerce platform (`predictleads ... first_seen_at_from`) |
| Cadence | Job signals (S1, S2, S5) weekly; technology (S3, S4) monthly; leadership (S6) monthly (workflow-anatomy.md periodicity) |

| Decision | Answer → what happens |
|---|---|
| Is the signal VERIFIED or UNTESTED in Matrix 2? | VERIFIED → run on the schedule. UNTESTED (S3, S5) → pilot on 50 companies first and check precision by hand. Precision too low → KILL it and record why in learnings |
| Event older than its window? | The window is not defined by research (open question in icp.md). INFERRED default: jobs posted within 30 days. Older → drop |
| Was the company already contacted in the last 90 days? (INFERRED suppression) | Yes → log the new signal on the record and do not send. Exception: a new, different signal can re-enter after the suppression ends. No → continue |

## Step 2. Find companies
| | |
|---|---|
| What happens | Turn signal rows into unique companies (domain is the key) and de-duplicate across signals |
| Tool | Clay Companies table (`build-clay` skill), imported with `import-clay-table` |

| Decision | Answer → what happens |
|---|---|
| Can the domain be resolved? | Yes → continue. No → drop (no domain means no enrichment) |
| Same company from more than one signal? | Yes → merge into one row and keep every signal (stacking for routing). No → single-signal row |
| Is the company an existing Adyen customer? | Yes (PredictLeads detects Adyen, or the CRM shows it) → exclude, or route to account management. No → continue |

## Step 3. Enrichment
| | |
|---|---|
| What happens | Add firmographics and stack data for the fit check and for routing |
| Tool | treg `companyenrich.companies.enrich` ($0.0098: employees, revenue, industry, HQ, tech). treg `predictleads.companies.technology_detections` ($0.04: PSPs in job posts, fraud tools, new platforms) only for rows that pass Step 4 basics |
| Order (cost control) | Cheap enrichment on everything, then the fit check, then expensive enrichment only on rows that pass |

| Decision | Answer → what happens |
|---|---|
| Enrichment returns a different company? (seen in 3 of 54 in [firmo]) | Yes → flag BAD and drop, or fix the domain by hand. No → continue |
| Headcount looks undercounted for a known brand? (Prada, Lavazza in [firmo]) | Yes → keep the row and mark SUSPECT. The fit check treats it as "Stretch", not a reject |

## Step 4. ICP fit check
| | |
|---|---|
| What happens | Score each company against icp.md Layer 1 bands and the "Not a fit" list |
| Tool | Clay formula columns (free), plus one Clay AI column only for segment classification (`prompt-builder` skill) |

| Decision | Answer → what happens |
|---|---|
| In the "Not a fit" list (below ~$10M volume, small merchant wanting self-serve, a platform's sub-merchant)? | Yes → reject and store the reason. No → continue |
| Merchant: 201-10K employees and $50M+ revenue? | Yes → Core. 51-200 employees and $50M+ revenue → Stretch (lower priority in the sequence queue). Otherwise → reject |
| Platform or marketplace: takes payments for, or pays out to, its own merchants? | Yes → B1 or B2 at any size from 10 employees. No → merchant path |
| Public sector? | Yes → hand to the account team. Not outbound (Matrix 1 priority Low) |
| HQ in the EU or UK? | Yes → apply the GDPR handling in Step 7 (framework: EU enrichment constraints). No → standard |

## Step 5. Route by segment
| | |
|---|---|
| What happens | Assign one segment, the angle and the proof story from Matrix 1 |
| Tool | Clay formula, plus the AI classifier from Step 4 |

| Segment → | Angle | Proof story to cite | Lead product |
|---|---|---|---|
| A1 Multi-country retail and consumer brands | Consolidating PSPs and local acquirers | Moose Knuckles (4 to 1), Louboutin, AS Colour | Unified Payments |
| A2 Hospitality, F&B and venues | Speed and terminals across new markets | FC Bayern (3s faster, ~10% more transactions), HEYTEA | In-person payments |
| A3 Subscriptions, travel and gaming | Authorization rate and pricing transparency | DAZN +5.19%, Travian, Rebtel | Online payments with Uplift |
| B1 Vertical SaaS | Payments as revenue | Teamfront, ROLLER, Tessitura | Adyen for Platforms |
| B2 Marketplaces | One funds flow for pay-ins and payouts | Vinted, Expedia | Intelligent Money Movement |

| Decision | Answer → what happens |
|---|---|
| Is a stack signal present (S4: 2 or more PSPs, or a competitor PSP in job posts)? | Yes → the consolidation variant of the angle. No → the base angle for the segment |
| Signal type is S5 (new country)? | Yes → the expansion variant ("as you open in {country}"). Overrides the base angle |
| Classifier confidence low? | Yes → manual review queue. No → auto-route |

## Step 6. Find people
| | |
|---|---|
| What happens | Find 1 to 3 contacts per company: the champion first, then the economic buyer and the influencer |
| Tool | Clay Find People (free search source), with the Claygent path as fallback (`build-clay` people paths). Titles come from personas.md |
| Titles | Champion: VP/Head/Director of Payments, Payments Lead. Economic buyer: CFO, Treasurer, VP Finance. Influencer: CTO, CIO, VP Engineering or eCommerce Technology |

| Decision | Answer → what happens |
|---|---|
| Signal S1 named the hiring team? | Yes → target the hiring manager's function (e.g. Fraud Manager role → Head of Payments or Risk). No → default persona order |
| Champion found? | Yes → champion first, CFO second, and the CTO only for A1/A2 replatform (S3) rows. No → CFO first (B2, A1) or CTO first (B1: the product lead or CPO owns payments, INFERRED) |
| No persona found at all? | Claygent fallback. Still none → park the company for LinkedIn company-page targeting, or drop it |

## Step 7. Find and verify emails
| | |
|---|---|
| What happens | Waterfall to find work emails, then validate before export |
| Tool | Clay waterfall (`tools/rex/templates/waterfall-work-email.json`: Prospeo first, then LeadMagic or Findymail). Validate with Bounceban, the recommended primary (knowledge-base/playbooks/clay/expert.md) |

| Decision | Answer → what happens |
|---|---|
| Validation result: valid? | Yes → email plus LinkedIn sequence |
| Catch-all? | Send only from the lowest-risk sender pool, or LinkedIn only (INFERRED policy). Watch bounces per Step 12 |
| Invalid or not found? | LinkedIn-only sequence |
| EU or UK contact? | Use business email only. Keep the source and the legitimate-interest basis on the record. Opt-out honoured in every message (INFERRED: legal review needed; research has no GDPR source) |

## Step 8. Sequence (email and LinkedIn)
| | |
|---|---|
| What happens | Multichannel sequence built from the approved spec. The copy references the signal first, then the segment proof |
| Tools | Copy: `build-workflow-spec` then `campaign-copy` (gated, facts only, no em dashes). Sender: EmailBison, Instantly or PlusVibe (the kit supports all three: **team decision**). LinkedIn: HeyReach (`linkedin-dm-frameworks.md`). QA: `campaign-qa`. Deliverability: `inbox-placement-test` (MailReach) before launch |
| Default shape (INFERRED) | Day 1 email (signal + proof). Day 2 LinkedIn connect. Day 4 email (second proof point). Day 7 LinkedIn DM. Day 10 email (CFO angle to the second contact). Day 14 breakup |

| Decision | Answer → what happens |
|---|---|
| Inbox placement test before launch: KILL, MARGINAL or KEEP? | KEEP → launch. MARGINAL → launch on half the volume and retest. KILL → swap inboxes or copy (fingerprint isolation) |
| Campaign QA against the spec passes? | Yes → launch. No → fix in Clay or the copy, then re-QA |
| Contact connects on LinkedIn? | Yes → DM step. No → email-only continues |

## Step 9. Reply handling
| | |
|---|---|
| What happens | Classify every reply, then act within 1 business day (INFERRED service level) |
| Tool | Sender's unified inbox, plus an AI reply classifier (`prompt-builder`). An SDR owns the decision |

| Reply class → | Action |
|---|---|
| Positive or meeting | Book the meeting. Create the deal in the CRM. Stop the sequence for the whole account |
| Objection: price or minimums | Send the interchange++ explainer (pricing in company.md). If volume is below the floor → polite disqualify |
| Objection: happy with current PSP | Ask whether they run more than one. Offer the consolidation proof. Snooze 90 days |
| Objection: migration effort | Proof: Teamfront "less than three months", AS Colour "record time" |
| Referral ("talk to X") | Add X as a new contact with the referrer named. Restart the sequence for X |
| Not now or timing | Snooze until the stated date. Re-enter if a new signal fires |
| Not interested or unsubscribe | Suppress the contact permanently. Suppress the company for 180 days (INFERRED) |
| Out of office | Pause and resume after the return date |
| Bounce | Mark the email invalid. Feed the bounce rate to Step 12 |

## Step 10. CRM
| | |
|---|---|
| What happens | Record company, contacts, signal, segment, angle, sequence and outcome. The CRM holds the suppression list |
| Tool | HubSpot or Salesforce (both supported by kit skills: **team decision**). Clay CRM sync column |
| Required fields | `signal_id` (S1 to S11), `signal_date`, `segment` (A1 to B2), `angle_variant`, `workflow_id` (W1 to W7), `persona`, `sequence_step_at_reply`, `reply_class`, `meeting_booked`, `opportunity_value` |

| Decision | Answer → what happens |
|---|---|
| Account already open in the CRM (owner or active opportunity)? | Yes → do not sequence. Alert the owner with the new signal. No → create the records |
| Meeting held and qualified? | Yes → opportunity, linked to the `workflow_id` for attribution. No → nurture with the reason |

## Step 11. Weekly feedback loop (back into the Context OS)
| | |
|---|---|
| What happens | Every week, compare results per signal, segment, angle and persona, and change the Context OS on that evidence |
| Tool | CRM report and sender metrics, then `knowledge-base/learnings/<week>.md`. Updates to `signal-matrix.md` (Matrix 3 tracker), `icp.md` and `personas.md`. Logged in `work-log/`. Clay cost check: `calculate-costs` / `workspace-cost-audit` |
| Metrics | Per workflow: rows found, ICP pass rate, email found rate, bounce rate, reply rate, positive reply rate, meetings, cost per meeting |

| Decision | Answer → what happens |
|---|---|
| Bounce rate above the threshold? (INFERRED: 2%) | Yes → pause the pool. Run inbox-placement-test. Tighten validation |
| Workflow reply rate below the bar after enough sends? (Thresholds INFERRED. Set them after W1's first 2 weeks) | Yes → change one thing: the angle first, then the persona, then the signal filter. Log the change. Still low after 2 iterations → KILL the signal in Matrix 2 with the reason |
| Signal produces positive replies? | Yes → raise its priority in Matrix 2. Clone it to the next segment with a keyword swap (composable) |
| New objection seen 3 or more times? | Yes → add it to personas.md objections, and add proof to research if it exists |
| ICP reject reasons cluster (e.g. many SUSPECT headcounts)? | Yes → fix the enrichment source or the bands in icp.md |
| Cost per meeting above budget? | Yes → move the expensive enrichment later in the flow, or cut columns |

---

## Check against the four pillars
| Pillar | Where the design meets it | Gaps (honest) |
|---|---|---|
| **Clean data** | Domain is the key with de-duplication (Step 2). BAD and SUSPECT flags from the enrichment test (Step 3). Waterfall plus Bounceban validation (Step 7). Required CRM fields (Step 10). Every fact cited (Step 0) | Enrichment errors are known (3 of 54 wrong company). PSP detection has about 50% recall. No GDPR legal review yet |
| **Always-on signals** | S1, S2, S5 run weekly and S3 and S4 monthly (Step 1), with suppression and re-entry on new signals | Only S1 is VERIFIED at volume. S3 and S5 need pilots. No working Worldpay or agentic detection |
| **Composable workflows** | 7 of 11 signals are universal: one Clay build with keyword swaps per segment. Shared enrichment, fit, routing and people steps. Angles are swapped by routing (Step 5), not rebuilt | Sender and CRM not chosen yet. Each choice locks one integration |
| **Tight feedback loops** | Weekly learnings file. Every outcome tied to `signal_id`, `segment` and `angle_variant`. Explicit kill and promote rules (Step 11) | `knowledge-base/learnings/` is empty, so every threshold is INFERRED until W1 has 2 weeks of data |

## Decisions the team still owns
1. Sender: EmailBison, Instantly or PlusVibe.
2. CRM: HubSpot or Salesforce.
3. Thresholds for bounce, reply and cost per meeting.
4. Suppression windows (90 and 180 days are placeholders).
5. GDPR and legal sign-off for EU and UK outbound.
6. Who works replies (the SDR owner).
