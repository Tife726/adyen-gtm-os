# Adyen: authorized copy facts
**Status: DRAFT, not yet approved.** Proposed 2026-11-13 by Claude from `context/docs/research/`. Nothing below may appear in copy until the "Approved by / date" column is filled in.

This is the ONLY source of names, numbers and claims for prospect-facing copy (`/campaign-copy` FACTS GATE). Anything not listed here renders as `{{placeholder}}`.

**Last updated:** 2026-11-13 (draft)

---

## Sender identity
| Field | Value |
|---|---|
| Sender name(s) | `{{sender_name}}`, **to confirm** |
| Title | **to confirm** |
| Signature format | **to confirm** (proposed: "First / Title / Adyen") |
| Sender persona notes | **to confirm** (proposed: first person singular, peer to payments leaders) |

## What we do (locked value prop)
Source: https://www.adyen.com/ (checked 2026-11-13, `research/voice.md`).
- **Variant A:** "Adyen is one platform for payments, data, and financial products." (verbatim self-description)
- **Variant B:** "Adyen runs online and in-person payments on one platform, with local acquiring in the markets you sell in." (built from `company.md` and `brand-stories.md`; needs approval as a phrasing)
- **Variant C (B1 platforms):** "Adyen for Platforms lets software companies offer payments under their own brand." (built from the homepage line "Launch payments and financial products under your brand")

## Approved claims and numbers (use these exact phrasings)
| # | Claim | Proposed phrasing | Source | Approved by / date |
|---|---|---|---|---|
| F1 | Retail consolidation | "Moose Knuckles went from at least four payment partners to one." | adyen.com/knowledge-hub/moose-knuckles-case-study (Adyen's wording) | pending |
| F2 | Retail consolidation (CFO voice) | AS Colour's CFO said Adyen let them "transition from multiple legacy providers to a single platform in record time." | adyen.com press release, 18 May 2026 | pending |
| F3 | Platform consolidation | Teamfront had "five or six integrated payment partners" and now manages "one relationship." | adyen.com/knowledge-hub/teamfront-chooses-adyen | pending |
| F4 | Streaming authorization | "DAZN increased authorization by 5.19%." | adyen.com/knowledge-hub/case-study-dazn (headline) | pending |
| F5 | Luxury authorization | "Prada Group saw a 9% authorization boost in Japan." | adyen.com/knowledge-hub/luxury-fashion-prada-customer-experience | pending |
| F6 | Marketplace funds flow | Vinted: "Unifying our funds flow on a single platform is a key driver for VintedPay's scalability." | adyen.com press release, 9 Apr 2026 | pending |
| F7 | Platform onboarding | ROLLER went "from onboarding customers in months to weeks." | adyen.com/knowledge-hub/roller-software-platform--payments-case-study | pending |

**Formatting rules for proof**
- Write company names exactly as above. Never use domains.
- Quotes stay verbatim. If a quote must be shortened, paraphrase it without quote marks.
- PSP names in `{{psp_list}}` come from job-post detection, so they are never stated as a complete list. Use "including" or "such as".

## Numbers to keep fuzzy (never quote as hard numbers)
- Pricing: interchange++ markup, per-transaction fees, minimum invoices (`company.md`).
- Adyen's company figures (revenue, volume, FTE). Not needed in outbound.
- Anything from the Forrester TEI study until its figures are added to research.

## Authorized social-proof names (proposed)
| Name | What it proves | Matches | Caveat |
|---|---|---|---|
| Moose Knuckles | PSP consolidation (4+ to 1) | A1 retail, cells 1 and 3 | Story is undated |
| AS Colour | Multi-provider to one platform, CFO voice | A1 retail, cells 1 and 2 (CFO) | 2026 press release |
| Teamfront | Platform consolidation (5-6 to 1) | B1 SaaS, cells 1 and 3 | Undated |
| DAZN | Authorization lift | A3 digital, cell 4 | Undated |
| Prada Group | Authorization lift in one market (Japan) | A1 luxury, cell 4 | Japan only. Do not generalise |
| Vinted | Single funds flow | B2 marketplaces, all cells | Quote is about VintedPay |
| ROLLER | Onboarding speed | B1 SaaS, cells 2 and 4 | Undated |

## Proof caveats
- Most knowledge-hub stories have no publish date. Never say "recently".
- Results are Adyen-published, so state them as the customer's result, never as a guarantee.
- F4 and F5 are authorization results, not cost results. Never imply savings from them.

## Banned
### Banned angles
- Disparaging any named competitor (Stripe, Worldpay and others). Name PSPs only as facts from `{{psp_list}}`.
- Regulation scare (PSD3, agentic rules). There are no fixed dates [market].
- Price, discount or "cheaper than" framing.
- Guarantees ("you will see X%").
- Anything that implies the new hire, or the current team, is failing (iron law 14 + the insult test).

### Banned words and phrasing (company-specific)
- "payment orchestration" as a description of Adyen (it is not Adyen's own term in research).
- "world-class", "best-in-class".

### Never in copy, under any circumstance
- Trustpilot scores, reviews or complaints.
- Internal research notes, treg costs, campaign metrics.

## Voice notes
- Register: direct, peer-to-peer, plain words. Adyen's own voice leans on certainty, control and "one platform" [voice].
- Hedges about the prospect: "my guess is", "could be off", "not sure if".
- Subjects: lowercase, 3-5 words, no variables, same subject across the thread.

## CTA ladder (proposed)
- **Primary:** a question about their setup ("How many payment providers are you running today?").
- **Secondary:** an offer of the relevant story ("Want the {{proof_customer}} write-up?").
- **Never:** calendar links or "book a call" in email 1.

## Internal only (never in prospect copy)
- Pricing, minimum volumes, ICP bands, signal names, PredictLeads, enrichment sources.
