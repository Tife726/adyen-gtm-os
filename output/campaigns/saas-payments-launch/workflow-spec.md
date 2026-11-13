# Workflow spec: SaaS Payments Launch
**Status:** STRUCTURE-APPROVED 2026-11-13 · E1, E2 approved · E3, DM1, DM2 written, awaiting approval

**Source idea:** #5 "SaaS launching payments" in `output/campaigns/campaign-strategy.md` (Tier 1). Signal S2 in `context/icp/signal-matrix.md`.

**Built on:**
- `context/icp/icp.md`
- `context/icp/personas.md`
- `context/docs/gtm-system.md`
- `context/messaging/facts.md` (approved 2026-11-13)

**Reuse:** this workflow uses the same trigger pipe, Clay table, enrichment and sequence setup as `payment-talent-signal`. Only the T1 keywords, the company-type qualifier (C1) and the routers change.

**Volume:** not yet measured. "Payments Product Manager" returned about 2,900 raw LinkedIn posts in a month (2026-11-13), but the share at vertical software companies is unknown. Measure one week through T1, T2 and C1 before launch, and record the count in section 7.

---

## 1. Mother Intent (Trigger) [ENGINE]
**A vertical software platform in the ICP posted a job in the last 30 days to build or run payments inside its own product.**
Catalog: ⭐ Hiring specific roles, combined with New product launches.

## 2. Inclusion / Exclusion [ICP]
**Include**
- **ICP box (icp.md Segment B1):**
  - **What they are:** software companies that sell to merchants in one vertical, and whose merchants take payments. Examples: restaurants and POS, attractions and ticketing, fitness, field services, arts and culture, salons and retail POS.
  - **Size:** 10+ employees, no revenue floor.
  - **HQ:** Europe or North America.
- **Trigger:** the job title matches T1, does not match T2, and was posted within 30 days, at a company C1 marks `vertical_saas`.
- **Recipient (a waterfall, not a router):**
  1. A Head, VP or Director of Payments, or a Head of Fintech, if one exists.
  2. Otherwise the CPO or VP Product.
  3. If C3 = under 50 employees, the CEO or co-founder instead (INFERRED).

**Exclude**

| Who | Why |
|---|---|
| Payment companies, PSPs, acquirers and banks (C1 `payments_company`, plus the competitor domain list: Stripe, Checkout.com, Worldpay, Global Payments, Fiserv, Nexi, dLocal, Rapyd, PayPal, Nuvei) | Payments is their product |
| Existing Adyen platform customers: Toast, Lightspeed, ROLLER, Tessitura, Teamfront, Sport Alliance, Zenchef, SECUTIX, SKIDATA, Epos Now, yetipay, Next Commerce, Relay, Atlas, plus HubSpot customers and PredictLeads Adyen detections | Account management |
| Marketplaces (C1 `marketplace`) | Segment B2, a separate workflow |
| Merchants (C1 `merchant`) | Covered by Payment Talent Signal |
| Horizontal software (C1 `horizontal_saas`) | Not B1 (INFERRED: their merchants don't take card payments through the product) |
| Open HubSpot deal or owner | n8n alerts the owner |
| Contacted in the last 90 days (INFERRED) | Suppression |
| Staffing agencies; title noise (T2) | Not real buyers |

## 3. Sources [ENGINE]
| Data | Source | Tool / cost |
|---|---|---|
| Trigger (job posts) | LinkedIn Jobs, past 7 days, one query each: "payments product manager", "product manager payments", "embedded payments", "head of payments", "payments engineer", "fintech product manager", "payments partnerships" | treg `harvestapi.linkedin.search.jobs` ($0.001/call), run weekly by n8n and sent to a Clay webhook |
| Firmographics | Company enrichment by domain | treg `companyenrich.companies.enrich` ($0.0098) |
| Company type and vertical (C1, C2) | Clay AI column reading the enrichment description and the homepage. Prompt built with `prompt-builder` | Clay, 1 action per row |
| PSPs in job posts, Adyen detection | Technology detections | treg `predictleads.companies.technology_detections` ($0.04). Runs only when C1 = `vertical_saas` |
| Job description | LinkedIn job detail | Endpoint to confirm. C4 is parked |
| CRM status, people, email | HubSpot lookup; Clay Find People (Claygent fallback); Clay waterfall plus validation | Clay |

## 4. Signals table [ENGINE]
| # | Signal | Role | How it's read | Output |
|---|---|---|---|---|
| T1 | Payments build role posted | **trigger** | Title regex, case-insensitive: `\b(payments?\|fintech\|embedded (payments\|finance))\b` AND `\b(product manager\|product owner\|head\|director\|vp\|vice president\|lead\|engineer\|engineering manager\|architect\|partnerships?)\b`. Also `posted_date` ≥ today minus 30 | boolean |
| T2 | Title noise | **trigger (exclusion)** | `accounts payable\|\bAP\b\|payroll\|billing clerk\|collections\|payment (clerk\|processor)` | boolean. TRUE drops the row |
| C1 | Company type | **trigger (qualifier)** | Clay AI column, hard enum: `vertical_saas` / `horizontal_saas` / `payments_company` / `marketplace` / `merchant` / `other` | Only `vertical_saas` continues |
| R1 | Stage of the payments work | **router** | Title regex `\b(engineer\|engineering manager\|architect\|developer)\b` gives `build`, otherwise `decide` | enum `decide` / `build` |
| R2 | Existing payments partner | **router** | Count of distinct PSPs in PredictLeads job-post detections from the last 12 months, excluding Adyen. List: Stripe, Braintree, PayPal, Checkout.com, Worldpay, Global Payments, Fiserv, Nuvei, Elavon, Cybersource | **Clusters: 1+ = `partnered`; 0 = `unconfirmed`** (recall is about 50%, so 0 never means "no payments") |
| C2 | Vertical | **context-clause** (selects the proof story, never routes) | The same AI column, second enum: `restaurants_hospitality` / `attractions_ticketing` / `fitness_wellness` / `field_services` / `arts_culture` / `retail_pos` / `other_vertical` | enum. Proof: `field_services` → Teamfront (F3); `attractions_ticketing` → ROLLER (F7); all others → no proof line |
| C3 | Company size band | **context-clause** (recipient waterfall only) | Enrichment employees | `under_50` / `50_plus` |
| C4 | Job description focus | **context-clause** | Keyword groups: onboarding / in-person / monetisation | **Parked** |

## 5. Routing matrix with angles [ENGINE]
R1 (2) × R2 (2) = **4 cells.**

| Cell | Deterministic read | What it tells us (inferred situation) | The angle it earns |
|---|---|---|---|
| **1. Decide × partnered** | R1 = `decide` AND R2 = 1+ PSPs | Their merchants already pay through a provider (`{{psp_list}}`), and they are hiring someone to own payments as a product. They are reviewing how payments sits inside their product: who owns the merchant relationship, the onboarding and the revenue. | **From a provider in the stack to payments as your product.** Adyen for Platforms lets them offer payments under their own brand, so the new hire's first decision is about owning the payments relationship rather than adding another provider. Proof where the vertical matches: Teamfront (F3). |
| **2. Decide × unconfirmed** | R1 = `decide` AND R2 = 0 PSPs | They are hiring a product or leadership role for payments, and no provider was detected. Payments looks like a new product line being scoped, so they are at the build-or-partner choice. | **A new revenue line without becoming a payments company.** The person they're hiring will choose between building payments and offering it through a platform. Put the platform route on the table before that choice is made. Proof where the vertical matches: Teamfront (F3) or ROLLER (F7). |
| **3. Build × partnered** | R1 = `build` AND R2 = 1+ PSPs | They are hiring engineers to work on payments while `{{psp_list}}` is in the stack. They are extending or rebuilding the payments integration. | **Decide the foundation before the integration work hardens.** Once engineers build onboarding and payouts around one provider, changing is expensive. Show what onboarding inside their product looks like on a platform built for software companies, while the work is still early. Proof where the vertical matches: ROLLER (F7). |
| **4. Build × unconfirmed** | R1 = `build` AND R2 = 0 PSPs | They are hiring engineers for payments, and no provider was detected. They are building payments into the product, most likely for the first time. | **Time to the first merchant live.** For a first launch, what matters is how fast a merchant can be onboarded and take a payment inside their product. Lead with onboarding speed and payments under their brand. Proof where the vertical matches: ROLLER (F7). |

**Branch coverage**

| Variable | Cell 1 | Cell 2 | Cell 3 | Cell 4 |
|---|---|---|---|---|
| `{{job_title}}`, `{{company_name}}` | ✓ | ✓ | ✓ | ✓ |
| `{{psp_list}}` | ✓ | ✗ | ✓ | ✗ |
| `{{vertical_phrase}}` | ✓ | ✓ | ✓ | ✓ |
| `{{proof_customer}}`, `{{proof_result}}` | only when C2 matches | only when C2 matches | only when C2 matches | only when C2 matches |
| `{{jd_focus}}` | parked | parked | parked | parked |

## 6. Variables contract [ENGINE]
| Variable | Source field (Clay) | Exists in branches |
|---|---|---|
| `{{first_name}}` | People: `first_name` | All |
| `{{company_name}}` | Enrichment: `name`, cleaned of Inc., Ltd., GmbH, B.V. | All |
| `{{job_title}}` | Trigger `job_title`, **cleaned**: strip gender tags ("(m/f/d)", "(f/m/x)"), locations, "- Remote …" suffixes and requisition codes, then lowercase except proper nouns | All |
| `{{psp_list}}` | R2: `psp_names` joined with "and", max 3, Adyen excluded | Cells 1 and 3 only |
| `{{vertical_phrase}}` | C2 mapped to a plain phrase: `restaurants_hospitality` → "restaurant software", `attractions_ticketing` → "ticketing and attractions software", `fitness_wellness` → "fitness software", `field_services` → "field service software", `arts_culture` → "arts and culture software", `retail_pos` → "retail POS software", `other_vertical` → (omitted) | All except `other_vertical` |
| `{{proof_customer}}` | facts.md by C2: `field_services` → Teamfront; `attractions_ticketing` → ROLLER | Only when C2 matches |
| `{{proof_result}}` | facts.md: F3 (Teamfront) or F7 (ROLLER), approved phrasing | Only when C2 matches |
| `{{sender_name}}` | PlusVibe sender (values pending in facts.md) | All |
| `{{jd_focus}}` | C4 | Parked |

Copy may use no other variable.

## 7. Cadence & channel [ENGINE]
- **Periodicity:** always-on. n8n pulls on Mondays (past 7 days). De-duplicate on domain plus job ID, with a 90-day suppression window (INFERRED).
  - A company already in an active Payment Talent Signal sequence is skipped. One sequence per account at a time.
- **Channel:** both. Email goes through PlusVibe, and the LinkedIn steps are HubSpot tasks. Contacts with no valid email get LinkedIn only.
- **Shape (3 emails + 2 DMs, the same as Payment Talent Signal's approved sequence):**

| Day | Channel | Step |
|---|---|---|
| 1 | Email 1 | Cell angle |
| 2 | LinkedIn | Connect, no note |
| 4 | Email 2 | New value |
| 6 | LinkedIn | DM 1 |
| 10 | Email 3 | Close and referral |
| 13 | LinkedIn | DM 2 |

- **Volume check:** one week through T1, T2 and C1 before launch. Measured count: `{{pending}}`.

---

## Definition of done
- [x] Mother intent: one sentence, signal-led, matched to the catalog
- [x] Inclusion: ICP box, trigger and recipient [ICP]
- [x] Exclusion with reasons
- [x] Sources: the trigger and every enrichment
- [x] Signals table: deterministic reads, roles declared
- [x] Count clusters declared (R2: 1+ / 0)
- [x] Routing matrix with angles
- [x] Branch coverage checked
- [x] Variables contract complete
- [x] Engine/ICP markers on every section
- [x] Cadence and channel declared

**Next:** `/campaign-copy` on this file.

---

## Copy

**Subject** (approved 2026-11-13, same across the thread as `Re:`): `{the payments role|payments at {{company_name}}|your payments hire}`

Note: the "payments at {{company_name}}" variant contains a variable, against the facts.md voice note. The user approved it as-is.

### Email 1 (day 1, approved 2026-11-13)

Shape: `{{first_name}},`, then L1 hiring, L2 homework and situation, L3 value prop, L4 proof (only when the vertical matches), L5 question, then the sign-off.

**L1 (every cell):** "{{company_name}} is hiring a {{job_title}}."

**L2 `{{e1l2}}`:**
- + Cell 1 (decide × partnered): "From what I could find, payments at {{company_name}} already run through {{psp_list}}, so I'd guess this hire is about owning payments as part of the product."
- + Cell 2 (decide × unconfirmed), vertical known: "My guess is payments is a new line for your {{vertical_phrase}}, and the person you hire will decide whether to build it or offer it through a partner."
- + Cell 2, FALLBACK (`other_vertical`): "My guess is payments is a new line in the product, and the person you hire will decide whether to build it or offer it through a partner."
- + Cell 3 (build × partnered): "From what I could find, the product already takes payments through {{psp_list}}, so I'd guess this role is about extending or rebuilding that integration."
- + Cell 4 (build × unconfirmed): "No payments provider showed up in my research, so my guess is this is the first payments build at {{company_name}}."

**L3 (static, facts.md variant C):** "Adyen for Platforms lets software companies offer payments under their own brand."

**L4 `{{social_proof}}`:**
- + C2 = `field_services` AND cell 1 or 2: With Adyen, Teamfront went from "five or six integrated payment partners" to "one relationship." (F3)
- + C2 = `attractions_ticketing` AND cell 2, 3 or 4: With Adyen, ROLLER went "from onboarding customers in months to weeks." (F7)
- No match: the line is omitted.

**L5 `{{e1l5}}`:**
- + Cell 1: "Would it be useful to see what payments under the {{company_name}} brand could look like?"
- + Cell 2: "Is partnering still on the table, or is the plan to build it in-house?"
- + Cell 3: "Is the provider decision settled for this build, or still open?"
- + Cell 4: "How soon do you want the first merchants taking payments in the product?"

**Sign-off:** Best, / {{sender_name}}

**Sample (cell 2, field service software, as received):**
```
Subject: the payments role

{{first_name}},

{{company_name}} is hiring a payments product manager.

My guess is payments is a new line for your field service software, and the person you hire will decide whether to build it or offer it through a partner.

Adyen for Platforms lets software companies offer payments under their own brand.

With Adyen, Teamfront went from "five or six integrated payment partners" to "one relationship."

Is partnering still on the table, or is the plan to build it in-house?

Best,
{{sender_name}}
```

### Email 2 (day 4, same thread, approved 2026-11-13)

Cell 1 (decide × partnered):
```
{{first_name}},

One question tends to decide this early: do you want merchants signing up for payments inside the {{company_name}} product, or on a provider's page?

Where do you lean today?

Best,
{{sender_name}}
```

Cell 2 (decide × unconfirmed):
```
{{first_name}},

Building payments in-house usually means owning merchant onboarding, risk checks and payouts as well as the checkout.

Which of those is the team planning to own?

Best,
{{sender_name}}
```

Cell 3 (build × partnered):
```
{{first_name}},

The part of a payments integration that's hardest to change later is usually merchant onboarding, because it sits inside the product flow.

Is onboarding part of this build?

Best,
{{sender_name}}
```

Cell 4 (build × unconfirmed):
```
{{first_name}},

For a first launch, the first thing merchants see is the sign-up: how long until they can take a payment.

Is onboarding being built in-house, or handled by whichever provider you choose?

Best,
{{sender_name}}
```

### Email 3 (day 10, same thread, written 2026-11-13, awaiting approval)

Cell 1 (decide × partnered):
```
{{first_name}},

This is my last email on this.

If payments for {{company_name}}'s merchants sits with someone else, or with the new hire once they start, who should I speak with?

Best,
{{sender_name}}
```

Cell 2 (decide × unconfirmed):
```
{{first_name}},

I'll stop here.

If the build-or-partner call happens after the new hire starts, I'm happy to come back then. When would make sense?

Best,
{{sender_name}}
```

Cell 3 (build × partnered):
```
{{first_name}},

Last one from me on this.

If the engineering lead on this build is the better person to talk to, who would that be?

Best,
{{sender_name}}
```

Cell 4 (build × unconfirmed):
```
{{first_name}},

I'll leave it here.

If the launch is further out than I'd guess, is there a better time to compare notes?

Best,
{{sender_name}}
```

### LinkedIn (written 2026-11-13, awaiting approval)

Connection request (day 2): empty, no note.

DM 1 (day 6, after the connection is accepted, dynamic by cell):
- Cell 1: "Hi {{first_name}}, with {{psp_list}} already in the product, is the new payments role about bringing more of it in-house?"
- Cell 2: "Hi {{first_name}}, is payments in the product on this year's roadmap, or further out?"
- Cell 3: "Hi {{first_name}}, what's the first thing the new {{job_title}} will work on?"
- Cell 4: "Hi {{first_name}}, is the payments launch aimed at new merchants, or the ones already on the product?"

DM 2 (day 13, static, all cells):
"{Thanks for connecting|Appreciate the connect}, {{first_name}}. I work with software companies on payments at Adyen, {happy to compare notes|glad to swap notes} whenever payments comes up for {{company_name}}."
