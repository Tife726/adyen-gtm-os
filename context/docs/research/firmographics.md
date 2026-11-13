# Firmographics of named Adyen customers (raw)
- Source: treg `companyenrich.companies.enrich`, called by domain on 2026-11-13. It cost $0.0098 per call, $0.53 for 54 calls.
- The sample is the 54 customers named in customers.md and brand-stories.md. Adyen picked these brands to showcase, so the sample is biased toward well-known brands.
- Data quality: the provider sometimes matches a regional entity or the wrong company. The Flag column marks rows that conflict with what we already know or are probably wrong.
  - BAD = wrong company, excluded from the counts.
  - SUSPECT = kept in the counts but probably undercounted.
- The provider detected no payment providers in any company's technology data, so it cannot serve as a PSP technographic source.

| Segment | Domain | Name returned | Employees | Revenue (USD) | Industry | HQ country | Type | Commerce tech | Flag |
|---|---|---|---|---|---|---|---|---|---|
| merchant | ascolour.com | AS Colour | 1-10 | n/a | Retail | United States | self-employed |  | BAD: matched a 1-10 person US entity |
| merchant | aritzia.com | Aritzia | 5K-10K | over-1b | Retail | Canada | public | Salesforce Commerce Cloud, Salesforce-Commerce-Cloud |  |
| merchant | lavazza.com | Lavazza | 201-500 | 200m-1b | Manufacturing | Italy | private |  | SUSPECT: undercount |
| merchant | starbucks.com | Starbucks | over-10K | over-1b | Hospitality | United States | public |  |  |
| merchant | pxg.com | PXG | 201-500 | 50m-100m | Retail | United States | private | Salesforce |  |
| merchant | muji.com | Muji | 51-200 | 10m-50m | Retail | Malaysia | private |  | SUSPECT: regional entity |
| merchant | dazn.com | DAZN | 1K-5K | 200m-1b | Media & Internet | United Kingdom | private |  |  |
| merchant | blablacar.com | BlaBlaCar | 501-1K | 200m-1b | Hospitality | France | private |  |  |
| merchant | birchbox.com | Birchbox | 11-50 | 50m-100m | Retail | United States | private | Shopify Plus |  |
| merchant | travian.com | Travian Games | n/a | under-1m | Software | Germany | None |  | SUSPECT: revenue looks wrong |
| merchant | pradagroup.com | Prada Group | 501-1K | n/a | Retail | Italy | public |  | SUSPECT: undercount |
| merchant | fcbayern.com | FC Bayern Munich | 501-1K | n/a | Hospitality | Germany | None |  |  |
| merchant | oriflame.com | Oriflame | 5K-10K | 200m-1b | Manufacturing | Switzerland | private | Salesforce |  |
| merchant | recharge.com | Recharge.com | 51-200 | 10m-50m | Retail | Netherlands | private |  |  |
| merchant | on.com | On | 1K-5K | over-1b | Manufacturing | Switzerland | public | Salesforce |  |
| merchant | kikocosmetics.com | KIKO Milano | 1K-5K | 200m-1b | Retail | Italy | private | Salesforce |  |
| merchant | brooksbrothers.com | Brooks Brothers | 1K-5K | n/a | Retail | United States | private |  |  |
| merchant | christianlouboutin.com | Christian Louboutin | 1K-5K | 50m-100m | Retail | Italy | private |  | SUSPECT: HQ and revenue look wrong |
| merchant | crocs.com | Crocs | 1K-5K | over-1b | Retail | United States | public | Salesforce |  |
| merchant | faoschwarz.com | FAO Schwarz | 51-200 | n/a | Retail | United States | private | Shopify |  |
| merchant | mooseknucklescanada.com | Moose Knuckles | 201-500 | 10m-50m | Retail | Canada | private |  |  |
| merchant | murdochs.com | Murdoch's | 51-200 | 200m-1b | Retail | United States | private |  | SUSPECT: headcount low for revenue |
| merchant | rebtel.com | Rebtel | 51-200 | 50m-100m | Telecommunications | Sweden | private |  |  |
| merchant | hillarys.co.uk | Hillarys | 201-500 | 50m-100m | Manufacturing | United Kingdom | private |  |  |
| merchant | citizenm.com | Marriott International | over-10K | over-1b | Hospitality | United States | public |  | BAD: resolved to Marriott parent |
| merchant | joejuice.com | Joe and the Juice | 1K-5K | 200m-1b | Hospitality | Denmark | public |  |  |
| merchant | aceandtate.com | Ace & Tate | 201-500 | 10m-50m | Retail | Netherlands | private |  |  |
| merchant | heytea.com | HEYTEA | 201-500 | 50m-100m | Hospitality | China | private |  |  |
| merchant | smcp.com | SMCP | 501-1K | over-1b | Retail | France | public | Salesforce |  |
| merchant | tinyme.com | Tinyme | 11-50 | n/a | n/a | Australia | private |  |  |
| merchant | transavia.com | Transavia | 1K-5K | over-1b | Transportation | Netherlands | private | Salesforce |  |
| merchant | brompton.com | Brompton | 201-500 | 50m-100m | Manufacturing | United Kingdom | private | Salesforce |  |
| merchant | etam.com | Etam | 501-1K | 50m-100m | Retail | France | public | Salesforce-Commerce-Cloud |  |
| merchant | justeattakeaway.com | Just Eat Takeaway | 5K-10K | over-1b | Retail | Netherlands | public |  |  |
| merchant | foodora.com | foodora | 1K-5K | 200m-1b | Transportation | Germany | partnership |  |  |
| platform | toasttab.com | Toast | 5K-10K | over-1b | Software | United States | public |  |  |
| platform | etsy.com | Etsy | 5K-10K | over-1b | Retail | United States | public |  |  |
| platform | expediagroup.com | Expedia Group | over-10K | n/a | Hospitality | United States | public |  |  |
| platform | vinted.com | Vinted | 1K-5K | 200m-1b | Retail | Lithuania | private |  |  |
| platform | freelancer.com | Freelancer | over-10K | 50m-100m | Media & Internet | Australia | public |  | SUSPECT: employees likely include freelancers |
| platform | teamfront.com | Teamfront | 11-50 | n/a | Business Services | United States | private |  |  |
| platform | tessitura.com | Tessitura | 201-500 | 10m-50m | Software | United States | nonprofit | Salesforce |  |
| platform | roller.software | ROLLER | 201-500 | 10m-50m | Software | Australia | private |  |  |
| platform | connectandgo.com | HugeDomains.com | 11-50 | 10m-50m | Retail | United States | private |  | BAD: resolved to HugeDomains |
| platform | sportalliance.com | Sport Alliance | 51-200 | 10m-50m | Software | Germany | public | Salesforce |  |
| platform | lightspeedhq.com | Lightspeed | 1K-5K | 200m-1b | Software | Canada | public |  |  |
| platform | zenchef.com | Zenchef | 201-500 | 1m-10m | Software | France | private | Salesforce-Account-Engagement |  |
| platform | secutix.com | SECUTIX | 51-200 | 10m-50m | Software | Switzerland | private | Salesforce |  |
| platform | skidata.com | SKIDATA | 201-500 | 200m-1b | n/a | Austria | public | Salesforce |  |
| platform | eposnow.com | Epos Now | 201-500 | 50m-100m | Finance | United Kingdom | private | Salesforce, Salesforce-Account-Engagement |  |
| platform | yetipay.me | yetipay | 11-50 | n/a | Finance | United Kingdom | private |  |  |
| platform | nextcommerce.com | Next Commerce | 1-10 | n/a | Software | Singapore | private |  | SUSPECT: undercount |
| platform | relayfi.com | Relay | 1-10 | 1m-10m | Finance | Canada | private | Salesforce | SUSPECT: may not be the Relay in the Adyen story |
| platform | atlas.kitchen | Atlas | 11-50 | n/a | Software | Singapore | private |  |  |

## Summary counts (excluding the 3 BAD rows)
**Merchants (n=33)**
- Employees:
  - 11-50: 2
  - 51-200: 5
  - 201-500: 7
  - 501-1K: 5
  - 1K-5K: 9
  - 5K-10K: 3
  - 10K+: 1
  - n/a: 1
- Result: 25 of 32 with data (78%) have 201+ employees.
- Revenue:
  - 10M-50M: 4
  - 50M-100M: 8
  - 200M-1B: 8
  - over 1B: 7
  - under 1M: 1 (suspect)
  - n/a: 5
- Result: 23 of 28 with data (82%) report $50M+.
- HQ:
  - Europe: 21 (Netherlands 4, Italy 4, UK 3, France 3, Germany 3, Switzerland 2, Sweden 1, Denmark 1)
  - North America: 9 (US 7, Canada 2)
  - APAC: 3
- Industry label:
  - Retail: 17
  - Hospitality: 5
  - Manufacturing (consumer brands): 5
  - Transportation: 2
  - Media, Software, Telecom: 1 each
  - n/a: 1
- Commerce stack: 10 of 33 show Salesforce (incl. Commerce Cloud at Aritzia and Etam). 2 show Shopify (Birchbox, FAO Schwarz).

**Platforms (n=18)**
- Employees:
  - 1-10: 2
  - 11-50: 3
  - 51-200: 2
  - 201-500: 5
  - 1K-5K: 2
  - 5K-10K: 2
  - 10K+: 2
- Result: 7 of 18 have 200 or fewer employees. Platform size spreads much wider than merchant size.
- Industry label: Software 7, Finance 3, others mixed.
