# Adyen signal matrix
**Status: work in progress (v0.1, 2026-11-13).** Validate it with sales before building.
- Built from `context/icp/icp.md`, `context/icp/personas.md` and `context/docs/research/`.
- Method: `knowledge-base/playbooks/signal-matrix-framework.md` and `workflow-anatomy.md`.
- Source tags: [icp], [personas], [stories] = brand-stories.md, [firmo] = firmographics.md, [signals], [market], [company], [customers], [competitors].
- INFERRED = not directly supported by the research.

## Matrix 1: Segments × personas
- **Rows:** ICP segments, split by how payments work for them [icp Layer 1].
- **Columns:** the three personas [personas].
- **Cells:** the opening angle in that buyer's own pain language. Every angle is anchored to a real customer story.
- **Company examples:** these are reference customers (proof), not prospects. Prospect names need Clay research with the filters shown, and none are invented here.

| Segment | Target definition (filterable) | Reference customers | Champion: Head of Payments | Economic buyer: CFO / Treasurer | Influencer: CTO / CIO | Lead product → upsell | Priority |
|---|---|---|---|---|---|---|---|
| **A1. Multi-country retail and consumer brands** | Retail, apparel, beauty or consumer goods. 201-10,000 employees, $50M+ revenue, sells in 2+ countries online and in stores, HQ in Europe or North America [firmo] | Aritzia, KIKO, On, Crocs, Moose Knuckles, Louboutin, AS Colour [customers, stories] | Too many PSPs and local acquirers. Moose Knuckles went from "at least four" to one [stories] | One view of payments data and cost across markets. "Before Adyen, our previous setup required various local acquirers." (Louboutin Treasurer) | One platform across online and store. "We had no omnichannel view" (PXG) | Unified Payments → Risk (Protect), Uplift (INFERRED order) | **High.** 22 of 33 merchant customers are in this group [firmo] |
| **A2. Hospitality, F&B and venues** | QSR, coffee, hotels, stadiums and attractions with many sites. Multi-country (INFERRED: site count is not in the research) | Starbucks Europe, Lavazza, HEYTEA, Joe & The Juice, FC Bayern, citizenM [customers, stories] | Speed at the till. FC Bayern: 3 seconds faster, about 10% more transactions [stories] | Rolling out new markets without a payments project. HEYTEA, Lavazza's 7-market rollout [stories, customers] | Terminals that work on any network. HEYTEA (Wi-Fi, 4G, Ethernet) [stories] | In-person payments → Unified Payments | **Medium.** Strong stories, fewer data signals (INFERRED) |
| **A3. Digital subscriptions, travel and gaming** | Streaming, apps, airlines, gaming and telecom. Card-not-present heavy, cross-border | DAZN, Transavia, Travian, Rebtel, Birchbox [stories] | Authorization rate. DAZN +5.19%, Travian up to 9% higher in key markets [stories] | Transparent pricing. Rebtel: "the last piece that pushed us to look for a new provider was pricing" | Downtime and checkout friction. Rebtel "real issues with downtime" [stories] | Online payments with Uplift → Risk | **Medium** |
| **B1. Vertical SaaS adding payments** | Software platforms for restaurants, attractions, fitness, field services, arts or ticketing, and their merchants pay through them. 10+ employees, any size [firmo] | Toast, Lightspeed, ROLLER, Tessitura, Teamfront, Sport Alliance, Zenchef, Secutix [customers, stories] | Payments as revenue. "Recurring revenue only comes by way of payments" (Teamfront) | New revenue line without building a payments company. "We didn't need a team of 40 payments experts" (Teamfront) | Onboarding speed. ROLLER "from onboarding customers in months to weeks" | Adyen for Platforms → Embedded Finance (accounts, cards, capital, e.g. Epos Now) | **High.** Platforms revenue +46% YoY [company] |
| **B2. Marketplaces with pay-ins and payouts** | Two-sided marketplaces (travel, resale, gig, mobility) that pay out to sellers or hosts | Etsy, Expedia, Vinted, BlaBlaCar, Freelancer [customers, stories] | One funds flow. "Unifying our funds flow on a single platform is a key driver for VintedPay's scalability." (Vinted) | Liquidity and faster capital: the Intelligent Money Movement pitch to "the CFO's office" [company, customers] | Fewer integrations across pay-ins and payouts (INFERRED) | Intelligent Money Movement → Platforms and Issuing | **Medium.** Fewer accounts, high value (INFERRED) |
| **C. Public sector** | Government payment services | GOV.UK Pay [customers] | n/a | Modernising "at scale" (Adyen quote) | n/a | n/a | **Low.** One data point; procurement-led, not outbound |

## Matrix 2: Buying signals
**Scoring.** Each of the four tests from workflow-anatomy.md is scored 1 to 5:
- **Why:** a clear buying reason.
- **Vol:** enough volume.
- **Data:** data we can get.
- **Story:** a natural story to tell.

**Status** uses the framework's labels: VERIFIED, COMPOUND (must be paired with another signal), UNTESTED or KILLED.

**Volume** for the hiring signals comes from LinkedIn job counts for the past month (treg `harvestapi.linkedin.search.jobs`, 2026-11-13). These are keyword matches, so they are upper bounds, not exact titles:
- Payments Product Manager: 2,900
- Treasury Manager: 2,365
- Head of Payments: 1,140
- Payments Manager: 911
- Fraud Manager: 487
- "Embedded Payments": 10
- "Director of Payments": 8,179 (too broad to use)

| # | Signal | Type | Segments | Mother intent (catalog) | Data source | Tool / cost | Why | Vol | Data | Story | Total /20 | Status |
|---|---|---|---|---|---|---|---|---|---|---|---|---|
| S1 | **Hiring for payments, fraud or treasury roles** (Head or VP of Payments, Payments PM, Fraud Manager, Treasury Manager) | Universal | A1, A2, A3, B1, B2 | Hiring specific roles | LinkedIn Jobs | treg `harvestapi.linkedin.search.jobs` ($0.001/call), then ICP filter | 4 | 5 | 5 | 4 | **18** | VERIFIED (method tested; counts above) |
| S2 | **SaaS platform launching its own payments** (hiring "Payments Product Manager", or a "/payments" product page appears) | Niche (B1) | B1 | New product launches + hiring | LinkedIn Jobs, then a check of the company website | treg harvestapi jobs + crawl4ai page check (INFERRED build) | 5 | 3 | 4 | 5 | **17** | VERIFIED (jobs); page check UNTESTED |
| S3 | **Newly adopted commerce platform** (Salesforce Commerce Cloud, new OMS or POS first seen recently) | Universal | A1, A2 | Companies using specific software (newly adopted) | PredictLeads technology detections with `first_seen_at_from` | treg `predictleads.companies.technology_detections` ($0.04/company) | 4 | 3 | 4 | 4 | **15** | UNTESTED. Precedent: Brooks Brothers, Crocs, KIKO switched alongside replatforming [stories] |
| S4 | **Several PSPs, or a competitor PSP, named in job posts** (e.g. Braintree, Stripe, Worldpay, PayPal) | Universal | A1, A3, B1, B2 | Competitor customers | PredictLeads detections seen on job openings | treg PredictLeads ($0.04/company) | 4 | 3 | 3 | 4 | **14** | COMPOUND. Only about 50% recall (4 of 8 known Adyen customers detected) [signals]. Pair with S1 or S5 |
| S5 | **Expanding into a new country** (first job posts in a country with no earlier postings) | Universal | A1, A2, A3, B2 | Hiring offshore / new market (niche-specific play) | LinkedIn Jobs with location filter, compared with HQ country | treg harvestapi jobs (`location` param) | 5 | 3 | 3 | 5 | **16** | UNTESTED build. Strongest switch precedent: BlaBlaCar, HEYTEA, Lavazza, MUJI [stories, customers] |
| S6 | **New payments leader or CFO (last 90 days)** | Universal | All | New leadership | People data with start dates | Clay "Find People" or a treg people search (not priced yet) | 3 | 3 | 3 | 3 | **12** | UNTESTED. INFERRED: no story shows a switch after a new hire |
| S7 | **Merger of two companies with payment stacks** | Niche | A1, A3, B2 | Niche-specific play (M&A) | News search | treg web search or Claygent (not priced) | 5 | 1 | 2 | 5 | **13** | UNTESTED. Precedent: Just Eat Takeaway [stories]. Low volume |
| S8 | **Third-party fraud tool on the site** (e.g. Forter) | Universal | A1, A3 | Companies using specific software | PredictLeads site detections | treg PredictLeads | 2 | 3 | 4 | 3 | **12** | COMPOUND. Adyen customers run it too [signals]. Use for the angle only |
| S9 | **Worldpay merchant during the Global Payments integration** | Niche | A1, A2 | Competitor customers | Undetermined | none tested | 3 | ? | 1 | 4 | n/a | INFERRED. No detection source works yet [icp Layer 2] |
| S10 | **Public payment complaints** (Trustpilot reviews mentioning checkout or payments) | Universal | A1, A3 | Niche-specific play | Trustpilot | treg `crawl4ai.trustpilot.business.reviews` ($0.00135/call) | 2 | 2 | 4 | 2 | **10** | UNTESTED on prospects. Risk of diagnosing gaps (framework guardrail) |
| S11 | **Lookalike of reference customers** | Universal | A1, B1 | Lookalike of customers | Enrichment (industry, size, Salesforce) | treg companyenrich ($0.0098) | 2 | 5 | 4 | 3 | **14** | COMPOUND. A static attribute, not timing. Use it as the ICP filter |

**Killed**
- **Fundraising:** the merchant segments are mostly mature, not VC-funded (framework rule). It may be worth retesting for B1 SaaS.
- **PSD3/PSR and agentic commerce rules:** there is no fixed date, so they are talking points only [market].
- **Front-end technographics for PSPs:** DataForSEO detected 0 of 8 [signals].

**Compounding:** 7 of 11 signals are universal. S1, S4, S5 and S11 use the same treg calls and only swap keywords per segment. S2 (B1) and S7 are the only unique builds.

## Top signals to build first
1. **S1. Payments, fraud or treasury hiring (18/20).** Highest volume (hundreds to thousands of posts a month), already tested at $0.001 a call, and it works for every segment with keyword swaps. The hire names the champion persona directly.
2. **S2. SaaS platform launching payments (17/20).** It feeds the fastest-growing segment (Platforms +46% YoY [company]), and the switch reason is explicit in Teamfront, Relay and Sport Alliance [stories]. It reuses the S1 build with platform keywords.
3. **S5. New-country expansion (16/20).** This is the most common switch moment in the customer stories, and the story is easy to tell ("as you open in X"). The build is untested: run a pilot on 50 companies to check that location-based detection works before scaling.
4. **S3. Newly adopted commerce platform (15/20).** Replatforming brings the payments decision with it (Crocs, KIKO, Brooks Brothers). It uses PredictLeads first-seen dates. Test cost: about $4 per 100 companies.
5. **S4 as a layer on S1 and S5.** It is not a stand-alone signal (about 50% recall), but naming the PSPs a company already uses routes the copy toward consolidation.

## Matrix 3: Workflow tracker
| W # | Status | Workflow | Mother intent | Segments | Persona | Campaigns | Launch | Clay URL | Copy URL | Leads |
|---|---|---|---|---|---|---|---|---|---|---|
| W1 | Planned | Hiring payments/fraud/treasury roles | Hiring specific roles | A1, A3, B1, B2 | Head of Payments | | | | | |
| W2 | Planned | SaaS launching embedded payments | New product launch | B1 | Head of Payments / CPO | | | | | |
| W3 | Planned | New-country expansion | New market | A1, A2, B2 | CFO / Head of Payments | | | | | |
| W4 | Planned | Newly adopted commerce platform | Specific software (new) | A1, A2 | CTO / CIO | | | | | |
| W5 | Planned | Multi-PSP consolidation (S4 layered on W1 and W3) | Competitor customers | A1, B1 | Head of Payments | | | | | |
| W6 | Planned | New payments leader or CFO | New leadership | All | CFO | | | | | |
| W7 | Planned | Merger payment-stack unification | M&A | A1, A3 | CFO | | | | | |

## Open questions
- Prospect company names per cell: run Clay with the Matrix 1 filters.
- A2 site counts: no data source tested (Google Places is a candidate).
- Real conversion of S1 to meetings: no outbound data yet (knowledge-base/learnings is empty).
- Whether B2 and A3 have enough accounts in the ICP bands to sustain workflows.
