# Trending Topics Predictor

AI-powered system for predicting emerging trends and scheduling content for optimal engagement.

## Purpose

Stay ahead of the news cycle by identifying trending topics before they peak, scheduling synthesis at optimal times, and discovering underreported stories.

## Capabilities

### 1. Trend Detection
- Analyze topic velocity (mentions over time)
- Track social media signals
- Monitor search trends
- Identify emerging patterns
- Predict topic lifecycle

### 2. Timing Optimization
- Determine peak engagement times
- Schedule synthesis for maximum impact
- Predict when stories will break
- Optimize publishing schedule

### 3. Gap Analysis
- Find underreported stories
- Identify unique angles
- Discover niche topics
- Spot competitor blind spots

### 4. Breaking News Detection
- Real-time news monitoring
- Urgency scoring
- Auto-trigger synthesis
- Alert system for major events

### 5. Content Calendar
- Suggest topics for next 7 days
- Plan editorial calendar
- Balance content categories
- Predict seasonal trends

## Usage

### Predict Trending Topics
```bash
claude /trending-forecast --horizon=24h
claude /trending-forecast --horizon=7days
```

### Schedule Optimal Synthesis
```bash
claude /schedule-synthesis --topic="AI технологии" --optimal=true
```

### Find Content Gaps
```bash
claude /content-gaps --category=технологии
```

### Breaking News Alert
```bash
claude /breaking-news --threshold=high
```

### Generate Editorial Calendar
```bash
claude /editorial-calendar --week
```

## Trend Detection Algorithm

```javascript
async function detectTrendingTopics(horizon = '24h') {
  // 1. Collect signals
  const signals = await collectSignals(horizon)

  // 2. Extract topics
  const topics = extractTopics(signals)

  // 3. Calculate velocity
  const trending = topics.map(topic => ({
    topic: topic.name,
    velocity: calculateVelocity(topic),
    volume: topic.mentions,
    sentiment: analyzeSentiment(topic),
    prediction: predictPeak(topic),
    score: calculateTrendScore(topic)
  }))

  // 4. Sort by trend score
  trending.sort((a, b) => b.score - a.score)

  return trending.slice(0, 20)
}

function calculateVelocity(topic) {
  const now = topic.current.mentions
  const before = topic.previous.mentions

  // Percent change
  const change = ((now - before) / before) * 100

  // Acceleration (change of change)
  const prevChange = topic.previous.velocity || 0
  const acceleration = change - prevChange

  return {
    change, // % change
    acceleration, // how fast it's accelerating
    direction: change > 0 ? 'up' : 'down'
  }
}

function predictPeak(topic) {
  const history = topic.history // last 7 days

  // Fit curve to historical data
  const curve = fitLogisticCurve(history)

  // Predict when it will peak
  const peakTime = curve.inflectionPoint

  // Predict peak volume
  const peakVolume = curve.maxValue

  return {
    expectedPeakAt: peakTime,
    expectedVolume: peakVolume,
    confidence: curve.rSquared,
    currentPhase: getCurrentPhase(topic, curve)
  }
}
```

## Signal Sources

### Google Trends
```javascript
async function getGoogleTrends(keyword) {
  const trends = await googleTrends.interestOverTime({
    keyword,
    startTime: new Date(Date.now() - 7 * 24 * 60 * 60 * 1000),
    geo: 'BG'
  })

  return trends.map(t => ({
    date: t.time,
    interest: t.value // 0-100
  }))
}
```

### Social Media Signals
```javascript
async function getSocialSignals(topic) {
  const [twitter, facebook, reddit] = await Promise.all([
    getTwitterMentions(topic),
    getFacebookEngagement(topic),
    getRedditPosts(topic)
  ])

  return {
    twitter: {
      mentions: twitter.count,
      engagement: twitter.likes + twitter.retweets,
      sentiment: analyzeSentiment(twitter.tweets)
    },
    facebook: {
      shares: facebook.shareCount,
      comments: facebook.commentCount
    },
    reddit: {
      posts: reddit.count,
      upvotes: reddit.score
    },
    overall: calculateOverallSignal(twitter, facebook, reddit)
  }
}
```

### News Velocity
```javascript
async function getNewsVelocity(topic) {
  const articles = await searchNews(topic, { hours: 24 })

  // Group by hour
  const byHour = groupBy(articles, a =>
    Math.floor(a.publishedAt / 3600000)
  )

  // Calculate mentions per hour
  const velocity = Object.entries(byHour).map(([hour, articles]) => ({
    hour: new Date(hour * 3600000),
    count: articles.length,
    sources: unique(articles.map(a => a.source))
  }))

  return velocity
}
```

## Trend Scoring

```javascript
function calculateTrendScore(topic) {
  let score = 0

  // Velocity (40%)
  score += Math.min(40, topic.velocity.change)

  // Acceleration (20%)
  score += Math.min(20, topic.velocity.acceleration)

  // Volume (20%)
  const volumeScore = Math.log10(topic.volume) * 4
  score += Math.min(20, volumeScore)

  // Freshness (10%)
  const hoursOld = (Date.now() - topic.firstSeen) / 3600000
  const freshnessScore = Math.max(0, 10 - hoursOld)
  score += freshnessScore

  // Source diversity (10%)
  const diversityScore = Math.min(10, topic.sources.length)
  score += diversityScore

  return score
}
```

## Timing Optimizer

```javascript
async function getOptimalPublishTime(topic) {
  // Predict when topic will peak
  const prediction = await predictPeak(topic)

  // Consider audience activity
  const audienceActivity = getAudienceActivity()

  // Find best time (before peak, during high activity)
  const optimalTime = findOptimalTime(
    prediction.expectedPeakAt,
    audienceActivity
  )

  return {
    recommended: optimalTime,
    reasoning: generateReasoning(optimalTime, prediction),
    alternatives: findAlternatives(optimalTime, 3)
  }
}

function findOptimalTime(peakTime, audienceActivity) {
  // Publish 4-6 hours before predicted peak
  const targetRange = {
    start: new Date(peakTime - 6 * 3600000),
    end: new Date(peakTime - 4 * 3600000)
  }

  // Find highest activity time in range
  const bestSlot = audienceActivity
    .filter(slot =>
      slot.time >= targetRange.start &&
      slot.time <= targetRange.end
    )
    .sort((a, b) => b.activity - a.activity)[0]

  return bestSlot?.time || targetRange.start
}
```

## Content Gap Finder

```javascript
async function findContentGaps(category) {
  // Get topics covered by competitors
  const competitorTopics = await getCompetitorTopics(category)

  // Get trending topics in category
  const trendingTopics = await getTrendingTopics(category)

  // Find gaps (trending but not covered)
  const gaps = trendingTopics.filter(topic =>
    !competitorTopics.some(comp =>
      similarity(topic, comp) > 0.7
    )
  )

  // Score gaps by opportunity
  const opportunities = gaps.map(gap => ({
    topic: gap,
    opportunity: calculateOpportunityScore(gap),
    competition: gap.competitors.length,
    difficulty: calculateDifficulty(gap)
  }))

  return opportunities.sort((a, b) =>
    b.opportunity - a.opportunity
  )
}

function calculateOpportunityScore(topic) {
  let score = 0

  // High search volume
  score += Math.log10(topic.searchVolume) * 10

  // Low competition
  score += (100 - topic.competitors.length) * 0.2

  // Rising trend
  score += topic.velocity.change

  // Freshness
  const daysOld = (Date.now() - topic.firstSeen) / 86400000
  score += Math.max(0, 20 - daysOld)

  return score
}
```

## Breaking News Detection

```javascript
async function detectBreakingNews() {
  const signals = await collectRealtimeSignals()

  const breaking = signals.filter(signal =>
    signal.velocity.change > 1000 && // 10x growth
    signal.velocity.acceleration > 500 && // Rapid acceleration
    signal.age < 2 * 3600000 // Less than 2 hours old
  )

  // Classify urgency
  const classified = breaking.map(item => ({
    topic: item.topic,
    urgency: classifyUrgency(item),
    impact: estimateImpact(item),
    sources: item.sources,
    recommendation: getRecommendation(item)
  }))

  return classified.sort((a, b) => b.urgency - a.urgency)
}

function classifyUrgency(signal) {
  let urgency = 0

  // Velocity
  urgency += Math.min(40, signal.velocity.change / 25)

  // Acceleration
  urgency += Math.min(30, signal.velocity.acceleration / 20)

  // Source credibility
  const avgCredibility = signal.sources.reduce(
    (sum, s) => sum + s.credibility, 0
  ) / signal.sources.length
  urgency += avgCredibility * 0.2

  // Social engagement
  urgency += Math.min(10, signal.social.engagement / 10000)

  return urgency
}
```

## Editorial Calendar

```json
{
  "week": "2025-01-20 to 2025-01-26",
  "recommendations": [
    {
      "date": "2025-01-20",
      "day": "Monday",
      "topics": [
        {
          "topic": "AI законодателство ЕС",
          "reason": "Predicted to peak Tuesday",
          "priority": "high",
          "suggestedTime": "08:00",
          "expectedEngagement": "high"
        },
        {
          "topic": "Технологични инвестиции",
          "reason": "Monday readers prefer business news",
          "priority": "medium",
          "suggestedTime": "12:00"
        }
      ]
    },
    {
      "date": "2025-01-21",
      "day": "Tuesday",
      "topics": [
        {
          "topic": "Киберсигурност",
          "reason": "Rising trend, low competition",
          "priority": "high",
          "suggestedTime": "10:00"
        }
      ]
    }
  ],
  "gaps": [
    "Климатични промени в България",
    "Дигитална трансформация"
  ],
  "trending": [
    {
      "topic": "Изкуствен интелект",
      "velocity": "+234%",
      "peakExpected": "2025-01-22T14:00:00Z"
    }
  ]
}
```

## Dashboard

```tsx
<TrendingDashboard>
  <LiveTrends>
    <TrendCard
      topic="AI в медицината"
      velocity="+156%"
      peak="in 6 hours"
      action={<Button>Синтезирай сега</Button>}
    />
    <TrendCard
      topic="Дигитална валута"
      velocity="+89%"
      peak="in 12 hours"
      status="scheduled"
    />
  </LiveTrends>

  <UpcomingSchedule>
    <ScheduledItem
      topic="Технологии"
      time="Today 14:00"
      reason="Peak expected at 18:00"
    />
    <ScheduledItem
      topic="Политика"
      time="Tomorrow 08:00"
      reason="High morning engagement"
    />
  </UpcomingSchedule>

  <ContentGaps>
    <Gap
      topic="Климатични промени"
      opportunity="High"
      competition="Low"
    />
  </ContentGaps>
</TrendingDashboard>
```

## Success Metrics

- **Prediction accuracy**: >75% of trends identified before peak
- **Timing optimization**: +30% engagement from optimal scheduling
- **Gap discovery**: 10+ unique stories per week
- **Breaking news**: <30min from detection to synthesis
- **Traffic increase**: +50% from trending content
