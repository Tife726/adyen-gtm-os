# Adyen GTM Context OS

## Company
Adyen (adyen.com): publicly listed (Euronext Amsterdam: ADYEN) financial technology platform: "One platform for payments, data, and financial products." Sells payments online and in person, platform payments (Adyen for Platforms), embedded finance, issuing, risk, and Intelligent Money Movement to enterprises and platforms. Q3 2026: €738.1M net revenue (+23%), €451.1B processed volume. 4,771 FTE (end 2025). Sources: `context/docs/research/company.md`.


## File index
Every tracked file, grouped by folder.

**Root**
- `CLAUDE.md`: this file. `README.md`: repo intro.
- `.gitkeep` files hold empty folders: `context/brand-kit/`, `context/docs/research/`, `context/icp/`, `context/messaging/`, `context/positioning/`, `knowledge-base/learnings/`, `knowledge-base/playbooks/`, `meetings/`, `output/campaigns/`, `tools/`, `.claude/skills/`, `tools/rex/registry/schemas/`.
- `.mcp.json`: Notion MCP server. `.claude/settings.json`: Miro plugin. `.gitignore`.

**context/icp/**
- `icp.md`: five-layer ICP (data-backed firmographic and technographic layers), not-a-fit, open questions.
- `personas.md`: champion (Head of Payments), economic buyer (CFO or Treasurer), influencer (CTO or CIO).
- `signal-matrix.md`: 6 segments x 3 personas with angles, 11 scored signals, top 5, workflow tracker.

**context/messaging/**
- `facts.md`: the ONLY source of names, numbers and claims for copy. Approved 2026-11-13. Sender identity still to be supplied.

**context/docs/**
- `gtm-system.md`: end-to-end outbound system v0.2 (Claude Code, Clay, HubSpot, n8n, PlusVibe, GitHub), with a link to the Miro board.
- `research/company.md`: products, pricing, financials, team, leadership.
- `research/customers.md`: named customers and verbatim testimonials.
- `research/competitors.md`: competitor positioning (incl. Fiserv, Nexi, Rapyd, dLocal in their own words).
- `research/market.md`: regulation and market changes (PSD3/PSR, agentic commerce).
- `research/signals.md`: buying-signal evidence and the PSP detection test.
- `research/voice.md`: Adyen's own headlines, taglines and product names.
- `research/brand-stories.md`: 54 Adyen customer stories (metrics, quotes, switch triggers).
- `research/firmographics.md`: enrichment of 54 named customers (size, revenue, HQ, stack).

**context/positioning/, context/brand-kit/**: empty (placeholders only).

**output/campaigns/**
- `campaign-strategy.md`: 19 ranked campaign ideas.
- `payment-talent-signal/workflow-spec.md`: payments-hiring workflow. Structure and copy approved.
- `saas-payments-launch/workflow-spec.md`: SaaS payments-launch workflow. Structure, E1 and E2 approved; E3 and DMs awaiting approval.

**diagrams/**
- `adyen-gtm-system.json`: flowchart spec. `adyen-gtm-system.svg`: the board source. `adyen-gtm-system.png`: local preview. Board: https://miro.com/app/board/uXjVEOU-iKg=/

**knowledge-base/playbooks/**
- `signal-matrix-framework.md`, `workflow-anatomy.md`: targeting method and Mother Intent Catalog.
- `copy-iron-laws.md`, `copy-frameworks.md`, `linkedin-dm-frameworks.md`, `copy-bans.yaml`: copy rules.
- `templates/facts-template.md`: template for facts.md.
- `clay/`: `actions-data-credits.md`, `expert.md`, `formulas.md`, `iron-laws.md`, `prompts.md`.

**knowledge-base/learnings/, meetings/**: empty.

**tools/**
- `agent-browser/README.md`, `crawl4ai/README.md`: tool notes.
- `rex/`: Rex v2.1 (unofficial Clay API client). Start at `tools/rex/README.md`. Every file:
  - top level: `.gitignore`, `CHANGELOG.md`, `COOKIE_SETUP.md`, `GETTING_STARTED.md`, `LEARNINGS.md`, `LICENSE`, `README.md`, `action-registry.md`, `clay-api-reference.md`, `clay-session.json.example`, `clay_browser.py`, `clay_client.py`, `clay_costs.py`, `clay_flow.py`, `clay_public.py`, `clay_workspace_costs.py`, `requirements.txt`, `rex-config.example.json`
  - `claylens/`: `README.md`, `analytics.html`, `analytics.js`, `background.js`, `content.js`, `manifest.json`, `styles.css`
  - `claylens/icons/`: `icon-128.png`, `icon-16.png`, `icon-48.png`
  - `claylens/lib/`: `aggregator.js`, `clay-api.js`, `column-classifier.js`, `cost-extractor.js`, `model-prices.js`, `provider-costs.js`
  - `claylens/vendor/`: `chart.umd.min.js`
  - `registry/`: `INDEX.md`
  - `registry/schemas/`: `.gitkeep`
  - `templates/`: `waterfall-work-email.json`
  - `tests/`: `test_client_offline.py`, `test_export_schema_offline.py`, `test_local_additions_offline.py`, `test_workspace_costs_offline.py`

**.claude/skills/** (18 skills, every file):
- `agent-browser/`: `SKILL.md`
- `build-clay/`: `SKILL.md`, `references/build-session-goal.md`, `references/column-playbook.md`, `references/technical-recipes.md`
- `build-workflow-spec/`: `SKILL.md`
- `calculate-costs/`: `SKILL.md`
- `campaign-contract/`: `SKILL.md`, `references/contract-standard.md`, `references/example-contract.md`
- `campaign-copy/`: `SKILL.md`
- `campaign-qa/`: `SKILL.md`
- `campaign-strategy/`: `SKILL.md`
- `crawl4ai/`: `SKILL.md`
- `design-clay-table-json/`: `SKILL.md`, `references/example.flow.json`, `references/flow-blueprint.schema.json`
- `export-tables/`: `SKILL.md`
- `import-clay-table/`: `SKILL.md`, `scripts/clay_table.py`, `scripts/import_flow.py`
- `inbox-placement-test/`: `LICENSE`, `SKILL.md`, `config.example.json`, `reference/sop.md`, `scripts/0_token_audit.py`, `scripts/1_build.py`, `scripts/2_load_seeds.py`, `scripts/3_arm_tags.py`, `scripts/4_qa_preflight.py`, `scripts/5_launch.py`, `scripts/6_poll.py`, `scripts/7_results.py`, `scripts/8_write_sheet.py`, `scripts/bison_api.py`, `scripts/discover_pools.py`, `weekly/README.md`, `weekly/discover.py`
- `miro-gtm-flowchart/`: `SKILL.md`, `examples/reliance-hmo.json`, `scripts/build_flowchart.py`
- `prompt-builder/`: `SKILL.md`, `references/decision-to-prompt.md`, `references/pipeline-prompts.md`
- `save-clay-schema/`: `SKILL.md`, `scripts/export_table_schema.py`
- `signal-matrix/`: `SKILL.md`
- `workspace-cost-audit/`: `SKILL.md`

**weekly-priorities/**: `2026-W46.md`. **work-log/**: `2026-W46.md`.

## How this folder works
- `.claude/skills/`: reusable Claude skills for repeat GTM tasks. Read when running a workflow that has a skill.
- `context/icp/`: ideal customer profiles and buyer personas. Read before any targeting, list building or copy.
- `context/positioning/`: positioning, differentiators and competitors. Read before writing any messaging or copy.
- `context/messaging/`: approved messaging frameworks and value props. Read before writing any outbound or campaign copy.
- `context/brand-kit/`: voice, tone, style and visual rules. Read before producing any customer-facing asset.
- `context/docs/research/`: raw research with sources. Read when you need to verify or cite a company fact.
- `knowledge-base/playbooks/`: step-by-step GTM playbooks. Read before executing a known motion.
- `knowledge-base/learnings/`: what worked, what failed and why. Read before planning new campaigns.
- `meetings/`: meeting notes and transcripts. Read when you need decisions or context from past conversations.
- `output/campaigns/`: finished campaign assets and copy. Read when reusing or iterating on past work.
- `tools/`: scripts and tool configs. Read when a task needs automation or data.
- `weekly-priorities/`: one file per ISO week with priorities. Read at the start of every session.
- `work-log/`: one file per ISO week logging every change. Read at session start, append after every change.

## Rules
- Cite a source for every company fact.
- Never invent numbers.
- No em dashes in any copy.
- Ask before sending or publishing anything.
- Log every change in this week's work log (`work-log/YYYY-Www.md`).

## Skills (`.claude/skills/`)
- Strategy: `signal-matrix`, `campaign-strategy`, `build-workflow-spec`, `campaign-copy`, `prompt-builder`
- Clay build: `campaign-contract`, `design-clay-table-json`, `build-clay`, `import-clay-table`, `save-clay-schema`, `export-tables`
- QA and cost: `campaign-qa`, `calculate-costs`, `workspace-cost-audit`, `inbox-placement-test`
- Research and mapping: `crawl4ai`, `agent-browser`, `miro-gtm-flowchart`

## Tools
Status as of 2026-11-13. "Connected" means it works from this repo today.

**Connected**
- **Claude Code:** builds and runs everything here (skills, research, specs, copy, reviews).
- **GitHub** (`git`, repo `adyen-gtm-os`): the source of truth. Commit and push after approved changes.
- **treg** (CLI): paid data lookups. Check the price before every call (`treg catalog get <id>`). Always run with `TREG_CONFIG` pointing to `.treg/config.json` (set in `.claude/settings.local.json`). Budget used this week: about $0.96. Endpoints used:
  - `harvestapi.linkedin.search.jobs` ($0.001/call): job-post signals.
  - `companyenrich.companies.enrich` ($0.0098): firmographics.
  - `predictleads.companies.technology_detections` ($0.04): PSPs in job posts.
  - `crawl4ai.trustpilot.business.reviews` ($0.00135): reviews.
  - `dataforseo.x.domain-analytics-technologies-domain-technologies-live` ($0.012): tested and not usable for PSPs.
- **Miro** (MCP, plugin `miro@miro-ai`): diagrams. Board: https://miro.com/app/board/uXjVEOU-iKg=/ (use `miro-gtm-flowchart`).
- **Web search and web fetch:** free research, used first.
- **Python 3:** scripts in skills and `tools/rex/`. Pillow is only needed for flowchart previews (install it in a virtualenv).

**Configured, not yet authenticated**
- **Notion** (MCP, `.mcp.json`): where finished docs are published. GitHub stays the source of truth; edit here first, then publish. Needs sign-in via `/mcp` before use.

**In the GTM system design, not yet connected to this repo** (see `context/docs/gtm-system.md`)
- **Clay:** companies, enrichment, ICP fit, routing, people, email waterfall and validation. Accessed through Rex (`tools/rex/`, needs a Clay session cookie) and the Clay skills.
- **HubSpot:** CRM, suppression list, LinkedIn task steps, reporting.
- **n8n:** the scheduler and the pipes (weekly signal pulls, PlusVibe to HubSpot syncs, reply classification, weekly metrics).
- **PlusVibe:** email sending, sending metrics, the reply inbox.

**In the kit, not used yet**
- **crawl4ai** (`tools/crawl4ai/`, skill `crawl4ai`): local crawling to Markdown.
- **agent-browser** (`tools/agent-browser/`, skill `agent-browser`): browser automation.
- **EmailBison and MailReach:** referenced only by the `inbox-placement-test` skill. Not part of this stack.
