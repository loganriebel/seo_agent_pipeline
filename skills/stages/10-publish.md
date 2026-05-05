# Stage 10 — Publish

You are the release operator. Publish the approved blog post by updating content records, committing all artifacts together, pushing to the deploy branch, and verifying the live page.

## Load

- Startup context from `skills/seo-pipeline.md`.
- `qa-reports/[slug].md`.
- `drafts/[slug].md`.
- `briefs/[slug].md`.
- `content-index.json`.
- `config/seo-stack-config.yaml`.

## Pre-Deploy Checks

Verify:

1. QA report exists and status is `pass` or `pass-with-notes`.
2. User explicitly approved publishing.
3. Deploy artifact exists at `deploy/blog/[slug]/index.html`.
4. Blog HTML exists at `blog/[slug].html`.
5. Required image assets exist.
6. `blog.html` and `sitemap.xml` were updated by the build pipeline.

If anything is missing, stop and report it.

## Process

1. Present the post title, slug, target URL, QA status, and files expected in the commit. Ask for explicit go-ahead if not already given.
2. Update `content-index.json` with the post entry:

```json
{
  "slug": "[slug]",
  "title": "[title]",
  "primary_keyword": "[primary keyword]",
  "secondary_keywords": ["..."],
  "pillar": "[pillar]",
  "status": "published",
  "url": "/blog/[slug]/",
  "published_date": "YYYY-MM-DD",
  "internal_links_to": ["..."],
  "internal_links_from": []
}
```

3. Add the slug to the matching pillar's `spoke_slugs` array.
4. Commit from the repo root. Include site files and all pipeline artifacts in one commit.
5. Push to the configured deploy branch. Your deploy provider auto-deploys from `deploy/`.
6. After deployment, verify:
   - The live post URL returns the correct page.
   - The blog index page includes the post card.
   - `sitemap.xml` includes the URL.

## Commit Scope

Expected files typically include:

- `blog/[slug].html`
- `deploy/blog/[slug]/index.html`
- `images/blog/[slug]/`
- `images/blog/[slug]-og.jpg`
- `sitemap.xml`
- `blog.html`
- `drafts/[slug].md`
- `research/[slug].md`
- `briefs/[slug].md`
- `outlines/[slug].md`
- `qa-reports/[slug]*.md`
- `content-index.json`

## Report

```markdown
## Publish Report: [slug]

**Status**: SUCCESS / PARTIAL / FAILED

### Deployed
- **HTML**:
- **Images**:
- **Sitemap**:

### Content Index
- **Added to**:
- **Pillar**:
- **Internal links to**:

### Verification
- **Live page loads**:
- **Title matches**:
- **Images load**:

### Follow-up
- [ ] Check Google Search Console indexing in 1-2 days.
- [ ] Submit URL for indexing if needed.
- [ ] Update existing posts that should link to this post.
```

## Completion Gate

Present the publish report. The pipeline is complete when live verification passes.
