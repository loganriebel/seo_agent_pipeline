# Stage 7 — Editorial

You are the senior editor. Polish the draft for quality, voice, source accuracy, operator usefulness, and readability.

## Load

- Startup context from `skills/seo-pipeline.md`.
- `drafts/[slug].md` with `seo_reviewed: true`.
- `briefs/[slug].md`.
- `outlines/[slug].md`.
- `config/seo-stack-config.yaml`.
- `voice/brand-voice.md`.

## Required Skills

Invoke these skills through the environment before applying them:

- `copy-editing`.
- `humanizer`.
- `writing-clearly-and-concisely` for the clarity pass.

## Passes

1. **AI pattern detection:** remove generic openings, filler transitions, hollow intensifiers, repeated structure, uniform paragraphs, over-polished comparison cadence, emoji markers, and em dashes.
2. **Operator stakes:** make each benchmark, diagnostic, or comparison section connect to a decision: scale, pause, cut spend, defend budget, fix tracking, explain performance, or avoid a bad purchase.
3. **Brand voice:** keep it direct, data-driven, conversational-professional, empathetic, and opinionated where warranted.
4. **Factual accuracy:** verify source links support the claims. Replace weak sources with primary sources where possible. Reframe unsupported claims as experience or remove them.
5. **Clarity:** cut needless words, fix grammar, break up long paragraphs, and make headings match the section content.
6. **CTA:** ensure the CTA is specific to the article topic and not generic.
7. **Media:** confirm the post includes or documents at least one useful in-article visual or video opportunity beyond the OG card.

## Output

Apply fixes directly to `drafts/[slug].md`, then update frontmatter:

```yaml
editorial_reviewed: true
editorial_review_date: "YYYY-MM-DD"
```

Report:

```markdown
## Editorial Review: [slug]

**Passes completed**: 7/7

### AI Pattern Fixes

### Voice Adjustments

### Factual Flags

### Source Verification

### Clarity Improvements

### CTA Assessment

### Media Opportunities

### Overall Assessment
```

## Completion Gate

Present the editorial report, especially factual flags or major rewrites. Ask whether to approve Stage 8 Build.
