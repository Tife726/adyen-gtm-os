# Signals: evidence a company needs Adyen now (raw)
Checked 2026-11-13. Each signal is linked to evidence where found.

## 1. Expanding into new countries
- Lavazza: unifying payments, 7-market rollout. AS Colour: consolidating global payments. Toast: expanding with Adyen into the US. Sources in customers.md.
- INFERRED: "consolidate multi-country PSPs into one" is the recurring buying story in 2026 announcements.

## 2. Hiring for payments, fintech, treasury or fraud roles
- treg LinkedIn job search works ($0.0005/call). Test results:
  - Adyen itself: "Senior Payments Partnerships Manager - Visa", London, posted 2026-11-03. https://www.linkedin.com/jobs/view/4477916384
  - A "Head of Payments" past-week search returned no parsed rows. Re-run with company or industry filters in Clay.

## 3. Using Stripe, Checkout.com or Worldpay
- Stripe weaknesses cited: cross-border declines on its own acquiring network (claimed by a comparison site). Source: https://thefinrate.com/why-businesses-are-ditching-stripe-for-adyen/ (checked 2026-11-13)
- Worldpay ownership churn (Global Payments deal). Source: https://www.shuttleglobal.com/guides/adyen-vs-worldpay (checked 2026-11-13)
- No documented public case of a company switching from Stripe to Adyen was found. The reverse exists (TWICE).

## 4. Customer complaints about payment experience
- Not found for prospects. Method: Trustpilot reviews via treg ($0.00135/call) work and can be run on target domains.
- Note: Adyen's own Trustpilot is 1.3/5 from 451 reviews. Complaints focus on payout delays and held funds from sub-merchants (e.g. via YetiPay, Reverb). Quote: "They tell that we don t accept buisness that can t make at least 1M$ per month." Source: https://www.trustpilot.com/review/www.adyen.com (checked 2026-11-13)

## 5. Digital transformation or payment infrastructure upgrade
- GOV.UK Pay migrating ~1,000 services. Starbucks Europe moving 940+ stores. Sources in customers.md.

## 6. High fraud or chargebacks
- Agentic commerce dispute liability is unsettled (see market.md). INFERRED: merchants launching AI-agent checkout are a fraud-risk signal.
- Company-level fraud data: Not found.

## 7. Competitors of Adyen's successful customers
- Lookalike seeds: Etsy, Expedia, Vinted, Toast, Starbucks, Aritzia, Lavazza, Acne Studios, Bolt, HelloFresh, Klook (sources in customers.md). INFERRED examples: eBay, Booking.com, Depop, Square, Costa, Lululemon, illy.

## Evidence from Adyen's own customer stories
See brand-stories.md section 3. The most common pre-switch trigger is 3 to 6+ fragmented providers or local acquirers. Then come country expansion, vertical SaaS launching embedded payments, fraud losses, and mergers.

## Technographic detection test (2026-11-13)
Can a data source detect which payment providers a company uses? I tested this on 8 known Adyen customers (Aritzia, PXG, On, Crocs, KIKO, Etsy, Vinted, DAZN), so the right answer was known in advance.

| Source (treg) | Price | Adyen detected | Where the evidence came from | Verdict |
|---|---|---|---|---|
| `predictleads.companies.technology_detections` | $0.04/call | 4 of 8 (Aritzia, Crocs, Etsy, On) | Job postings only. It never detected Adyen from the website itself | Usable, with about 50% recall. Missing PSP data does not mean the company has no PSP |
| `dataforseo.x.domain-analytics-technologies-domain-technologies-live` | $0.012/call | 0 of 8 | Front-end scan: it saw only Klarna and Afterpay | Not usable for PSPs |
| `companyenrich.companies.enrich` (earlier run) | $0.0098/call | 0 of 54 | n/a | Not usable for PSPs |

What PredictLeads detects reliably, from the websites themselves:
- **BNPL and wallets:** Klarna, Afterpay, Affirm, PayPal.
- **Third-party fraud tools:** Forter at Aritzia, Crocs and KIKO.

Other findings:
- **More than one PSP at once:** Crocs shows Adyen plus Braintree and PayPal (all from job postings, last seen 2026-08 to 2026-10).
- **Noise at marketplaces:** Etsy shows Stripe, Square and WooCommerce on its subpages. These most likely come from sellers' content, not Etsy's own stack (INFERRED). Detections from subpages at marketplaces are unreliable.
- **Cost:** $0.416 for 16 calls.
