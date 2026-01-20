# Newsletter Generator

Automated newsletter creation and distribution system for AI-ктуално subscribers.

## Purpose

Generate engaging, personalized email newsletters that keep subscribers informed and drive traffic back to the platform.

## Capabilities

### 1. Content Curation
- Select top articles from time period (daily/weekly)
- Personalize based on user reading history
- Balance categories and topics
- Include trending stories
- Feature synthesized articles

### 2. Newsletter Formats

**Daily Digest**
- Top 5 stories from last 24 hours
- Quick reading time: 3-5 minutes
- Mobile-optimized layout

**Weekly Roundup**
- Best 10-15 articles from week
- Category-based sections
- Editor's picks
- Trending topics summary

**Breaking News Alert**
- Single important story
- Real-time trigger
- High-priority delivery

**Personalized Feed**
- Based on user interests
- Machine learning recommendations
- Engagement optimization

### 3. Email Design
- Responsive HTML templates
- Dark mode support
- One-click reading
- Social sharing buttons
- Unsubscribe management

### 4. A/B Testing
- Subject line variants
- Content order testing
- Send time optimization
- Template design tests

### 5. Analytics
- Open rates
- Click-through rates
- Article engagement
- Conversion tracking
- Subscriber growth

## Usage

### Generate Daily Newsletter
```bash
claude /newsletter --type=daily --schedule=8am
```

### Generate Weekly Roundup
```bash
claude /newsletter --type=weekly --schedule=sunday-6pm
```

### Personalized Newsletter
```bash
claude /newsletter --type=personal --user-id=xxx
```

### Breaking News Alert
```bash
claude /newsletter --type=breaking --article-id=xxx
```

## Template Structure

```html
<!DOCTYPE html>
<html>
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <title>{{newsletterTitle}}</title>
</head>
<body>
  <!-- Header -->
  <header>
    <img src="logo.png" alt="AI-ктуално">
    <h1>{{greeting}}, {{userName}}!</h1>
    <p>{{subtitle}}</p>
  </header>

  <!-- Featured Story -->
  <section class="featured">
    <img src="{{article.image}}">
    <h2>{{article.title}}</h2>
    <p>{{article.summary}}</p>
    <a href="{{article.url}}">Прочети повече →</a>
  </section>

  <!-- Top Stories -->
  <section class="stories">
    <h3>Топ новини</h3>
    {{#each articles}}
    <article>
      <h4>{{title}}</h4>
      <p>{{summary}}</p>
      <a href="{{url}}">Прочети →</a>
    </article>
    {{/each}}
  </section>

  <!-- Footer -->
  <footer>
    <a href="{{unsubscribe}}">Отпиши се</a>
    <a href="{{preferences}}">Настройки</a>
  </footer>
</body>
</html>
```

## Personalization Engine

### User Segmentation
- Active readers (daily visits)
- Casual readers (weekly visits)
- Category enthusiasts (tech, politics, etc.)
- New subscribers (<30 days)
- Inactive users (>14 days no visit)

### Content Matching
```javascript
// Score each article for user
function scoreArticle(article, user) {
  let score = 0

  // Category preference
  if (user.topCategories.includes(article.category)) {
    score += 30
  }

  // Reading history similarity
  score += cosineSimilarity(article, user.readArticles) * 20

  // Recency
  const hoursOld = (Date.now() - article.publishedAt) / 3600000
  score += Math.max(0, 20 - hoursOld) // Max 20 points for freshness

  // Quality
  score += article.qualityScore * 0.3

  return score
}
```

## Subject Line Generator

Uses AI to create compelling subject lines:

```bash
# Input
topic: "Изкуствен интелект в България"
articles: [...]

# Output variations
[
  "🤖 AI в България: Новата технологична революция",
  "Изкуственият интелект променя бизнеса - как?",
  "3 начина AI да промени вашия бизнес (2025)",
  "BREAKING: България инвестира €50М в AI"
]
```

## Scheduling System

```javascript
// Optimal send times by user segment
const SEND_SCHEDULES = {
  'morning-readers': '08:00',    // Before work
  'lunch-readers': '12:30',      // Lunch break
  'evening-readers': '18:00',    // After work
  'weekend-readers': 'Sat 10:00' // Weekend morning
}

// Timezone handling
function getOptimalSendTime(user) {
  const userSegment = determineSegment(user)
  const localTime = convertToUserTimezone(
    SEND_SCHEDULES[userSegment],
    user.timezone
  )
  return localTime
}
```

## Analytics Dashboard

Track newsletter performance:

```markdown
📊 Newsletter Analytics

**Daily Digest - Jan 22, 2025**
- Sent: 10,523
- Opened: 4,734 (45.0%) ⬆️ +3.2%
- Clicked: 1,892 (18.0%) ⬆️ +1.5%
- Articles read: 3,456
- Avg engagement: 2.3 min

**Top Performing Articles**
1. "AI в медицината" - 892 clicks
2. "Нови данъчни промени" - 654 clicks
3. "Технологични новини" - 543 clicks

**Best Subject Line**
"🔥 Днес: AI революция в България" - 52% open rate
```

## Integration

### Email Service Provider
```typescript
// services/email/sendgrid.ts
import sendgrid from '@sendgrid/mail'

export async function sendNewsletter(
  template: string,
  subscribers: string[],
  subject: string
) {
  await sendgrid.send({
    to: subscribers,
    from: 'newsletter@bAIpress.bg',
    subject,
    html: template,
    trackingSettings: {
      clickTracking: { enable: true },
      openTracking: { enable: true }
    }
  })
}
```

### Subscription Management
```sql
-- Add to schema
CREATE TABLE newsletter_subscriptions (
  id UUID PRIMARY KEY,
  email TEXT UNIQUE NOT NULL,
  user_id UUID REFERENCES users,
  frequency TEXT DEFAULT 'daily', -- daily, weekly, breaking
  categories TEXT[], -- preferred categories
  active BOOLEAN DEFAULT true,
  confirmed BOOLEAN DEFAULT false,
  subscribed_at TIMESTAMP DEFAULT NOW(),
  last_sent_at TIMESTAMP
);
```

## Success Metrics

- **Open rate > 40%** (industry avg: 25%)
- **Click rate > 15%** (industry avg: 3-5%)
- **Unsubscribe < 0.5%** per send
- **Forward rate > 5%**
- **Traffic from newsletter > 20%** total traffic
