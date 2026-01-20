# Personalized Recommendation Engine

ML-powered content recommendation system that learns user preferences and suggests relevant articles.

## Purpose

Increase engagement by showing users content they're most likely to enjoy, based on their reading history and behavior patterns.

## Capabilities

### 1. User Profiling
- Track reading history (categories, topics, authors)
- Analyze engagement metrics (time spent, scroll depth)
- Identify interests and preferences
- Detect reading patterns (time of day, device, frequency)

### 2. Content-Based Filtering
- Analyze article features (category, keywords, entities)
- Calculate similarity between articles
- Match articles to user interests
- Consider recency and trending topics

### 3. Collaborative Filtering
- Find similar users based on reading patterns
- Recommend what similar users enjoyed
- Identify emerging trends in user behavior

### 4. Hybrid Recommendations
- Combine content-based + collaborative filtering
- Balance personalization with diversity
- Include serendipity factor (10% unexpected content)

### 5. Real-Time Updates
- Adapt to recent user actions
- Boost fresh content
- Re-rank based on current context

## Usage

### Get Recommendations for User
```bash
claude /recommend --user-id=xxx --limit=10
```

### Similar Articles
```bash
claude /recommend --article-id=xxx --similar=5
```

### Category Recommendations
```bash
claude /recommend --category=технологии --user-id=xxx
```

### Email Digest Recommendations
```bash
claude /recommend --user-id=xxx --format=email --period=daily
```

## Recommendation Algorithm

```javascript
function recommend(userId, limit = 10) {
  const user = getUserProfile(userId)

  // 1. Get candidate articles
  const candidates = getRecentArticles(days = 7)

  // 2. Score each article
  const scored = candidates.map(article => ({
    article,
    score: calculateScore(article, user)
  }))

  // 3. Sort by score
  scored.sort((a, b) => b.score - a.score)

  // 4. Apply diversity filter
  const diverse = applyDiversity(scored, user)

  // 5. Add serendipity
  const final = addSerendipity(diverse, 0.1)

  return final.slice(0, limit)
}

function calculateScore(article, user) {
  let score = 0

  // Category preference (40%)
  const categoryMatch = user.categoryPreferences[article.category] || 0
  score += categoryMatch * 40

  // Keyword similarity (30%)
  const keywordMatch = cosineSimilarity(
    article.keywords,
    user.topKeywords
  )
  score += keywordMatch * 30

  // Recency bonus (15%)
  const hoursOld = (Date.now() - article.publishedAt) / 3600000
  const recencyScore = Math.max(0, 15 - (hoursOld / 24))
  score += recencyScore

  // Quality (10%)
  score += (article.qualityScore / 10)

  // Popularity (5%)
  score += Math.min(5, article.viewCount / 1000)

  return score
}
```

## User Profile Model

```typescript
interface UserProfile {
  userId: string

  // Reading preferences
  categoryPreferences: {
    [category: string]: number // 0-1 score
  }

  topKeywords: string[]
  topAuthors: string[]

  // Behavioral patterns
  avgReadingTime: number // seconds
  avgScrollDepth: number // 0-100%
  preferredReadingTimes: number[] // hours of day

  // Engagement metrics
  articlesRead: number
  favoriteCategories: string[]
  dislikedCategories: string[]

  // Collaborative signals
  similarUsers: string[]
  clusterGroup: number

  // Recency
  lastActive: Date
  lastUpdated: Date
}
```

## Building User Profile

```javascript
async function buildUserProfile(userId) {
  // Get reading history
  const history = await getReadingHistory(userId, limit = 100)

  // Calculate category preferences
  const categoryCount = {}
  history.forEach(read => {
    categoryCount[read.category] =
      (categoryCount[read.category] || 0) + 1
  })

  const totalReads = history.length
  const categoryPreferences = {}
  Object.entries(categoryCount).forEach(([cat, count]) => {
    categoryPreferences[cat] = count / totalReads
  })

  // Extract top keywords
  const allKeywords = history.flatMap(h => h.article.keywords)
  const keywordFreq = countFrequency(allKeywords)
  const topKeywords = Object.entries(keywordFreq)
    .sort((a, b) => b[1] - a[1])
    .slice(0, 20)
    .map(([kw]) => kw)

  // Calculate engagement metrics
  const avgReadingTime =
    history.reduce((sum, h) => sum + h.timeSpent, 0) / history.length

  const avgScrollDepth =
    history.reduce((sum, h) => sum + h.scrollDepth, 0) / history.length

  // Detect reading time patterns
  const readingHours = history.map(h =>
    new Date(h.startedAt).getHours()
  )
  const preferredReadingTimes = getMostCommon(readingHours, 3)

  return {
    userId,
    categoryPreferences,
    topKeywords,
    avgReadingTime,
    avgScrollDepth,
    preferredReadingTimes,
    articlesRead: history.length,
    lastUpdated: new Date()
  }
}
```

## Collaborative Filtering

```javascript
// Find similar users
function findSimilarUsers(userId) {
  const user = getUserProfile(userId)
  const allUsers = getAllUserProfiles()

  const similarities = allUsers
    .filter(u => u.userId !== userId)
    .map(other => ({
      userId: other.userId,
      similarity: calculateUserSimilarity(user, other)
    }))
    .sort((a, b) => b.similarity - a.similarity)
    .slice(0, 10)

  return similarities
}

function calculateUserSimilarity(user1, user2) {
  // Cosine similarity of category preferences
  const catSim = cosineSimilarity(
    Object.values(user1.categoryPreferences),
    Object.values(user2.categoryPreferences)
  )

  // Jaccard similarity of keywords
  const kwSim = jaccardSimilarity(
    user1.topKeywords,
    user2.topKeywords
  )

  return (catSim * 0.7) + (kwSim * 0.3)
}
```

## Diversity & Serendipity

```javascript
function applyDiversity(articles, user, threshold = 0.7) {
  const diverse = []
  const categories = new Set()

  for (const item of articles) {
    // Ensure category diversity
    if (categories.size < 3 || !categories.has(item.article.category)) {
      diverse.push(item)
      categories.add(item.article.category)
    } else if (item.score > threshold) {
      // High scoring articles always included
      diverse.push(item)
    }

    if (diverse.length >= 10) break
  }

  return diverse
}

function addSerendipity(articles, ratio = 0.1) {
  const count = Math.floor(articles.length * ratio)

  // Replace bottom N items with random unexpected content
  const main = articles.slice(0, -count)
  const random = getRandomArticles(count, {
    excludeCategories: main.map(a => a.article.category)
  })

  return [...main, ...random]
}
```

## UI Components

### Recommendation Widget
```tsx
<RecommendationsWidget userId={userId}>
  <h3>Препоръчано за вас</h3>
  {recommendations.map(rec => (
    <ArticleCard
      key={rec.id}
      article={rec}
      reason={rec.reason}
    />
  ))}
</RecommendationsWidget>
```

### Personalized Homepage
```tsx
<HomePage userId={userId}>
  <Section title="За вас">
    <RecommendedArticles limit={5} />
  </Section>

  <Section title="Продължи четенето">
    <ContinueReading userId={userId} />
  </Section>

  <Section title="Подобни на {lastRead.title}">
    <SimilarArticles articleId={lastRead.id} />
  </Section>
</HomePage>
```

## Database Schema

```sql
CREATE TABLE user_profiles (
  id UUID PRIMARY KEY,
  user_id TEXT UNIQUE NOT NULL,
  category_preferences JSONB,
  top_keywords TEXT[],
  avg_reading_time INTEGER,
  avg_scroll_depth DECIMAL,
  preferred_reading_times INTEGER[],
  articles_read INTEGER DEFAULT 0,
  similar_users TEXT[],
  last_updated TIMESTAMP DEFAULT NOW()
);

CREATE TABLE recommendations (
  id UUID PRIMARY KEY,
  user_id TEXT NOT NULL,
  article_id UUID REFERENCES articles(id),
  score DECIMAL NOT NULL,
  reason TEXT,
  created_at TIMESTAMP DEFAULT NOW(),
  clicked BOOLEAN DEFAULT false,
  clicked_at TIMESTAMP
);

CREATE INDEX idx_recommendations_user ON recommendations(user_id, created_at DESC);
```

## API Endpoints

```typescript
// GET /api/recommendations?userId=xxx&limit=10
export async function GET(request) {
  const { userId, limit = 10 } = request.query

  const profile = await getUserProfile(userId)
  const recommendations = await generateRecommendations(profile, limit)

  return NextResponse.json({ recommendations })
}

// POST /api/recommendations/feedback
export async function POST(request) {
  const { userId, articleId, action } = await request.json()

  // Track user feedback
  await recordFeedback(userId, articleId, action)

  // Update user profile
  await updateUserProfile(userId)

  return NextResponse.json({ success: true })
}
```

## Success Metrics

- **Click-through rate**: % of recommended articles clicked
- **Diversity score**: Number of unique categories recommended
- **Session duration**: Time spent after seeing recommendations
- **Return rate**: % of users who come back
- **Engagement lift**: Increase in articles read per session
