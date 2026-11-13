# Adyen buyer personas
Built only from `context/docs/research/` (2026-11-13). Quotes are verbatim from real customers in [stories] = brand-stories.md or [customers]. "Measured on" is INFERRED from the outcomes those buyers report.

## 1. Champion: Head of Payments
| | |
|---|---|
| Titles | VP of Payments (Teamfront, Vinted), VP Global Payments (Expedia), VP Business Development, Payments (DAZN), Senior Lead Payments and Risk (On), Senior Manager Payment (Secutix) |
| Measured on (INFERRED) | Authorization rate, payment conversion, fraud and chargeback rate, payment cost, speed of adding markets and payment methods |
| Pains, in their words | 1. "We had five or six integrated payment partners and it was incredibly difficult to manage" (Teamfront) 2. "Since we were working with multiple payment providers in multiple countries, new integrations were complex and delayed expansion" (Connect&GO) 3. "Our customers want to watch sport live so the worst possible scenario would be to have a major event and someone being unable to pay" (DAZN) |
| Starts looking when | Entering new countries, launching embedded payments, seeing fraud spike, or merging with another business [stories §3] |
| Objections | Migration effort and risk (Adyen counters with "The integration took less than three months", Teamfront). Support model: "enterprise-mediated support that lags Stripe's 24/7 self-serve" [competitors]. Losing leverage by consolidating: the common setup is one primary provider for 80-90% plus a backup [competitors] |

## 2. Economic buyer: CFO or Treasurer
| | |
|---|---|
| Titles | CFO (AS Colour), Group Treasurer (Christian Louboutin). Adyen's own CFO addresses "the CFO's office" and "treasurers" [customers] |
| Measured on (INFERRED) | Total payment cost, cash and liquidity speed, data quality for reconciliation, operational overhead |
| Pains, in their words | 1. "Before Adyen, our previous setup required various local acquirers." (Louboutin Treasurer) 2. "Payments are a critical touchpoint in our brand experience," (AS Colour CFO) 3. "the last piece that pushed us to look for a new provider was pricing" (Rebtel) |
| Starts looking when | Pricing renegotiation stalls, payouts and liquidity are fragmented (Intelligent Money Movement pitch), or the business is replatforming [stories, company] |
| Objections | Price and minimums: interchange++ with a ~0.60% markup and a minimum monthly invoice [company]. Lock-in and concentration risk (open question: no direct source). Recent CFO change at Adyen itself is unlikely to matter to buyers (INFERRED) |

## 3. Influencer: CTO, CIO or engineering lead
| | |
|---|---|
| Titles | CTO (Birchbox), CIO (Murdoch's), Chief Engineer and Co-Founder (Tessitura), Head of ICT (Hillarys), VP Global ecommerce Technology and Operations (Crocs) |
| Measured on (INFERRED) | Integration effort, uptime, PCI scope, speed of shipping checkout features |
| Pains, in their words | 1. "Troubleshooting meant digging through multiple systems to find what happened with a transaction." (FAO Schwarz) 2. "We had no omnichannel view" (PXG) 3. "On top of the technical challenges we had some real issues with downtime." (Rebtel) |
| Starts looking when | The legacy platform can't scale, PCI burden grows, or a new channel (in-store, app, agentic) needs payments [stories, market] |
| Objections | Stripe is the developer default [competitors]. Setup complexity is a known criticism [competitors]. Adyen's counter: "The Adyen API is amazing. It's just incredibly extensive and incredibly well-documented." (Tessitura) |

## Open questions
- Real KPIs per role: none of the sources state them directly.
- Who signs at platforms compared with merchants (for example CPO or GM Fintech at Toast)?
- Objections from lost deals: no loss data in the research.
