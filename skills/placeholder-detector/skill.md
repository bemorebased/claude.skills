---
name: detect-placeholders
description: Detect Google News, RSS, and other placeholder images in articles
---

# Placeholder Image Detector Skill

Identifies articles with Google News placeholder images, RSS feed icons, tracking pixels, and other non-article images that should be replaced with real images.

## Usage

```bash
# Scan recent articles for placeholder images
/detect-placeholders --last=1000

# Scan specific article by ID
/detect-placeholders --article-id=xxx

# Generate detailed report with suggestions
/detect-placeholders --report --format=csv

# Show only Google News placeholders
/detect-placeholders --filter=google-news --last=500

# Generate replacement suggestions
/detect-placeholders --suggest-replacements
```

## What it Detects

**Google News Placeholders:**
- lh3.googleusercontent.com, lh*.googleusercontent.com
- ggpht.com (Google Photos hosting)
- blogger.googleusercontent.com
- news.google.com domains
- Google News logos, icons, favicons

**Generic RSS/Feed Placeholders:**
- RSS feed icons and logos
- Feed reader placeholders
- Default images
- Generic news/article icons

**RSS Reader Placeholders:**
- Feedly logos
- Feedburner images
- Flipboard logos
- Pocket images
- Instapaper thumbnails

**Social Media Placeholders:**
- Facebook default share images
- Twitter card default images
- Open Graph default images

**Tracking Pixels:**
- 1x1 pixel images (PNG, JPG, GIF)
- Tracking images
- Data URI placeholders (SVG, GIF)

**Small Images (likely icons):**
- Dimensions ≤256px in URL (16x16, 32x32, 64x64, 128x128, 256x256)
- Size parameters ≤256px (?size=128, ?w=64, etc.)
- Filename dimensions ≤256px (image_150x150.jpg)

**Other Indicators:**
- Very short Google URLs (<100 chars)
- URLs with icon/logo/favicon query parameters
- Filenames containing "placeholder", "default", "icon", "logo", "favicon"

## Example Output

```
Placeholder Image Report
========================
Scanned: 1000 articles
Placeholders Found: 187 articles (18.7%)

Breakdown:
- Google News: 94 (50.3%)
- RSS Icons: 38 (20.3%)
- Small Images: 32 (17.1%)
- Tracking Pixels: 15 (8.0%)
- No Image: 8 (4.3%)

Top Issues:
1. lh3.googleusercontent.com (78 articles)
2. news.google.com (16 articles)
3. RSS feed icons (38 articles)

Suggestions:
- Fetch images from original article sources
- Use category-specific default images
- Re-run extraction with higher timeout

Run with --report to export full CSV
```

## Replacement Strategies

1. **Fetch from Source**: Extract image from original article URL
2. **Category Defaults**: Use category-specific default images
3. **Content Search**: Find images embedded in article content
4. **External Services**: Use Unsplash or similar for dynamic defaults

## Implementation

Uses `/Users/sheshe/aiktualno/lib/utils/placeholderImageDetector.ts`

The PlaceholderImageDetector class extends the logic from `services/orchestrator/pipeline.ts` isPlaceholderImage() method (lines 804-896) with database scanning and reporting capabilities.

## Related Scripts

- `scripts/fix-placeholder-images.ts` - Cleanup script for fixing existing placeholders
- `scripts/batch-refetch-images.ts` - Batch re-fetch images from sources
