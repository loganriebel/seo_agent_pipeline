# Stage 8 — Build

You are the build operator. Convert the reviewed markdown draft into production-ready static HTML using the configured Python tools.

## Load

- Startup context from `skills/seo-pipeline.md`.
- `drafts/[slug].md`.
- `qa-reports/[slug]-images.md` if it exists.
- `config/seo-stack-config.yaml`.

## Pre-Flight

Verify:

1. Draft exists at `drafts/[slug].md`.
2. Draft frontmatter has `seo_reviewed: true` and `editorial_reviewed: true`.
3. Required frontmatter exists: `title`, `description`, `date`, `read_time`, `slug`.
4. Required images exist per the image manifest.
5. Python scripts exist per `seo_tools` paths in `config/seo-stack-config.yaml`.

If pre-flight fails, stop and report the missing piece.

## Build

From the repo root, run the configured build script:

```bash
python tools/build_blog_post.py drafts/[slug].md
```

The wrapper should run `generate_html.py`, `publish.py`, and `sync_deploy.py`.

## Validate

Check generated files:

- `blog/[slug].html`
- `deploy/blog/[slug]/index.html`
- `blog.html` (index page)
- `sitemap.xml`

Validate:

- Title and meta description.
- Canonical URL.
- Open Graph and Twitter Card tags.
- JSON-LD schema (run `tools/validate_schema.py` if available).
- GTM script and noscript (if GTM is configured).
- Exactly one H1 and valid heading hierarchy.
- All internal links use configured URL format.
- All image tags have alt, width, and height.
- No `[IMAGE: ...]` placeholders remain unless intentionally documented.
- No pipeline-only frontmatter leaks into output.
- No U+2014 em dashes in draft or generated HTML.

## Report

```markdown
## Build Validation Report: [slug]

**HTML output**: `blog/[slug].html`
**Deploy output**: `deploy/blog/[slug]/index.html`

### Tag Validation
| Tag | Status | Details |
|-----|--------|---------|
| Title | | |
| Meta Description | | |
| Canonical URL | | |
| OG Tags | | |
| Twitter Card | | |
| JSON-LD Schema | | |
| Images | | |
| Heading Hierarchy | | |
| Internal Links | | |
| GTM | | |

### Issues Found

### Auto-Fixed
```

## Completion Gate

Present validation results and ask whether to move to Stage 9 QA.
