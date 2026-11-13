# Workflow spec: Payment Talent Signal
**Status:** STRUCTURE-APPROVED 2026-11-13 
**Copy:** COPY-APPROVED 2026-11-13 (E1, E2, E3, DM1, DM2). Before launch: subject line, sender values, section 7 cadence update, `{{job_title}}` cleaning rule

**Source idea:** #3 "Payments team hiring" in `output/campaigns/campaign-strategy.md` (Tier 1, rank 1). Signal S1 / workflow W1 in `context/icp/signal-matrix.md`.

**Built on:**
- `context/icp/icp.md`
- `context/icp/personas.md`
- `context/docs/gtm-system.md`
- `knowledge-base/playbooks/workflow-anatomy.md`

**Facts:** `context/messaging/facts.md` does not exist yet, so every customer name, quote and metric is a `{{placeholder}}` until `/campaign-copy` creates and approves it.

**Scope:** payments leadership and payments operations roles only. Fraud (#4), payments product managers (#5) and treasury (#6) are sister campaigns. They use the same build with different T1 keywords.

---

## 1. Mother Intent (Trigger) [ENGINE]
**A company in the ICP posted a payments leadership or payments operations job on LinkedIn in the last 30 days.**
Catalog: ⭐ Hiring specific roles.

## 2. Inclusion / Exclusion [ICP]
**Include**
- **ICP box (icp.md Layer 1):**
  - **Segments:** A1 (multi-country retail and consumer brands), A3 (digital subscriptions, travel, gaming), B1 (vertical SaaS), B2 (marketplaces).
  - **Merchant size:** Core is 201-10K employees and $50M+ revenue. Stretch is 51-200 employees and $50M+ revenue, sent after Core.
  - **Platforms (B1, B2):** 10+ employees, no revenue floor.
  - **HQ:** Europe or North America.
- **Trigger:** the job title matches T1, does not match T2, and was posted within 30 days.
- **Recipient (personas.md), set by R1:**
  - **R1 = `leader`** (Head, VP, Director or Chief of Payments posted): primary contact is the **CFO**, with **VP Finance** as fallback. They are hiring the payments leader.
  - **R1 = `team`** (Manager, Lead, Senior, Specialist or Analyst posted): primary contact is the **Head, VP or Director of Payments**, who is the hiring manager. The CFO is the second contact (day 10).

**Exclude**

| Who | Why |
|---|---|
| PSPs, acquirers, banks and payment fintechs (enrichment industry Finance/Payments, plus a competitor domain list: Stripe, Checkout.com, Worldpay, Global Payments, Fiserv, Nexi, dLocal, Rapyd, PayPal, Nuvei) | Competitors, not buyers |
| Staffing and recruiting agencies | The real employer is unknown |
| Existing Adyen customers (HubSpot customer, or Adyen detected by PredictLeads) | Account management |
| Accounts with an open HubSpot deal or an owner | n8n alerts the owner instead |
| Contacted in the last 90 days (INFERRED window) | Suppression |
| icp.md "Not a fit" (below ~$10M volume, small merchant wanting self-serve, a platform's sub-merchant) | No fit |
| Public sector | Not outbound |
| Title noise (T2) | Not payments-platform roles |

## 3. Sources [ENGINE]
| Data | Source | Tool / cost |
|---|---|---|
| Trigger (job posts) | LinkedIn Jobs, past 7 days, sorted by date, one query per title keyword | treg `harvestapi.linkedin.search.jobs` ($0.001/call), run weekly by n8n and sent to a Clay webhook |
| Job description | LinkedIn job detail | **Endpoint to confirm** in the treg catalog. C2 stays off until then |
| Firmographics (employees, revenue, industry, HQ) | Company enrichment by domain | treg `companyenrich.companies.enrich` ($0.0098), written back into Clay with the source recorded |
| PSPs in job posts, Adyen detection | Technology detections | treg `predictleads.companies.technology_detections` ($0.04). Runs only on rows that pass the ICP filter |
| CRM status | HubSpot company lookup | Clay HubSpot integration |
| People | Clay Find People (Claygent fallback) | Clay |
| Email | Waterfall plus validation | Clay |

## 4. Signals table [ENGINE]
| # | Signal | Role | How it's read | Output |
|---|---|---|---|---|
| T1 | Payments role posted | **trigger** | Title regex, case-insensitive: `\b(head\|vp\|vice president\|director\|chief\|lead\|manager\|senior manager)\b.*\bpayments?\b` OR `\bpayments? (operations\|ops\|lead\|manager\|director)\b`. Also `posted_date` ≥ today minus 30 | boolean |
| T2 | Title noise | **trigger (exclusion)** | Title regex: `accounts payable\|\bAP\b\|payroll\|billing clerk\|collections\|payment (clerk\|processor)` | boolean. TRUE drops the row |
| R1 | Seniority of the posted role | **router** | Title regex `\b(head\|vp\|vice president\|director\|chief)\b` gives `leader`, otherwise `team` | enum `leader` / `team` |
| R2 | PSP footprint | **router** | Count of distinct PSPs in PredictLeads job-post detections from the last 12 months, excluding Adyen. List: Stripe, Braintree, PayPal, Checkout.com, Worldpay, Global Payments, Cybersource, Fiserv, Nuvei, Elavon | **Clusters: 2+ = `multi`** (several PSPs confirmed); **0-1 = `unconfirmed`** (one or none detected). Recall is about 50%, so 0-1 never means "one PSP" |
| C1 | Role based outside the HQ country | **context-clause** | Job location country ≠ HQ country | boolean, plus `role_country` |
| C2 | Focus named in the job description | **context-clause** | Description keyword groups, first match wins: acceptance / fraud / local_methods / stack | enum. **Parked** until the job-detail endpoint is confirmed |
| C3 | Segment | **context-clause** (selects the proof story, never routes) | Clay classifier column built with prompt-builder, hard enum output | `A1` / `A3` / `B1` / `B2` |
| C4 | Days since posted | **context-clause** | `today - posted_date` | `this week` / `this month` |

## 5. Routing matrix with angles [ENGINE]
R1 (2) × R2 (2) = **4 cells.**

| Cell | Deterministic read | What it tells us (inferred situation) | The angle it earns |
|---|---|---|---|
| **1. Leader × multi** | R1 = `leader` AND R2 = 2+ PSPs | They are creating or refilling the role that owns payments, while payments already runs across several providers. The new leader's first responsibility is a multi-provider setup they didn't choose. | **Shape the inheritance.** To the CFO: before the new payments leader starts, see what one platform across `{{psp_list}}` would look like. The hire then begins with a consolidation decision already framed. Proof: a consolidation story matched to the segment. |
| **2. Leader × unconfirmed** | R1 = `leader` AND R2 = 0-1 PSPs | They are turning payments into a function with a named owner. Until now someone in finance or engineering has run it alongside another job. | **A payments owner means a payments review.** To the CFO: a new payments leader usually starts by mapping markets, methods and providers. Offer that map up front. No provider is named. |
| **3. Team × multi** | R1 = `team` AND R2 = 2+ PSPs | An existing payments leader is adding headcount to operate several providers. The team grows with each provider they run. | **Grow capacity without growing the provider count.** To the Head of Payments: they are hiring to run `{{psp_list}}`. One relationship instead of several is the alternative to adding people for each provider. Proof: `{{proof_quote}}` (Teamfront, once in facts.md). |
| **4. Team × unconfirmed** | R1 = `team` AND R2 = 0-1 PSPs | The payments function is growing, and the stack isn't known from outside. | **Performance as the team scales.** To the Head of Payments: lead with a segment-matched acceptance or conversion result, and ask how many providers they run today. If C1 is TRUE, `{{role_country}}` adds an expansion flavour. |

**Branch coverage**

| Variable | Cell 1 | Cell 2 | Cell 3 | Cell 4 |
|---|---|---|---|---|
| `{{job_title}}`, `{{company_name}}`, `{{posted_window}}` | ✓ | ✓ | ✓ | ✓ |
| `{{psp_list}}` | ✓ | ✗ | ✓ | ✗ |
| `{{role_country}}` (only when C1 is TRUE) | optional | optional | optional | optional |
| `{{proof_customer}}`, `{{proof_result}}`, `{{proof_quote}}` | ✓ | ✓ | ✓ | ✓ |
| `{{jd_focus}}` | parked | parked | parked | parked |

## 6. Variables contract [ENGINE]
| Variable | Source field (Clay) | Exists in branches |
|---|---|---|
| `{{first_name}}` | People: `first_name` | All |
| `{{company_name}}` | Enrichment: `name` (cleaned of Inc., Ltd., N.V.) | All |
| `{{job_title}}` | Trigger: `job_title` (as posted) | All |
| `{{posted_window}}` | C4: `posted_cluster` (`this week` / `this month`) | All |
| `{{psp_list}}` | R2: `psp_names` joined with "and", max 3, Adyen excluded | Cells 1 and 3 only |
| `{{psp_count}}` | R2: `psp_count` (only 2+ is ever used) | Cells 1 and 3 only |
| `{{role_country}}` | C1: `job_country` | Any cell, only when C1 is TRUE |
| `{{segment}}` | C3: `segment_enum` | All (never printed; it selects proof) |
| `{{proof_customer}}` | facts.md lookup by `segment_enum` and cell | All. Placeholder until facts.md is approved |
| `{{proof_result}}` | facts.md lookup by `segment_enum` | All. Placeholder until facts.md is approved |
| `{{proof_quote}}` | facts.md lookup (consolidation quotes) | Cells 1 and 3. Placeholder until facts.md is approved |
| `{{jd_focus}}` | C2: `jd_focus_enum` | Parked |
| `{{sender_name}}` | PlusVibe sender | All |

Copy may use no other variable.

## 7. Cadence & channel [ENGINE]
- **Periodicity:** always-on. n8n pulls job posts every Monday (past 7 days) and the Clay pipeline runs on the new rows. De-duplicate on company domain plus job ID, with 90-day suppression and re-entry on a new posting after the window (INFERRED).
- **Channel:** both, following `gtm-system.md` Step 8.
  - **Email** goes through PlusVibe.
  - **LinkedIn** steps are HubSpot tasks.
  - Contacts with no valid email get LinkedIn only.
- **Shape (INFERRED):**

| Day | Channel | Step |
|---|---|---|
| 1 | Email | Cell angle |
| 2 | LinkedIn | Connect |
| 4 | Email | Second proof |
| 7 | LinkedIn | DM |
| 10 | Email | Second contact. Team cells: the CFO. Leader cells: VP Finance if found |
| 14 | Email | Breakup |

- **Volume check before launch:** run one week of posts through T1, T2 and the ICP filter, and record the counts here.

---

## Definition of done
- [x] Mother intent: one sentence, signal-led, matched to the catalog
- [x] Inclusion: ICP box, trigger definition and recipient, tagged [ICP]
- [x] Exclusion: who is out and why
- [x] Sources: the trigger and every enrichment
- [x] Signals table: every read deterministic, every role declared
- [x] Count clusters declared (R2: 2+ / 0-1)
- [x] Routing matrix with the angle column complete
- [x] Branch coverage checked
- [x] Variables contract complete
- [x] Engine/ICP markers on every section
- [x] Cadence and channel declared

**Next:** `/campaign-copy` on this file. Its gates will create `context/messaging/facts.md` first.

---

## Copy

### Email 1

**Cell 1 (Leader × multi), approved 2026-11-13, user-supplied text (render mode, kept verbatim)**

```
{{first_name}},

Saw that {{company_name}} is hiring a Head of Payments.

You're also currently processing payments through multiple providers, including {{psp_list}}.

If simplifying your payment infrastructure is part of the team's priorities, it might be worth a conversation.

Adyen helped {{proof_customer}} consolidate its payment providers and {{proof_result}}.

Would you be open to exploring what that could look like for {{company_name}}?

Best,
{{sender_name}}
```

Flags raised. On 2026-11-13 the user chose to keep the text exactly as written ("none"). Flags 1-6 are accepted. Flag 7 (cells 2-4) was resolved on 2026-11-13: cells 2-4 were written in the user's shape and approved (below).
1. "Saw that" matches the banned opener pattern in copy-bans.yaml.
2. "Head of Payments" is hard-coded. The leader cells also fire on VP, Director and Chief titles. `{{job_title}}` is the contract variable.
3. "You're also currently processing…" states the provider detection as certain. Detection recall is about 50%, and the list comes from job posts.
4. "If simplifying … it might be worth a conversation" is conditional selling (line law: value prop stated flat).
5. `{{proof_result}}` must be phrased to follow "consolidate its payment providers and". facts.md entries are pending approval.
6. There is no subject line yet.
7. Cells 2-4: this text needs `{{psp_list}}`, which only exists in cells 1 and 3. Cells 2 and 4 need their own Email 1.

**Cells 2-4, approved 2026-11-13 (written in the user's cell 1 shape)**

Cell 2 (Leader × unconfirmed, to the CFO):
```
{{first_name}},

{{company_name}} is hiring a Head of Payments.

My guess is payments is becoming its own function there, with one person owning the markets, methods and providers.

If getting that setup right for the new hire is part of the priorities, it might be worth a conversation.

Adyen helped {{proof_customer}} {{proof_result}}.

Would you be open to exploring what that could look like for {{company_name}}?

Best,
{{sender_name}}
```

Cell 3 (Team × multi, to the Head of Payments):
```
{{first_name}},

{{company_name}} is hiring a {{job_title}}.

You're also currently processing payments through multiple providers, including {{psp_list}}.

If simplifying your payment infrastructure is part of the team's priorities, it might be worth a conversation.

Adyen helped {{proof_customer}} consolidate its payment providers and {{proof_result}}.

Would you be open to exploring what that could look like for {{company_name}}?

Best,
{{sender_name}}
```

Cell 4 (Team × unconfirmed, to the Head of Payments):
```
{{first_name}},

{{company_name}} is hiring a {{job_title}}.

My guess is the team is measured on how many payments get approved in each market.

If raising approval rates is part of the team's priorities, it might be worth a conversation.

Adyen helped {{proof_customer}} {{proof_result}}.

Would you be open to exploring what that could look like for {{company_name}}?

Best,
{{sender_name}}
```

Email 1 notes:
- Subject: not set yet.
- The proof phrasing follows "helped" (cells 2 and 4) or "…and" (cells 1 and 3). The facts.md entries must be written to fit.
- `{{job_title}}` needs a cleaning rule (strip gender tags, locations and codes) before launch. This is a pending spec change.

### Email 2 (day 4, same thread, approved 2026-11-13)

Cell 1 (Leader × multi, to the CFO):
```
{{first_name}},

A new Head of Payments usually inherits whatever provider setup is in place on day one.

{{proof_quote}}

Is that worth a conversation before the hire starts?

Best,
{{sender_name}}
```

Cell 2 (Leader × unconfirmed, to the CFO):
```
{{first_name}},

One thing that tends to help a new payments lead is a single view of markets, payment methods and providers before their first day.

Happy to put a first version together for {{company_name}}.

Would that be useful?

Best,
{{sender_name}}
```

Cell 3 (Team × multi, to the Head of Payments):
```
{{first_name}},

Adding people is one way to run several providers. Running fewer of them is another.

{{proof_quote}}

Worth comparing notes on how {{company_name}} has it set up?

Best,
{{sender_name}}
```

Cell 4 (Team × unconfirmed, to the Head of Payments):
```
{{first_name}},

A simpler question than my last email.

How many providers are {{company_name}}'s payments running through today?

Best,
{{sender_name}}
```

Email 2 notes:
- `{{proof_quote}}` resolves to F2 (AS Colour CFO) for A1 and F3 (Teamfront) for B1, once facts.md is approved.
- Cell 2's offer of "a first version" needs sales to agree to deliver it.

### Email 3 (day 10, same thread, approved 2026-11-13)

Cell 1 (Leader × multi, to the CFO):
```
{{first_name}},

Last note from me.

If the provider question sits with someone else at {{company_name}}, or with the new Head of Payments once they start, who should I talk to?

Best,
{{sender_name}}
```

Cell 2 (Leader × unconfirmed, to the CFO):
```
{{first_name}},

Last note from me.

If this makes more sense once the new Head of Payments has started, happy to wait. Who should I speak with then?

Best,
{{sender_name}}
```

Cell 3 (Team × multi, to the Head of Payments):
```
{{first_name}},

Last note from me.

If running fewer providers isn't on the table this year, no problem. Is there a better time to compare notes, or someone else on the team I should speak with?

Best,
{{sender_name}}
```

Cell 4 (Team × unconfirmed, to the Head of Payments):
```
{{first_name}},

Last note from me.

If approval rates sit with someone else on the team, who would be the right person?

Best,
{{sender_name}}
```

### LinkedIn (approved 2026-11-13)

Connection request (day 2): empty, no note.

DM 1 (day 6, after the connection is accepted, dynamic by cell):
- Cell 1: "Hi {{first_name}}, with the Head of Payments role open, is the provider setup something you'd want settled before they start, or left for them?"
- Cell 2: "Hi {{first_name}}, with the Head of Payments role open at {{company_name}}, what's the first thing you'd want them to take on?"
- Cell 3: "Hi {{first_name}}, is the new {{job_title}} mostly about running the current provider setup, or something new?"
- Cell 4: "Hi {{first_name}}, what will the new {{job_title}} focus on first?"

DM 2 (day 13, static, all cells):
"{Thanks for connecting|Appreciate the connect}, {{first_name}}. I work on payments at Adyen, in case the provider question {ever comes up|comes up later} for {{company_name}}."

Sequence notes:
- The spec cadence in section 7 still shows 4 emails and needs updating to 3 emails plus 2 DMs (proposed: days 1, 2, 4, 6, 10, 13).
- No subject line is set yet.
