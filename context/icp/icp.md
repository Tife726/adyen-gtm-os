# Adyen ICP
Built only from `context/docs/research/` (2026-11-13). Source tags: [firmographics] = firmographics.md, [company], [customers], [competitors], [market], [signals], [voice], [stories] = brand-stories.md. INFERRED = my reading of the research, not a stated fact.

## One line
Businesses that sell across several countries or channels, or software platforms that want to offer their own payments, and that have outgrown a patchwork of payment providers. [stories §3, customers, voice: "One platform"]

## Layer 1: Firmographic
Bands come from enriching 51 named Adyen customers [firmographics]. The sample is brands Adyen chose to showcase, so these are the bands where Adyen wins publicly. They are not a hard floor.

### Segment A: enterprise merchants
| Filter | Core band | Stretch | Evidence |
|---|---|---|---|
| Employees | 201-10,000 | 51-200 if revenue is $50M+ | 25 of 32 customers have 201+ employees. Under-200 examples: Rebtel, Recharge.com, FAO Schwarz [firmographics] |
| Revenue | $50M+ | $10-50M with strong multi-country presence (Ace & Tate, Moose Knuckles) | 23 of 28 report $50M+ [firmographics] |
| Payment volume | $10M+ a year (third-party estimate) | n/a | [company], [competitors] |
| HQ | Europe first (21 of 33), then North America (9) | APAC (3: HEYTEA, Tinyme, MUJI) | [firmographics] |
| Industry label | Retail and consumer brands (22 of 33), hospitality and F&B (5) | Media and streaming, travel and transport, telecom, gaming | [firmographics], [stories] |
| Footprint | Sells in 2+ countries, online plus physical or app | n/a | BlaBlaCar, Lavazza, KIKO (1,000 stores) [stories, customers] |
| Commerce stack | Salesforce Commerce Cloud is common (10 of 33 show Salesforce) | Shopify (Birchbox, FAO Schwarz) | [firmographics] |

### Segment B: platforms and marketplaces
| Filter | Band | Evidence |
|---|---|---|
| Employees | Any size from about 10 upward. 7 of 18 have 200 or fewer employees | yetipay, Atlas, Teamfront (small); ROLLER, Tessitura, Epos Now (201-500); Toast, Etsy, Expedia (5K+) [firmographics] |
| What qualifies | Vertical SaaS or a marketplace that takes payments for, or pays out to, its own merchants or sellers. The fit depends on how much volume runs through its merchants, not on its own headcount (INFERRED from the size spread) | [stories §3] |
| Verticals seen | Restaurants and POS, attractions and ticketing, fitness, field services, arts, parking, travel, resale | [stories], [customers] |
| HQ | Mixed: US, UK, France, Germany, Switzerland, Austria, Australia, Canada, Singapore | [firmographics] |

### Segment C: public sector
| Filter | Band | Evidence |
|---|---|---|
| Example | GOV.UK Pay (~1,000 services) | [customers]. One data point only, so do not build a list on it yet |

Firmographic open questions:
- Real payment volume per account. Enrichment gives revenue, not volume processed.
- Whether bands differ for wins against Stripe compared with Worldpay.
- A sample beyond Adyen's showcase brands (for example a lookalike or technographic source that detects PSPs).

## Layer 2: Technographic
Each signal is scored on two things: whether it predicts a switch, and whether we can actually detect it. Detection was tested on 8 known Adyen customers [signals: technographic detection test].

| Signal | Why it matters | Can we detect it? | Use it as |
|---|---|---|---|
| 2 or more PSPs, gateways or acquirers | The most common situation before a switch: Teamfront ("five or six"), Tessitura ("four or five"), Moose Knuckles ("at least four") [stories] | **Partly.** PredictLeads job-posting detections show PSPs by name, and it found Adyen at only 4 of 8 known customers. Crocs shows 3 PSPs | A list filter where detected. Otherwise confirm it on the call. A missing result does not mean one PSP |
| A named PSP comes up in job postings (Adyen, Braintree, Stripe, etc.) | Shows the stack the team runs today, and how recently (last-seen date) | **Yes**, through PredictLeads at $0.04 per company | A filter. Exclude existing Adyen accounts, and route Braintree, Stripe or Worldpay accounts to competitive plays |
| Third-party fraud tool on the site (e.g. Forter) | Fraud is a stated switch reason (PXG -97%, On's "complex matrix of manual risk rules") [stories]. Adyen sells its own risk tools [company] | **Yes.** Forter detected at Aritzia, Crocs and KIKO, from the site itself | A message angle (consolidating fraud into the payments platform), not a qualifier. Adyen customers run these tools too |
| Runs Salesforce Commerce Cloud | 10 of 33 customer merchants show Salesforce [firmographics]. Crocs rolled out with Commerce Cloud [stories] | **Yes**, through enrichment | A lookalike filter for Segment A |
| Runs New Black, Teamwork Commerce or Guidewire | Partners named in customer stories and releases [stories, customers] | **Not tested** | Open question |
| Replatforming ecommerce, order management or point of sale | Opens the payments decision (Brooks Brothers, Crocs, KIKO) [stories] | Through job posts or press (see Layer 5) | A trigger, not a static filter |

Removed: "Uses Stripe, Checkout.com or Worldpay" as a stand-alone fit signal. No research shows a win pattern against Stripe, and the tested sources detected Stripe only as noise from seller pages at Etsy. Competitor use now sits inside the job-posting row above.

Not usable: front-end scanners (DataForSEO: 0 of 8 detected) and company enrichment (0 of 54) for PSPs.

## Layer 3: Intent signals
| Signal | Strength | Evidence |
|---|---|---|
| Launching in a new country, or opening a first store abroad | High | [signals §1], [stories §3] |
| SaaS platform launching "X Payments" or embedded payments | High | Relay, Teamfront, Sport Alliance (Magicline Payments) [stories] |
| Hiring a VP or Head of Payments, or payments, fraud or treasury roles | Medium. The method works but has not been tested at scale | [signals §2] |
| Merger of two businesses with different payment stacks | High | Just Eat Takeaway [stories] |
| Fraud or chargeback losses made public | Medium | PXG, Freelancer (0.5% of volume) [stories] |
| Disputes over pricing, or downtime, with the current provider | Medium | Rebtel [stories] |
| Public complaints about payments (Trustpilot) | Low, untested | Method is priced at $0.00135 a call [signals §4] |
| Launching checkout through AI agents | INFERRED, untested | Dispute rules unsettled [market] |

## Layer 4: Persona (detail in personas.md)
| Role | Who | Evidence |
|---|---|---|
| Champion | VP or Head of Payments | Vinted, Expedia, Teamfront, DAZN, On [customers, stories] |
| Economic buyer | CFO or Treasurer | AS Colour CFO, Louboutin Group Treasurer. Adyen pitches "the CFO's office" [customers, stories] |
| Influencer | CTO, CIO or engineering lead | Birchbox CTO, Murdoch's CIO, Tessitura Chief Engineer, Hillarys Head of ICT [stories] |

## Layer 5: Engagement readiness
Readiness means an event a rep can verify from public evidence. It does not mean an attitude. Each row is a moment when a past Adyen customer switched.

| Tier | Observable event | How to verify | Precedent in research |
|---|---|---|---|
| **A: act now** | Announced a launch in a new country, or a first store abroad | Press release or news | BlaBlaCar (6 countries in one year), HEYTEA, Lavazza (7-market rollout through 2027) [stories, customers] |
| **A** | Software platform announcing its own payments product | Product page or press release | Relay, Teamfront, Sport Alliance (Magicline Payments) [stories] |
| **A** | Merger of two companies with separate payment stacks | M&A news | Just Eat Takeaway: payments "among the most fragmented areas" [stories] |
| **A** | Replacing the ecommerce platform, order management system or point of sale | Vendor press release or job posts | Brooks Brothers (Adyen rolled out "behind" a new OMS), Crocs (Commerce Cloud), KIKO (New Black) [stories] |
| **B: open with a proof point** | Public fraud or chargeback problem | News, earnings, reviews | PXG (-97% fraud), Freelancer (0.5% of volume lost) [stories] |
| **B** | Hiring a VP or Head of Payments, or payments, fraud or treasury roles | LinkedIn jobs via treg ($0.0005/call) | Method tested once only [signals §2] |
| **B** | Customer on Worldpay during its absorption into Global Payments | Technographic check (open question: no tool tested) | INFERRED from [competitors], [market] |
| **Not now** | No event above. Or a recent PSP migration (open question: no data on contract cycles) | | |

Not used as triggers: PSD3/PSR (no fixed date; the clock starts ~21 months after publication [market]) and agentic commerce (no binding rules yet [market]). Use them as talking points only.

Open question: how long after an event the buying window stays open. No source gives timing, so none is assumed.

## Not a fit
| Who | Why | Evidence |
|---|---|---|
| Below roughly $10M a year (or $1M a month) in volume | Below the typical floor, and the minimum monthly invoice raises their effective rate | [company], [signals §4] |
| Small merchants wanting self-serve, 24/7 support | Adyen support is criticised as "enterprise-mediated" and lagging Stripe's self-serve | [competitors] (Baremetrics via search) |
| Platforms that want to decouple payments from their core product | TWICE left Adyen for Stripe for that reason | [customers] |
| Sub-merchants and small payees of platforms | They are the source of the 1.3/5 Trustpilot score (payout delays, held funds). Reach them through the platform, not directly | [competitors], [signals §4] |

## Open questions
- What are the volume thresholds by segment? Only third-party figures exist so far.
- Volume processed per account (revenue bands are now in Layer 1).
- Which countries and licences does Adyen cover? (Not found)
- What is the win rate against Stripe compared with Worldpay? No switch stories from Stripe were found.
- What is the timing of Visa/Mastercard agentic dispute rules, and of PSD3?
