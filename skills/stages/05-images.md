# Stage 5 — Images

You are the image asset operator. Create or document the post's hero, inline visual assets, and OG card.

## Load

- Startup context from `skills/seo-pipeline.md`.
- `outlines/[slug].md`.
- `config/seo-stack-config.yaml`.
- Existing files under `images/blog/` for the slug.

## Process

1. Read the outline's Image Summary.
2. Confirm every post has at least one useful in-article visual or video opportunity beyond the OG card.
3. Create SVG assets when practical. Use the subfolder convention: `images/blog/[slug]/[descriptive-name].svg`.
4. If a stock photo is better, provide 2–3 free/licensed options and wait for user choice before using one.
5. If a screenshot is needed, write exact capture instructions in the manifest.
6. If a short walkthrough video would be more useful, add it as a video opportunity. Do not create video unless the user asks.
7. Write descriptive alt text and optional captions.
8. Generate the OG card when possible:

```bash
python tools/generate_og_assets.py [slug]
```

This should produce `images/blog/[slug]-og.jpg`.

## Output

Save `qa-reports/[slug]-images.md`:

```markdown
# Image Manifest: [slug]

## Images

### 1. [Image name]
- **File**:
- **Type**: SVG / stock / screenshot / video note
- **Section**:
- **Alt text**:
- **Caption**:
- **Dimensions**:
- **Status**: complete / needs-manual-capture / needs-user-choice

## Stock Photo Options

## Manual Captures Needed

## Video Opportunities

## OG Card
- **File**: `images/blog/[slug]-og.jpg`
- **Status**:
```

## Completion Gate

Summarize completed assets, manual captures, stock choices, video opportunities, and OG status. Ask whether to move to Stage 6 SEO Review.
