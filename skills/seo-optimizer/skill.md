# SEO & Social Media Optimizer

Comprehensive SEO and social media optimization for maximum discoverability and engagement.

## Purpose

Maximize organic traffic from search engines and social platforms through optimized titles, descriptions, images, and strategic content distribution.

## Capabilities

### 1. SEO Optimization
- Generate SEO-friendly titles (50-60 chars)
- Write compelling meta descriptions (150-160 chars)
- Suggest target keywords
- Optimize URL slugs
- Create schema markup (Article, NewsArticle)
- Generate XML sitemap entries

### 2. Social Media Optimization
- Create Open Graph images (1200×630)
- Write platform-specific descriptions
- Suggest optimal hashtags
- Generate Twitter cards
- LinkedIn post optimization
- Facebook post optimization

### 3. Content Analysis
- Keyword density check
- Readability scoring
- Internal linking suggestions
- Image alt text optimization
- Header hierarchy check

### 4. Performance Tracking
- Search ranking monitoring
- Social engagement metrics
- Traffic source analysis
- Conversion tracking

### 5. Automated Posting
- Schedule social media posts
- Auto-post new articles
- Create thread variations
- A/B test different formats

## Usage

### Optimize Single Article
```bash
claude /seo-optimize --article-id=xxx
```

### Generate Social Content
```bash
claude /social-optimize --article-id=xxx --platforms=twitter,facebook
```

### Bulk SEO Audit
```bash
claude /seo-audit --date=last-30-days
```

### Create Social Campaign
```bash
claude /social-campaign --article-id=xxx --schedule=true
```

## SEO Title Generator

```javascript
function generateSEOTitle(article) {
  const variants = [
    // Question format
    `${extractQuestion(article)} | AI-ктуално`,

    // How-to format
    `Как ${extractAction(article)} - Пълен гид`,

    // Listicle format
    `${extractNumber(article)} начина за ${extractTopic(article)}`,

    // News format
    `${article.category}: ${article.title}`,

    // Keyword-optimized
    `${article.primaryKeyword} - ${article.summary.slice(0, 30)}...`
  ]

  // Select best variant (highest predicted CTR)
  return variants.map(v => ({
    title: v,
    score: predictCTR(v, article)
  })).sort((a, b) => b.score - a.score)[0]
}
```

## Meta Description Generator

```javascript
function generateMetaDescription(article) {
  const hook = extractHook(article.content)
  const cta = "Прочетете повече →"

  // Template: Hook + Summary + CTA
  let description = `${hook}. ${article.summary}`

  // Ensure 150-160 chars
  if (description.length > 160 - cta.length) {
    description = description.slice(0, 155 - cta.length) + '...'
  }

  return `${description} ${cta}`
}
```

## Open Graph Image Generator

```javascript
// Auto-generate OG images for articles
async function generateOGImage(article) {
  const canvas = createCanvas(1200, 630)
  const ctx = canvas.getContext('2d')

  // Background gradient
  const gradient = ctx.createLinearGradient(0, 0, 1200, 630)
  gradient.addColorStop(0, '#2563eb') // blue-600
  gradient.addColorStop(1, '#ea580c') // accent
  ctx.fillStyle = gradient
  ctx.fillRect(0, 0, 1200, 630)

  // Logo
  const logo = await loadImage('/logo.png')
  ctx.drawImage(logo, 50, 50, 150, 50)

  // Title
  ctx.fillStyle = '#ffffff'
  ctx.font = 'bold 60px Inter'
  ctx.textAlign = 'left'
  wrapText(ctx, article.title, 50, 200, 1100, 80)

  // Category badge
  ctx.fillStyle = 'rgba(255,255,255,0.2)'
  ctx.fillRect(50, 500, 200, 60)
  ctx.fillStyle = '#ffffff'
  ctx.font = '24px Inter'
  ctx.fillText(article.category.toUpperCase(), 70, 540)

  return canvas.toBuffer('image/png')
}
```

## Schema Markup

```json
{
  "@context": "https://schema.org",
  "@type": "NewsArticle",
  "headline": "{{article.title}}",
  "image": "{{article.imageUrl}}",
  "datePublished": "{{article.publishedAt}}",
  "dateModified": "{{article.updatedAt}}",
  "author": {
    "@type": "Organization",
    "name": "{{site.name}}",
    "url": "{{site.url}}"
  },
  "publisher": {
    "@type": "Organization",
    "name": "{{site.name}}",
    "logo": {
      "@type": "ImageObject",
      "url": "{{site.url}}/logo.png"
    }
  },
  "description": "{{article.summary}}",
  "mainEntityOfPage": {
    "@type": "WebPage",
    "@id": "{{site.url}}/{{article.slug}}"
  }
}
```

## Social Media Templates

### Twitter/X Thread
```
🧵 {THREAD}

1/5 {HOOK}
{Article intro}

2/5 {KEY_POINT_1}
{Expand on first point}

3/5 {KEY_POINT_2}
{Expand on second point}

4/5 {KEY_POINT_3}
{Expand on third point}

5/5 {CONCLUSION + CTA}
Прочетете пълната статия: {URL}

{HASHTAGS}
```

### LinkedIn Post
```
{PROFESSIONAL_HOOK}

{Article summary - 2-3 sentences}

🔑 Ключови точки:
• {Point 1}
• {Point 2}
• {Point 3}

{Call to action}
Прочетете повече: {URL}

#{Hashtag1} #{Hashtag2} #{Hashtag3}
```

### Facebook Post
```
{ENGAGING_HOOK} 🎯

{Article summary - conversational tone}

👉 {Call to action}

{URL}

{HASHTAGS}
```

## Hashtag Generator

```javascript
function generateHashtags(article) {
  const keywords = extractKeywords(article)
  const category = article.category
  const trending = getTrendingHashtags()

  const hashtags = [
    // Category-based
    `#${category}`,

    // Keyword-based
    ...keywords.slice(0, 3).map(kw => `#${kw.replace(/\s/g, '')}`),

    // Trending (if relevant)
    ...trending.filter(tag =>
      article.content.toLowerCase().includes(tag.toLowerCase())
    ),

    // Brand
    '#AIктуално'
  ]

  return hashtags.slice(0, 5) // Max 5 hashtags
}
```

## Automated Posting

```javascript
// Post to Twitter
async function postToTwitter(article) {
  const thread = generateTwitterThread(article)
  const media = await uploadMedia(article.imageUrl)

  let previousTweetId = null
  for (const tweet of thread) {
    const response = await twitter.post('statuses/update', {
      status: tweet,
      media_ids: previousTweetId ? null : [media.media_id_string],
      in_reply_to_status_id: previousTweetId
    })
    previousTweetId = response.id_str
  }
}

// Post to Facebook
async function postToFacebook(article) {
  const post = generateFacebookPost(article)

  await facebook.post('/me/feed', {
    message: post.text,
    link: article.url,
    picture: article.imageUrl
  })
}
```

## SEO Audit Report

```markdown
# SEO Audit Report - AI-ктуално

## Overall Score: 78/100

### ✅ Strengths
- Fast page load (1.2s)
- Mobile-friendly
- HTTPS enabled
- Structured data present

### ⚠️ Issues Found

#### Critical (Fix ASAP)
- 15 articles missing meta descriptions
- 8 articles with titles >60 chars
- No XML sitemap

#### Major
- 24 articles missing alt text on images
- H1 tag missing on 5 pages
- Duplicate meta descriptions (12 instances)

#### Minor
- Keyword density low on 8 articles
- Internal linking could be improved

### Recommendations

1. **Add meta descriptions** to all articles
2. **Generate XML sitemap** and submit to Google
3. **Optimize titles** to 50-60 characters
4. **Add alt text** to all images
5. **Improve internal linking** (suggest related articles)

### Traffic Potential
Estimated traffic increase: **+45%** with fixes applied
```

## Performance Dashboard

```tsx
<SEODashboard>
  <MetricCard
    title="Organic Traffic"
    value="12,453"
    change="+23.4%"
    period="Last 30 days"
  />

  <MetricCard
    title="Avg. Position"
    value="8.3"
    change="-1.2"
    period="Top 10 keywords"
  />

  <MetricCard
    title="Social Shares"
    value="3,456"
    change="+15.2%"
    period="Last 7 days"
  />

  <TopKeywords>
    <Keyword term="изкуствен интелект българия" position={3} traffic={892} />
    <Keyword term="ai новини" position={7} traffic={654} />
    <Keyword term="технологии българия" position={12} traffic={432} />
  </TopKeywords>
</SEODashboard>
```

## Success Metrics

- **Organic traffic**: +40% within 3 months
- **Search rankings**: Top 10 for 20+ keywords
- **Social engagement**: 500+ shares per week
- **CTR from search**: >5% average
- **Social CTR**: >3% average
