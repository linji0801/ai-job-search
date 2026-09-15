# Search Queries for Job Scraper

<!-- SETUP: Customize these queries based on your skills, target roles, and location -->

## Installed portal CLIs (primary for `/scrape`)

`/scrape` discovers every portal skill under `.agents/skills/*/SKILL.md` and runs its CLI first. Shipped country-agnostic CLIs include `linkedin-search` and `freehire-search`; Danish demos and any skill you add with `/add-portal` are included the same way. You do **not** need a matching `site:` line below for those CLIs to run.

The `site:` query templates in this file are the **WebSearch fallback** — for portals without a CLI, company career pages, or when a CLI fails.

**Language scope:** write every query category in every language listed in your CLAUDE.md Languages table (typically 1-2, sometimes more). A posting requiring a language you have *not* declared, as a job condition, is excluded before scoring; a posting requiring a *higher level* than you declared in a language you *do* work in is flagged for your own judgment, not excluded — see `04-job-evaluation.md`'s Language Gate, the single source of truth for this rule. Translate each category's keywords rather than machine-translating word-for-word (e.g. "Frontend Developer" -> "Desarrollador Frontend", not a literal word-for-word translation) if you work in more than one language.

## Search Sites

Primary (your market's job boards - scaffold one with `/add-portal`):
- **indeed.com** - largest general US job board
- **linkedin.com/jobs** - LinkedIn job listings (filter: United States / Los Angeles); also covered by `linkedin-search` CLI
- **builtin.com** - niche/industry tech board with strong LA/remote filters (optional)
- **glassdoor.com** - another major US job board (optional)

Secondary (company career pages via Google):
- Direct Google searches with `site:` filters for target companies: Airbnb, Netflix, Anthropic, OpenAI, Nvidia (see Target Companies below)

## Target Companies

Companies to specifically monitor for openings, in addition to the general search categories below:
- Airbnb
- Netflix
- Anthropic
- OpenAI
- Nvidia

## Query Categories

Queries are grouped by priority. Write **each category in every language from your Languages table** (see Language scope above). Combine each query with your location terms (e.g. your city, region, or metro area) where the site supports it.

**Organize by function, not job title.** The same underlying work carries different titles across companies and markets (a "Data Scientist" role at one employer may be posted as "Insights Analyst" or "Data Consultant" at another). Name each priority category after the function it covers, and list several plausible job titles as query variants within that category rather than betting an entire priority tier on one exact title string.

### Priority 1: Senior Software Engineer / Backend & Distributed Systems

These match your strongest and most desired career direction.

```
site:indeed.com "Senior Software Engineer" (Los Angeles OR Remote)
site:indeed.com "Senior Software Development Engineer" (Los Angeles OR Remote)
site:indeed.com "Staff Software Engineer" distributed systems (Los Angeles OR Remote)
site:linkedin.com/jobs "Senior Software Engineer" "distributed systems" United States
```

### Priority 2: GenAI / ML Platform Engineering

These match your domain expertise.

```
site:indeed.com "GenAI Engineer" (Los Angeles OR Remote)
site:indeed.com "ML Platform Engineer" (Los Angeles OR Remote)
site:indeed.com LLM AWS backend engineer United States
site:linkedin.com/jobs "ML Platform Engineer" OR "GenAI Engineer" United States
```

### Priority 3: Applied Scientist (Engineering) / Technical Lead

Adjacent roles you could pivot into.

```
site:indeed.com "Applied Scientist" "distributed systems" (Los Angeles OR Remote)
site:indeed.com "Technical Lead" GenAI (Los Angeles OR Remote)
site:linkedin.com/jobs "Applied Scientist" AWS United States
```

### Priority 4: Broader Backend / Data Infrastructure

Wider net for general technical roles.

```
site:indeed.com backend engineer "data lake" OR "streaming" (Los Angeles OR Remote)
site:linkedin.com/jobs "backend engineer" AWS United States
site:builtin.com backend engineer distributed systems Los Angeles
```

**Key skill terms to combine into the above queries as needed:** Backend, Distributed Systems, Relational Database, NoSQL Database, ElastiCache, ElasticSearch, AWS, GenAI, LLM, Data-driven Architecture, GraphQL, Data Lake, Streaming Processing

## Location Filter

When evaluating results, verify the job location is within reasonable commute distance from home (Brea, CA), or fully remote (US). Define acceptable areas:
- Remote (United States) - PASS
- Brea, CA and surrounding Orange County / Los Angeles / Southern California areas within ~50 miles - PASS
- On-site/hybrid role requiring presence more than ~50 miles from Brea, CA - FAIL (deal-breaker, see CLAUDE.md Deal-breakers)
- Requires relocation outside Southern California - FAIL (deal-breaker)

## Language Filter

Your working languages and levels are in CLAUDE.md's Languages table. When filtering scraped results, apply `04-job-evaluation.md`'s Language Gate: a posting requiring a language you haven't declared at all is excluded; a posting requiring a higher level than you declared in a language you do work in is not excluded, flag it clearly instead (see `job-scraper/SKILL.md`'s Step 3 "Quick Fit Assessment" for how the flag surfaces in `/scrape` output). Postings simply *written* in a language you don't work in, that don't require it on the job, are fine.

## Date Filter

Only include jobs posted within the last 14 days, or with an application deadline that has not yet passed. If a posting date cannot be determined, include it but flag as "date unknown".

## Adapting Queries

If the user specifies a focus area, select queries from the matching category and also generate 2-3 custom queries for that focus. For example:
- "/scrape [focus_area]" -> relevant category queries + custom focus-specific queries
