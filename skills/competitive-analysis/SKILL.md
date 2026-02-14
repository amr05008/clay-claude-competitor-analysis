---
name: competitive-analysis
description: Research a company or product and produce a structured competitive analysis report. Triggers on product teardown, competitive analysis, competitor research.
tools: WebSearch, WebFetch, Read, Write
argument-hint: <company name> [landing-page-url]
---

# Competitive Analysis Skill

Generate a structured competitive analysis report for a given company or product. Optionally analyze a specific landing page to interpret strategic direction.

## Usage

```
/competitive-analysis <company or product name>
/competitive-analysis <company name> <landing-page-url>
/competitive-analysis <landing-page-url>
```

## Workflow

Execute these phases in order. Use Clay MCP for structured data enrichment and WebSearch/WebFetch for qualitative research.

### Phase 0: Setup

1. Parse the argument to detect if a URL is present (token starting with `http`).
   - If both a company name and URL are provided: store the company name and set `landing_page_url` to the URL.
   - If only a URL is provided (no company name): set `landing_page_url`, then **WebFetch** the URL and extract the company name from the page content (title, branding, og:site_name, etc.).
   - If only a company name is provided: set `landing_page_url = null` and proceed as before.
2. Determine the company slug (lowercase, hyphenated) from the company name.
3. Set the output path: `./competitive-analysis/YYYY-MM-DD-<company-slug>.md`
4. Check if the `./competitive-analysis/` directory already exists. If not, create it.
5. Read the report template from `@./references/report-template.md`

### Phase 1: Clay Enrichment (Structured Data)

Fire two Clay MCP calls to gather structured data upfront:

**Company enrichment** — Use `find-and-enrich-company` with the company name and ALL of these data points:
- Company Competitors
- Company Customers
- Tech Stack
- Annual Revenue
- Revenue Model
- Latest Funding
- Investors
- Headcount Growth
- Open Jobs
- Recent News
- Website Traffic

**Leadership enrichment** — Use `find-and-enrich-contacts-at-company` for leadership roles (CEO, CTO, CPO, Founder) with the `Summarize Work History` data point.

Store all Clay results for use across later phases.

### Phase 1.5: Landing Page Analysis (conditional)

**Skip this phase if `landing_page_url` is null.**

**Goal:** Deeply analyze the landing page to understand what strategic direction it signals.

- **WebFetch** the `landing_page_url` to extract:
  - Page headline / hero copy
  - Target audience (who is this page speaking to?)
  - Product/service being offered (what specific capability?)
  - Positioning vs. their core product (extension? pivot? new vertical?)
  - Pricing or CTA details if present
  - Social proof / case studies / testimonials on the page
- **WebSearch** for the specific product or feature mentioned on the landing page to understand the market context (e.g., "[feature name] market", "[feature name] competitors").
- Store all findings for use in the Phase 9 synthesis.

### Phase 2: Product Discovery

**Goal:** What it is, who it's for, pricing.

- **WebSearch** for the company's official website, product pages, and pricing page.
- **WebFetch** the official site and pricing page to extract:
  - One-line value proposition
  - Core features list
  - Pricing tiers and structure
  - Target audience / ICP

### Phase 3: Jobs to Be Done

**Goal:** Why customers hire this product.

- **WebSearch** for reviews, Reddit discussions, Hacker News threads, and "switched from [competitor] to [product]" posts.
- **WebFetch** 2-3 of the most relevant results to extract:
  - Primary job (functional, emotional, social dimensions)
  - Secondary jobs
  - Underserved jobs / gaps
  - Switching triggers (what makes people leave competitors)

### Phase 4: Competitor Landscape

**Goal:** Direct, indirect, and adjacent competitors.

- Use Clay `Company Competitors` and `Company Customers` data from Phase 1.
- **WebSearch** for "[product] vs [competitor]" comparisons and "[product] alternatives".
- Categorize into: direct (3-5), indirect (2-3), adjacent (1-2).
- Note key differentiators for each.

### Phase 5: Tech Stack

**Goal:** What's under the hood.

- Use Clay `Tech Stack` data from Phase 1.
- **WebSearch** for engineering blog posts, open-source contributions, and tech talks.
- Organize by layer: frontend, backend, database, infrastructure, mobile, API.

### Phase 6: Business Model & Financials

**Goal:** Funding, revenue, business model.

- Use Clay `Annual Revenue`, `Revenue Model`, `Latest Funding`, `Investors` data from Phase 1.
- **WebSearch** for SEC filings, Crunchbase profiles, and pricing analysis articles.
- Document: model type, funding history, key investors, revenue estimates, valuation.

### Phase 7: Team & Organization

**Goal:** Headcount, leadership, hiring signals.

- Use Clay `Headcount Growth`, `Open Jobs` data from Phase 1.
- Use Clay leadership contacts and work histories from Phase 1.
- **WebSearch** for recent leadership changes or notable hires.
- Document: current headcount, growth trend, leadership table, hiring focus areas.

### Phase 8: Recent Activity

**Goal:** Last 12 months of signals.

- Use Clay `Recent News`, `Website Traffic` data from Phase 1.
- **WebSearch** for product launches, funding rounds, M&A activity, partnerships, press coverage, regulatory developments.
- Organize chronologically with dates.

### Phase 9: Synthesis & Report Generation

**Goal:** SWOT analysis, strategic direction analysis (if applicable), and final report.

1. Compile all findings into the report template.
2. Write a SWOT analysis based on everything gathered.
3. Write an overall strategic position summary.
4. **If `landing_page_url` is set:** Write the "Strategic Direction Analysis" section (Section 9 in the template) synthesizing:
   - What the landing page signals about the company's direction
   - How this new direction relates to their core product
   - Market opportunity being targeted
   - Competitive implications (who are they going after? who should be worried?)
   - Assessment: is this a complement, pivot, or expansion?
5. **If `landing_page_url` is null:** Omit Section 9 entirely.
6. Compile a numbered sources list of all URLs referenced.
7. Write the final report to `./competitive-analysis/YYYY-MM-DD-<company-slug>.md`.

## Quality Rules

- **Cite sources**: Every factual claim must reference a source (numbered, listed at end).
- **Date all figures**: Revenue, headcount, funding — always include the date or timeframe.
- **Fact vs. inference**: Clearly distinguish verified data from educated guesses. Use phrases like "estimated", "likely", "based on [source]".
- **Flag low confidence**: If a section has sparse data, note it explicitly (e.g., "Limited public data available").
- **No hallucination**: If data isn't found, say so. Never fabricate numbers or claims.

## Output

Final report written to: `./competitive-analysis/YYYY-MM-DD-<company-slug>.md`
