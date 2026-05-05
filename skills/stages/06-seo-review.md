# Stage 6 — SEO Review

You are the SEO reviewer. Check discoverability mechanics and fix narrow technical issues that do not require major editorial rewriting.

## Load

- Startup context from `skills/seo-pipeline.md`.
- `drafts/[slug].md`.
- `briefs/[slug].md`.
- `content-index.json`.
- `config/seo-stack-config.yaml`.
- `voice/brand-voice.md`.

## Process

1. Extract the primary keyword, secondary keywords, target word count, internal link targets, and content parameters from the brief.
2. Check title, meta description, heading hierarchy, keyword placement, content length, internal links, external source links, structure, media placeholders, and frontmatter.
3. Treat secondary keywords as a natural-coverage check, not a rigid exact-match requirement. PASS if roughly 60% or more appear naturally, or if the missing terms would read awkwardly.
4. Search the draft for U+2014 em dashes and banned/soft-watch terms from the brand voice rules.
5. Auto-fix clear technical issues:
   - Missing or malformed frontmatter fields.
   - Meta description length when a small edit is enough.
   - Heading level mistakes.
   - Missing CTA link if the surrounding copy supports a simple insertion.
   - Obvious internal link formatting issues.
6. Do not auto-fix issues that require new substantive writing, changed positioning, or unsupported claims.
7. Update frontmatter when review passes or passes with non-blocking notes:

```yaml
seo_reviewed: true
seo_review_date: "YYYY-MM-DD"
```

## Scorecard

Report inline to the user unless a persistent scorecard is needed:

```markdown
## SEO Review Scorecard: [slug]

**Overall**: PASS / NEEDS FIXES / FAIL

### Results
| Category | Status | Issues |
|----------|--------|--------|
| Title Tag | | |
| Meta Description | | |
| Heading Hierarchy | | |
| Keyword Placement | | |
| Content Length | | |
| Internal Links | | |
| External Source Links | | |
| Content Structure | | |
| Frontmatter | | |
| Style QA | | |

### Stats
- **Word count**:
- **H2 count**:
- **Internal links**:
- **External source links**:
- **Primary keyword occurrences**:
- **U+2014 em dash count**:

### Auto-Fixed

### Requires Manual Fix

### Recommendations
```

## Completion Gate

Present the scorecard and ask whether to address manual fixes or move to Stage 7 Editorial.
