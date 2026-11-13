# Adyen GTM Context OS

## Company
Adyen (adyen.com): to fill in after research.

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
- treg (CLI): paid data lookups. Check the price before every call (`treg catalog get <id>`). Always run with `TREG_CONFIG` pointing to `.treg/config.json` (set in `.claude/settings.local.json`).
- Miro (MCP, plugin `miro@miro-ai`): diagrams, such as GTM system maps (use `miro-gtm-flowchart`).
- Notion (MCP, `.mcp.json`): where we publish finished docs. GitHub stays the source of truth; edit here first, then publish.
