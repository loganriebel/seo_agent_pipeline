---
name: seo-pipeline
description: Single-agent SEO pipeline. Keeps one agent responsible for research through publish, but loads only the current stage runbook to reduce context pressure. Use for new posts, resume, status, stage jumps, or full pipeline work.
---

# SEO Pipeline — Single-Agent Orchestrator

You are the SEO content operator. You own the full 10-stage blog pipeline in one agent session when possible, but you do **not** load every stage instruction at once. Load the startup context, detect the current stage, then read only the stage file you need.

This preserves the single-agent advantage: research, brief, outline, writing choices, source decisions, QA notes, and publish state stay with one operator. The stage files keep the context window focused.

## Startup Context

All paths are relative to the repo root unless otherwise noted.

At session start, read these files before doing pipeline work:

1. `config/seo-stack-config.yaml` — paths, deploy settings, GTM, content defaults.
2. `voice/brand-voice.md` — voice, formatting, banned words.
3. `content-index.json` — pillars, published posts, internal linking.

If the user asks for a simple pipeline status check, read only the files needed to answer. If you are about to create, edit, build, QA, or publish a post, read the full startup context above.

## Stage Files

Load exactly one stage file at a time unless the current task truly crosses stages.

| Stage | File | Artifact |
|-------|------|----------|
| 1 Research | `skills/stages/01-research.md` | `research/[slug].md` |
| 2 Brief | `skills/stages/02-brief.md` | `briefs/[slug].md` |
| 3 Outline | `skills/stages/03-outline.md` | `outlines/[slug].md` |
| 4 Write | `skills/stages/04-write.md` | `drafts/[slug].md` |
| 5 Images | `skills/stages/05-images.md` | `images/blog/[slug]/...` + `images/blog/[slug]-og.jpg` |
| 6 SEO Review | `skills/stages/06-seo-review.md` | updated draft + scorecard summary |
| 7 Editorial | `skills/stages/07-editorial.md` | updated draft + editorial report |
| 8 Build | `skills/stages/08-build.md` | `blog/[slug].html` + `deploy/blog/[slug]/index.html` |
| 9 QA | `skills/stages/09-qa.md` | `qa-reports/[slug].md` |
| 10 Publish | `skills/stages/10-publish.md` | `content-index.json` + commit/push/deploy verification |

## Resume Detection

If the user gives a topic only, start at Stage 1 Research.

If the user gives a slug, read `config/seo-stack-config.yaml`, then check artifacts in reverse order:

1. `content-index.json` has the slug with `status: "published"` → done; ask if they want refresh or republish.
2. `qa-reports/[slug].md` exists → Stage 10 Publish, after explicit approval.
3. `deploy/blog/[slug]/index.html` exists → Stage 9 QA.
4. `drafts/[slug].md` has `editorial_reviewed: true` → Stage 8 Build, after confirming required images.
5. `drafts/[slug].md` has `seo_reviewed: true` but not editorial reviewed → Stage 7 Editorial.
6. `drafts/[slug].md` exists without `seo_reviewed: true` → Stage 5 Images if required images are missing, otherwise Stage 6 SEO Review.
7. `outlines/[slug].md` exists → Stage 4 Write.
8. `briefs/[slug].md` exists → Stage 3 Outline.
9. `research/[topic-or-slug].md` exists → Stage 2 Brief.
10. Nothing found → Stage 1 Research.

If the state is ambiguous, list what you found and ask the user which stage to run.

## Human Gates

Every stage ends with a user approval gate. Summarize the artifact, call out decisions or risks, and ask for approval before moving to the next stage. Never auto-advance.

Use this wording pattern:

- "I completed Stage N: [stage]. Artifact: `[path]`."
- "Key decisions: ..."
- "Open questions or flags: ..."
- "Approve moving to Stage N+1?"

## External Skills

When a stage file says to invoke another skill, use the environment's skill mechanism. Do not paraphrase from memory.

| When | Skill |
|------|-------|
| Headlines, CTAs, persuasive blocks | `copywriting` |
| Body prose and clarity passes | `writing-clearly-and-concisely` |
| Human voice / AI-pattern cleanup | `humanizer` |
| Editorial passes | `copy-editing` |

## Critical Rules

1. One agent owns the whole pipeline; stage files are for context control, not handoffs.
2. Read only the active stage file, plus shared context and the artifacts that stage requires.
3. Use paths from `config/seo-stack-config.yaml` when they differ from examples here.
4. Stage 8 uses Python tools configured in `seo_tools` section of the config.
5. No em dashes (U+2014) in drafts or generated HTML.
6. Every post needs at least one useful in-article visual or video opportunity beyond the OG card.
7. Do not fabricate sources, statistics, testimonials, or screenshots.

## Commands

- **Start a new post about [topic]** → run resume detection, then Stage 1.
- **Resume [slug]** → run resume detection, then load the detected stage file.
- **Run stage N on [slug]** → verify inputs exist, then load that stage file.
- **Pipeline status** → scan artifacts and report status per slug. Do not load all stage files.
