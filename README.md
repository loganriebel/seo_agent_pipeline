# SEO Content Pipeline

**Disclaimer: This repo is a nerfed version of what I use to build SEO ready blog posts on my websites. Think of this repo as a way to understand the basic process and pipeline. This agent becomes significantly more powerful when you add business context, GSC MCP, GA4 MCP, CRM outcomes, competitor data, SEM Rush KW tracking, and detailed descriptions of your ICP and brand.**

A 10-stage SEO content pipeline built on Claude Code skills. One agent owns the full lifecycle, research through live deploy. The most important part of the agent is that it uses the seo-pipeline.md as a thin orchestrator and then has 10 separate 'stage' skills so that once we move onto each stage, the agent doesn't have to keep everything in context and can just use the previous stage's artifact to continue the build. There is also a human-in-the-loop approval between every stage to ensure quality at each stage. If you just blindly run each stage, you'll get crap in and crap out.

**Used to publish 30+ blog posts, but here are 4 that performed partically well. All 4 are ranking on the first page of the SERP with 6,900+ impressions in the last 30 days across competitive paid media queries.**

| Post | Impressions (30d) | Avg Position |
|---|---|---|
| [Facebook Ads Cost Benchmarks](https://makometrics.com/blog/facebook-ads-cost-benchmarks) | 2,978 | 10.1 |
| [Meta Ad Library Guide](https://makometrics.com/blog/meta-ad-library-guide) | 1,969 | 8.5 |
| [7 Signs of Facebook Ads Creative Fatigue](https://makometrics.com/blog/7-signs-facebook-ads-creative-fatigue) | 1,696 | 12.4 |
| [Facebook Ads Not Converting](https://makometrics.com/blog/facebook-ads-not-converting) | 272 | 6.6 |

---

## What it does

The pipeline takes a seed topic and produces a published, schema-validated blog post. Each stage writes a file the next stage consumes. The agent can be interrupted at any stage and resumes by detecting which artifacts already exist.

```
Seed topic
    │
    ▼
01 Research ──► research/[slug].md
    │
    ▼
02 Brief ──────► briefs/[slug].md
    │
    ▼
03 Outline ────► outlines/[slug].md
    │
    ▼
04 Write ──────► drafts/[slug].md
    │
    ▼
05 Images ─────► images/blog/[slug]/ + [slug]-og.jpg
    │
    ▼
06 SEO Review ─► updates draft + scorecard
    │
    ▼
07 Editorial ──► updates draft + editorial report
    │
    ▼
08 Build ──────► blog/[slug].html + deploy/blog/[slug]/index.html
    │
    ▼
09 QA ─────────► qa-reports/[slug].md
    │
    ▼
10 Publish ────► content-index.json + git push → live
```

---

## Design decisions

**Single agent, not multi-agent.** One agent owns research through publish. Keyword decisions made in Stage 1 are still in context during Stage 7 editorial, no handoff errors. The skills are modularized with artifacts after each step so the agent only pulls what's needed into context.

**Stage files for context control, not handoffs.** The orchestrator (`skills/seo-pipeline.md`) handles startup, resume logic, and human gates. It loads only the active stage's runbook on demand. This keeps one agent in control while keeping the context window focused and not bloated.

**Artifact chaining enables resume-anywhere.** The agent detects the current stage by checking which artifacts exist (in reverse order). Drop in mid-pipeline, restart after a break, or jump to a specific stage by name.

**Human gate at every transition.** Each stage ends with a summary and explicit approval request before advancing. A bad brief doesn't silently become a bad 2,000-word draft. This ensures quality for each post.

**Config-driven paths and thresholds.** All site details, deploy targets, content defaults, and tool paths live in `config/seo-stack-config.yaml`. The skill files contain no hardcoded domain or deploy logic.

---

## Stack

- **Claude Code skills** — orchestrator + 10 stage runbooks
- **Python 3.10+** — Jinja2 HTML build, JSON-LD schema validation
- **Static site deploy** — git push triggers deploy (DigitalOcean App Platform, Netlify, Vercel, or equivalent)
- **Google Search Console** — optional; used in Stage 1 (keyword sizing) and Stage 6 (existing page performance)

---

## How to adapt it

1. **Fork and open in Claude Code** (agent mode required)
2. **Fill in `config/seo-stack-config.yaml`** — domain, deploy provider, GTM, author, brand voice topics
3. **Customize `voice/brand-voice.md`** — audience, tone, banned phrases, formatting standards
4. **Run `/seo-pipeline`** — the agent reads startup context, detects stage, and begins

---

## File map

```
seo_agent_pipeline/
├── README.md
├── ARCHITECTURE.md          ← design decisions and tradeoffs
├── config/
│   └── seo-stack-config.example.yaml
├── skills/
│   ├── seo-pipeline.md      ← single-agent orchestrator (start here)
│   └── stages/
│       ├── 01-research.md
│       ├── 02-brief.md
│       ├── 03-outline.md
│       ├── 04-write.md
│       ├── 05-images.md
│       ├── 06-seo-review.md
│       ├── 07-editorial.md
│       ├── 08-build.md
│       ├── 09-qa.md
│       └── 10-publish.md
├── voice/
│   └── brand-voice.md       ← audience, tone, formatting, banned phrases
└── tools/
    └── validate_schema.py   ← JSON-LD Article schema validator
```

Runtime artifact directories (`research/`, `briefs/`, `outlines/`, `drafts/`, `qa-reports/`) are created by the pipeline and excluded from the repo via `.gitignore`.

---

## Requirements

- Claude Code with agent mode enabled
- Python 3.10+ (for Stage 8 build tools and schema validation)
- A static site with git-push deploy (or adapt Stage 8/10 for your stack)

## Build Your Own!

Use this repo to get a step by step walkthrough to setup your own AI SEO agent https://github.com/loganriebel/seo-content-stack
