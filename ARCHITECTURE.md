# Architecture Decisions

- **Single-agent over multi-agent.** Eliminates context hand-off errors between stages. Research stats, brief angles, and outline structure stay with one operator when writing and reviewing.

- **Stage files over one giant skill.** `skills/seo-pipeline.md` is the entrypoint and orchestrator. It owns startup context, resume detection, human gates, and critical rules. The detailed runbooks live in `skills/stages/01-research.md` through `10-publish.md` and are loaded only when needed. This keeps the single-agent workflow while reducing context pressure.

- **SVG over PNG/JPG for AI-generated images.** SVGs render at any resolution, are version-controllable as plain text, don't require image optimization tooling, and can be created directly by Claude.

- **Git push as deploy.** Auto-deploy from a `deploy/` directory on every push to `master` (DigitalOcean App Platform, Netlify, Vercel, or equivalent). No separate upload step.

- **Human gate between every stage.** Prevents compounding errors. A bad brief becomes a bad outline becomes a bad post.

- **Config-driven paths and thresholds.** All domain, deploy, path, and content defaults live in `config/seo-stack-config.yaml`. Skill files contain no hardcoded values. This is the only file that changes when adapting the pipeline to a different site.

- **HTML output (not MDX) for static sites.** Stage 8 writes `blog/[slug].html` directly. If the site is built on Next.js or another framework, update Stage 8 to output MDX to `content/blog/`.

- **Subfolder convention for inline assets.** Hero and inline SVGs live under `images/blog/[slug]/[name].svg`. OG card lives at `images/blog/[slug]-og.jpg` (slug-level).

## Google Search Console (optional)

When GSC credentials are available, the pipeline uses them in two stages:

- **Stage 1 Research:** check existing rankings on the target keyword. If something ranks at positions 11–30, prefer consolidating or internal-linking over creating a competing post.
- **Stage 6 SEO Review:** pull impressions/clicks/CTR/position for internal link targets. High impressions + low CTR signals a title or description weakness, flagged for refresh.

Configure via `data_sources.google_search_console` in `config/seo-stack-config.yaml`. Store credentials outside the repo and add the path to `.gitignore`.
