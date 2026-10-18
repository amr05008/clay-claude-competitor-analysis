# Clay Competitor Analysis Plugin

> **Retired, October 2026.** I'm no longer maintaining this plugin. Its successor is [Competitive Analysis](https://aaronroy.com/competitive-analysis/), an evidence-graded teardown plugin ([source](https://github.com/amr05008/competitive-analysis)). This repo stays up, read-only, for reference.

A Claude Code plugin that generates structured competitive analysis reports, powered by [Clay](https://www.clay.com/) enrichment data.

Run `/competitive-analysis Acme Corp` and get a full report covering product overview, JTBD analysis, competitor landscape, tech stack, financials, team, recent activity, and strategic assessment — all backed by real data.

## Prerequisites

- [Claude Code](https://docs.anthropic.com/en/docs/claude-code) CLI installed
- [Clay MCP server](https://www.clay.com/) configured in Claude Code (used for structured company/contact enrichment)

## Install

```bash
claude plugin install aaronroy/clay-claude-competitor-analysis
```

## Usage

```
/competitive-analysis <company name>
/competitive-analysis <company name> <landing-page-url>
/competitive-analysis <landing-page-url>
```

### Examples

```
/competitive-analysis Linear
/competitive-analysis Notion https://notion.so/product/ai
/competitive-analysis https://vercel.com/ai
```

When a landing page URL is provided, the report includes an additional **Strategic Direction Analysis** section interpreting what the page signals about the company's trajectory.

## Output

Reports are written to `./competitive-analysis/YYYY-MM-DD-<company-slug>.md` in your current working directory.

## What's in a report

1. **Product Overview** — value prop, features, pricing
2. **Jobs to Be Done** — why customers hire this product
3. **Competitor Landscape** — direct, indirect, and adjacent competitors
4. **Tech Stack** — frontend through infrastructure
5. **Business Model & Financials** — funding, revenue, investors
6. **Team & Organization** — headcount, leadership, hiring signals
7. **Recent Activity** — last 12 months of product launches, funding, partnerships, press
8. **Strategic Assessment** — SWOT analysis and overall position
9. **Strategic Direction Analysis** — landing page interpretation (when URL provided)

## Data sources

- **Clay MCP** — company enrichment (competitors, customers, tech stack, revenue, funding, headcount, jobs, news, traffic) and leadership contact enrichment
- **Web research** — product pages, pricing, reviews, Reddit/HN discussions, engineering blogs, press coverage
