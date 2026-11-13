# Campaign strategy: Adyen (v0.1, 2026-11-13)
**Inputs:**
- `CLAUDE.md`
- `context/icp/` (icp.md, personas.md, signal-matrix.md)
- `context/docs/research/` (incl. brand-stories.md, firmographics.md, signals.md)
- `context/docs/gtm-system.md`
- `knowledge-base/playbooks/workflow-anatomy.md`

**Status:** ideas only. Volumes marked INFERRED have no measurement behind them.

**Before any copy names a customer:** `context/messaging/facts.md` does not exist yet. Customer names and metrics below come from Adyen's published stories [research]. Each one must be approved into facts.md before it appears in copy (`campaign-copy` facts gate).

**Base parameters**
- **Who:** Segments A1 (multi-country retail and consumer brands), A2 (hospitality, F&B, venues), A3 (digital subscriptions, travel, gaming), B1 (vertical SaaS adding payments) and B2 (marketplaces).
- **Merchant size:** 201-10K employees and $50M+ revenue.
- **Platform size:** 10+ employees.
- **Where:** HQ in Europe or North America first [icp].
- **Personas:** Head of Payments (champion), CFO or Treasurer (economic buyer), CTO or CIO (influencer) [personas].
- **Core offer:** "One platform for payments, data, and financial products" [voice].
- **Differentiators in the research:**
  - One platform across online and in-person.
  - Local acquiring.
  - Interchange++ pricing.
  - Platforms and embedded finance.
  - Intelligent Money Movement (pay-ins, liquidity, payouts).

---

## Customer discovery analysis (summary)
The full detail is in `context/docs/research/brand-stories.md` and `firmographics.md`.

| Pattern | Evidence |
|---|---|
| Most common switch trigger: 3 to 6+ PSPs or local acquirers | Teamfront, Tessitura, Moose Knuckles, Louboutin, SKIDATA, Connect&GO |
| Second trigger: country expansion | BlaBlaCar (6 countries in a year), HEYTEA, MUJI, Lavazza |
| Third trigger: SaaS launching embedded payments | Teamfront, Relay, ROLLER, Sport Alliance, Lightspeed |
| Industries | Retail and consumer brands are 22 of 33 merchant customers. Then hospitality and F&B, then vertical SaaS |
| Who bought or spoke | VP or Head of Payments, CFO, Group Treasurer, CTO, CIO, Chief Engineer |

**Targeting challenges (INFERRED):**
- **Platforms of any size win.** 7 of 18 platform customers have 200 or fewer employees, so B1 should not carry a merchant-size filter.
- **Commerce partners drive deals.** New Black's partner story shows commerce partners bringing merchants to Adyen (Creative non-ICP idea #16).

**Best lookalike seeds:**
- Moose Knuckles: four PSPs down to one.
- KIKO Milano: 1,000 stores in under 9 months.
- PXG: fraud -97%.
- Teamfront: "five or six" payment partners down to one.
- DAZN: authorization +5.19%.

---

## Campaign ideas (broad → niche)
**Column key:**
- **Sig** = signal strength (1-5).
- **Data** = how easy the data is to get (1-5).
- **Rank** = position by Sig + Data (ties broken by volume).

Volume per month is the matched prospect companies **before** the ICP filter, unless noted:
- "LinkedIn count" = past-month job posts from treg `harvestapi.linkedin.search.jobs` on 2026-11-13. These are keyword matches, so they are upper bounds [signal-matrix].
- Expect a large cut after the ICP filter. Treat every post-filter number as INFERRED until measured.

| # | Campaign | Level | Mother intent | Tier | Segment / persona | Signal and data source | AI strategy | Angle (value prop) | Volume / month | Difficulty | Sig | Data | Rank |
|---|---|---|---|---|---|---|---|---|---|---|---|---|---|
| 1 | **Creative Ideas: 3 payment ideas** | Broad | Niche-specific play | Tier 2: no timing signal | A1, A2 / Head of Payments | None. Base ICP list from enrichment (companyenrich) | Claygent reads the site (countries served, stores, checkout methods, apps) and writes 3 ideas matched to Adyen products | Make money (conversion), save money (one stack) | Bounded by the ICP list; not measured | Medium | 2 | 4 | 12 |
| 2 | **Lookalike: brands like our retail customers** | Broad | Lookalike of customers | Tier 2: static, but proven seeds | A1 / Head of Payments, CFO | Enrichment: retail or consumer, 201-10K employees, $50M+, Salesforce stack [firmo] | Pick the closest reference customer by industry and HQ, and cite its result | Save money, make money | Bounded by the ICP list; not measured | Low | 2 | 5 | 9 |
| 3 | **Payments team hiring** | Focused | Hiring specific roles | **Tier 1**: tested source, highest volume, all segments | A1, A3, B1, B2 / Head of Payments (the hiring manager) | LinkedIn Jobs: "Head of Payments" (1,140), "Payments Manager" (911) | Parse the job description for countries, PSPs named and scope. Mention the role in the opener | Save time, make money: "a new payments hire usually inherits the PSP mix" | ~2,000 raw posts (LinkedIn count); post-ICP not measured | Low | 4 | 5 | **1** |
| 4 | **Fraud team hiring** | Focused | Hiring specific roles | **Tier 1**: clear pain, tested source | A1, A3 / Head of Payments or Risk | LinkedIn Jobs: "Fraud Manager" (487) | Pull the fraud scope from the job description (chargebacks, manual review, tools named) | Mitigate risk: PXG -97% fraud; On cut manual rule effort 60% | ~490 raw (LinkedIn count) | Low | 4 | 5 | **2** |
| 5 | **SaaS launching payments** | Focused | New product launches + hiring | **Tier 1**: fastest-growing segment, explicit trigger | B1 / VP Product or Payments, CPO | LinkedIn Jobs: "Payments Product Manager" (2,900), filtered to software companies | Parse the job description for "embedded payments" or "payfac" language. Optionally confirm a /payments page | Make money: "Recurring revenue only comes by way of payments" (Teamfront) | 2,900 raw, but mostly non-SaaS. B1 share not measured | Medium | 5 | 4 | **3** |
| 6 | **Treasury and payouts hiring** | Focused | Hiring specific roles | Tier 2: keyword is broad (corporate treasury) | B2, A1 / CFO, Treasurer | LinkedIn Jobs: "Treasury Manager" (2,365), filtered to marketplaces | Parse for payouts, liquidity or FX scope | Save time, save money: one funds flow (Vinted, Expedia) | ~2,365 raw; the marketplace share is small (INFERRED) | Medium | 3 | 4 | 7 |
| 7 | **New Hire: new payments leader or CFO** | Focused | New leadership (past 90 days) | Tier 2: data untested, no switch precedent in research | All / new Head of Payments, CFO | Clay Find People filtered by start date (not yet tested) | Pull the start date and previous company. Did their old company run Adyen? | Make money fast: "first 90 days, the PSP review" (INFERRED) | Not measured | Medium | 3 | 3 | 10 |
| 8 | **New country launch** | Focused | Hiring offshore / new market | Tier 2: strongest precedent, untested build | A1, A2, B2 / CFO, Head of Payments | First LinkedIn job posts in a country with no earlier posts (harvestapi `location`) | Name the country and the local methods that matter there (iDEAL, BLIK, Pix from the BlaBlaCar story) | Make money: "add a market by adding a payment method" (BlaBlaCar) | Not measured; pilot on 50 first | Medium | 5 | 3 | 4 |
| 9 | **Omnichannel: first store or new store market** | Focused | Hiring specific roles (store openings) | Tier 2: data is noisy | A1 DTC brands / Head of eCommerce, CFO | LinkedIn Jobs: "Store Manager" or "Retail Opening" at online-first brands in a new city | Name the city. Check whether the brand has stores (Claygent) | Save time: one platform online and in store (Brompton's first US store, KIKO) | Not measured | Medium | 4 | 3 | 8 |
| 10 | **Replatforming commerce** | Focused | Companies using specific software (newly adopted) | Tier 2: untested, about $0.04 per company | A1, A2 / CTO, CIO | PredictLeads detections with `first_seen_at_from` (Salesforce Commerce Cloud, New Black) | Name the new platform. "Payments usually move with the replatform" (Crocs, KIKO, Brooks Brothers) | Save time | Not measured | Medium | 4 | 3 | 6 |
| 11 | **Multi-PSP consolidation** | Niche | Competitor customers | Tier 2: about 50% detection recall | A1, B1 / Head of Payments | PredictLeads: 2+ PSPs named in job posts [signals] | List the PSPs they run. Cite the Moose Knuckles 4-to-1 consolidation | Save money, save time | Not measured; a subset of #3 and #2 | Medium | 4 | 3 | 5 |
| 12 | **Competitor stack displacement** | Niche | Competitor customers | Tier 2: same detection limits | A1, A3 / Head of Payments | PredictLeads job-post detections: Braintree, Stripe, Worldpay | Swap the angle per competitor. Worldpay angle: the Global Payments integration (INFERRED) | Mitigate risk, save money | Not measured | Medium | 3 | 3 | 11 |
| 13 | **Authorization-rate JD parse (Creative Stretch)** | Niche | Website content mentions (job descriptions) | Tier 2: needs job-description text | A3, A1 / Head of Payments | Job descriptions mentioning "authorization rate", "declines", "chargebacks" or "payment success" (harvestapi, then the description) | Quote their own job-description words back to them | Make money: DAZN +5.19%, Travian up to 9% higher in key markets | Not measured | Medium | 4 | 3 | 13 |
| 14 | **Golden ICP: payments hire + 2+ PSPs + new country** | Niche | Hiring specific roles (stacked) | Tier 2: low volume, very high relevance | A1 / Head of Payments, CFO | #3, #11 and #8 stacked | Everything a rep would find in 10 minutes: role, PSPs, new market | Save money, make money | Low; not measured (INFERRED tens) | High | 5 | 2 | 14 |
| 15 | **Funded vertical SaaS without payments** | Niche | Companies fundraising | Tier 3: funding fits B1 only; absence of payments is hard to prove | B1 / CEO, CPO | Funding data (Crunchbase via Clay, not tested), plus hiring "Payments" in job descriptions as positive proof | Name the round. "Payments as the next revenue line" | Make money | Not measured | High | 3 | 2 | 16 |
| 16 | **Creative (non-ICP): commerce partners and SIs** | Niche | Niche-specific play (partners) | Tier 2: indirect, but proven by the New Black story | Agencies and integrators building Salesforce Commerce Cloud, New Black or Teamwork projects / partner leads | Partner directories of those platforms (scrape; not tested) | Find their client logos that overlap with the ICP | Make money: win rates and referral share (New Black "increases win rates") | Not measured | Medium | 3 | 3 | 15 |
| 17 | **Creative (non-ICP): P&C insurers on Guidewire** | Niche | Niche-specific play | Tier 3: outside the ICP segments | Insurers / CFO, Head of Claims Payments | Guidewire customer lists, plus Adyen's Guidewire Marketplace listing [customers] | Claygent checks for Guidewire mentions and payout scope | Save time: pay-ins and claim payouts on one platform (IMM targets insurance [company]) | Not measured | High | 2 | 2 | 18 |
| 18 | **M&A: merging payment stacks** | Niche | Niche-specific play (M&A) | Tier 3: very low volume | A1, A3, B2 / CFO | News search (Claygent or treg web search, not priced) | Name both brands and their PSPs if detectable | Save money: Just Eat Takeaway (86% faster rollouts) | Very low (INFERRED a handful) | High | 5 | 1 | 17 |
| 19 | **Agentic checkout readiness (Creative Stretch)** | Niche | New product launches | Tier 3: speculative, no binding rules yet [market] | A1, A3 / CTO, Head of eCommerce | Brands announcing AI shopping assistants or ChatGPT checkout (news, Claygent) | Reference their launch. Adyen Agentic APIs [company] | Make money: sell in AI channels without rebuilding | Not measured | High | 2 | 2 | 19 |

**Signals considered and dropped:**
- **High transaction volume:** not observable, so it is used as an ICP filter (revenue $50M+), not a signal.
- **Customer complaints about payments:** Trustpilot data is avoided per the skill's data rules. The research also shows that Adyen's own Trustpilot is mostly payee complaints.

### Campaign overviews (for copy handoff)
1. **Creative Ideas**
   - **Hook:** "I had three payment ideas for {{company}}." The bullets come from their site: countries served and the matching local method, stores plus online combined in one view, app checkout friction.
   - **Proof:** one reference customer matched by industry.
   - **CTA:** "Worth a look at any of these?"
2. **Lookalike**
   - **Hook:** "{{reference_customer}} went from several payment partners to one."
   - **Personalisation:** pick the reference by segment and HQ.
   - **CTA:** "Is your setup closer to before or after?"
3. **Payments team hiring** (see the recommendation below)
   - **Hook:** "Saw you're hiring a {{job_title}}."
   - **Angle:** whoever takes the role inherits the PSP and acquirer mix. Use the countries and PSPs named in the job description.
   - **Proof:** Teamfront's "hodgepodge of vendors" quote, or Moose Knuckles.
   - **CTA:** a short question on how many PSPs they run today.
4. **Fraud team hiring**
   - **Hook:** "You're hiring a {{fraud_title}}."
   - **Angle:** fraud tools built into the payments platform.
   - **Proof:** PXG fraud -97% and $1.5M saved in year one; On cut manual risk-rule effort 60%.
   - **CTA:** "Are chargebacks or manual review the bigger load today?"
5. **SaaS launching payments**
   - **Hook:** "Looks like {{company}} is building payments into the product."
   - **Angle:** a new revenue line without a 40-person payments team (Teamfront).
   - **Personalisation:** their vertical's reference platform (ROLLER for attractions, Lightspeed or Zenchef for restaurants).
   - **CTA:** "Build, partner, or still deciding?"
6. **Treasury and payouts:** one funds flow for pay-ins and payouts. Proof: Vinted ("Unifying our funds flow…"). CFO persona.
7. **New Hire:** welcome note in the first 90 days. "Most new payments leaders map the PSP setup first" (INFERRED framing; test it). Previous company named.
8. **New country:** "As {{company}} opens in {{country}}." The local methods that matter there, plus BlaBlaCar's expansion quote.
9. **Omnichannel:** "First store in {{city}}." One view across online and store (Brompton's first US store, KIKO).
10. **Replatforming:** "Moving to {{platform}}." Payments often move in the same project (Crocs with Commerce Cloud, KIKO with New Black).
11. **Multi-PSP:** "{{psp_list}} across {{n}} markets." Consolidation proof.
12. **Competitor displacement:** one angle per competitor, for example Worldpay customers during the Global Payments integration (INFERRED). Keep it factual and never disparage.
13. **Authorization-rate JD:** quote their job-description phrase ("improve payment success rates"). DAZN or Travian proof.
14. **Golden ICP:** all three facts in one short email, sent from a named AE.
15. **Funded SaaS:** "Congrats on the {{round}}. Payments as the next revenue line." A B1 proof story.
16. **Partners:** "Your clients like {{overlap_logo}} run {{platform}}. New Black grew win rates with Adyen as a partner." The CTA asks about a partnership, not a sale.
17. **Insurers:** the Guidewire integration, plus pay-ins and claim payouts on one platform.
18. **M&A:** "Congrats on bringing {{a}} and {{b}} together." The payments stack is usually among the most fragmented areas (Just Eat Takeaway).
19. **Agentic:** "Saw the {{AI assistant}} launch." Taking payments inside AI channels. Use it as a talking point only, with no claims about regulation.

---

## Ranked: signal strength + data ease
| Rank | # | Campaign | Sig + Data | Tier |
|---|---|---|---|---|
| 1 | 3 | Payments team hiring | 9 | Tier 1 |
| 2 | 4 | Fraud team hiring | 9 | Tier 1 |
| 3 | 5 | SaaS launching payments | 9 | Tier 1 |
| 4 | 8 | New country launch | 8 | Tier 2 |
| 5 | 11 | Multi-PSP consolidation | 7 | Tier 2 |
| 6 | 10 | Replatforming commerce | 7 | Tier 2 |
| 7 | 6 | Treasury and payouts hiring | 7 | Tier 2 |
| 8 | 9 | Omnichannel store openings | 7 | Tier 2 |
| 9 | 2 | Lookalike | 7 | Tier 2 |
| 10 | 7 | New Hire | 6 | Tier 2 |
| 11 | 12 | Competitor displacement | 6 | Tier 2 |
| 12 | 1 | Creative Ideas | 6 | Tier 2 |
| 13 | 13 | Authorization-rate JD parse | 7 (lower volume) | Tier 2 |
| 14 | 14 | Golden ICP stack | 7 (very low volume) | Tier 2 |
| 15 | 16 | Partners and SIs (non-ICP) | 6 | Tier 2 |
| 16 | 15 | Funded vertical SaaS | 5 | Tier 3 |
| 17 | 18 | M&A | 6 (very low volume) | Tier 3 |
| 18 | 17 | Insurers on Guidewire (non-ICP) | 4 | Tier 3 |
| 19 | 19 | Agentic checkout | 4 | Tier 3 |

## No-AI campaign
**"Four to one"**
- **Who:** Heads of Payments at A1 brands where 2 or more PSPs were detected (#11).
- **Why it works without AI:** the list filter is the message. They run several PSPs, and a comparable brand went to one.
- **Message:** one line on the setup ("Several payment partners across your markets?"), one proof line (Moose Knuckles: at least four down to one, once approved into facts.md), and one question ("Worth comparing notes?").

## Front-end offers
1. **PSP consolidation map:** a one-page view of their markets, likely local methods, and where several providers overlap. It is built from public data before the call. Low effort, and it shows the research.
2. **Forrester TEI study (commissioned by Adyen)** for the CFO persona. It is an existing published asset [customers]. Don't quote numbers from it until they are added to research.
3. **Interchange++ cost walk-through:** a review of how their current blended rate compares with an interchange++ structure. Rebtel left its provider over pricing transparency [stories]. INFERRED: needs sales to agree to offer it.

---

## Recommendation: build #3 "Payments team hiring" first
- **Strongest data:** it is the only signal tested end to end (treg LinkedIn Jobs at $0.001 a call), with the highest volume, around 2,000 raw posts a month.
- **Composable:** the same build becomes #4 (fraud), #5 (SaaS payments PM) and #6 (treasury) by swapping keywords. One Clay table, four campaigns.
- **Names the champion:** the hiring manager is usually the Head of Payments persona, and the job description gives countries and PSPs for personalisation.
- **Matches the plan:** it is S1 and workflow W1 in `signal-matrix.md`, and it follows the system in `gtm-system.md`.

**Before launch:**
1. Measure the post-ICP volume on one week of posts.
2. Write `context/messaging/facts.md` with the approved proof points.

**Next step:** `/build-workflow-spec` on idea #3.
