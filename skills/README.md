# AI-ктуално Skills

Advanced AI-powered tools to elevate your news platform to the next level.

## 📚 Available Skills

### 1. 🎨 **Design Review Expert**
**Path:** `/design-review`
**Purpose:** Comprehensive UI/UX audits and design consistency fixes

**Usage:**
```bash
# Full platform audit
claude /design-review

# Specific page review
claude /design-review --page=leaderboard

# Accessibility focus
claude /design-review --focus=accessibility
```

**When to use:**
- Before major releases
- After adding new features
- When users report UI issues
- Monthly maintenance checks

---

### 2. ✅ **Content Quality Analyzer**
**Path:** `/content-quality`
**Purpose:** Prevent low-quality articles from being published

**Usage:**
```bash
# Analyze article before publishing
claude /quality-check --article-id=xxx

# Set quality gate threshold
claude /quality-gate --threshold=70

# Bulk analysis
claude /quality-check --last=10
```

**Quality Scoring:**
- 90-100: Excellent ✅
- 70-89: Good ⚠️
- 50-69: Fair 🔴
- 0-49: Poor ❌

**When to use:**
- After every synthesis
- Before publishing articles
- During editorial review

---

### 3. 📧 **Newsletter Generator**
**Path:** `/newsletter-generator`
**Purpose:** Automated newsletter creation and distribution

**Usage:**
```bash
# Daily digest
claude /newsletter --type=daily --schedule=8am

# Weekly roundup
claude /newsletter --type=weekly

# Breaking news alert
claude /newsletter --type=breaking --article-id=xxx
```

**Formats:**
- Daily Digest (5 top stories)
- Weekly Roundup (10-15 articles)
- Breaking News Alerts
- Personalized Feeds

**When to use:**
- Daily at optimal send time
- Weekly on Sundays
- When breaking news occurs

---

### 4. 🔍 **Multi-Source Fact Checker**
**Path:** `/fact-checker`
**Purpose:** Verify claims and build trust

**Usage:**
```bash
# Check all facts in article
claude /fact-check --article-id=xxx

# Verify specific claim
claude /fact-check --claim="България инвестира €50М в AI"

# Batch verification
claude /fact-check --date=today
```

**Confidence Levels:**
- Verified (90-100%): Multiple sources agree
- Likely (70-89%): Most sources agree
- Disputed (30-69%): Sources contradict
- Unverified (<30%): No corroboration

**When to use:**
- During synthesis
- Before publishing
- When claims seem questionable

---

### 5. 🎯 **Personalized Recommender**
**Path:** `/recommender`
**Purpose:** Increase engagement with smart recommendations

**Usage:**
```bash
# Get recommendations for user
claude /recommend --user-id=xxx --limit=10

# Similar articles
claude /recommend --article-id=xxx --similar=5

# Email digest recommendations
claude /recommend --user-id=xxx --format=email
```

**Recommendation Types:**
- Content-based filtering
- Collaborative filtering
- Hybrid approach
- Serendipity factor (10%)

**When to use:**
- On homepage
- After reading article
- In email newsletters
- On profile page

---

### 6. 🚀 **SEO & Social Optimizer**
**Path:** `/seo-optimizer`
**Purpose:** Maximize discoverability and engagement

**Usage:**
```bash
# Optimize for SEO
claude /seo-optimize --article-id=xxx

# Generate social content
claude /social-optimize --article-id=xxx --platforms=twitter,facebook

# Full SEO audit
claude /seo-audit --date=last-30-days
```

**Optimizations:**
- SEO titles & descriptions
- Open Graph images
- Schema markup
- Social media posts
- Hashtag suggestions

**When to use:**
- Before publishing articles
- Monthly SEO audits
- When launching campaigns

---

### 7. 📈 **Trending Predictor**
**Path:** `/trending-predictor`
**Purpose:** Stay ahead of the news cycle

**Usage:**
```bash
# Predict trends
claude /trending-forecast --horizon=24h

# Schedule optimal synthesis
claude /schedule-synthesis --topic="AI технологии"

# Find content gaps
claude /content-gaps --category=технологии

# Breaking news detection
claude /breaking-news --threshold=high
```

**Predictions:**
- Topic velocity tracking
- Peak time predictions
- Content gap analysis
- Breaking news alerts

**When to use:**
- Daily editorial planning
- Real-time monitoring
- Weekly calendar creation

---

## 🎯 Quick Start Guide

### Immediate Impact (Week 1)

1. **Design Review**
   ```bash
   claude /design-review --scope=full
   ```
   Fix critical UI issues immediately

2. **Content Quality**
   ```bash
   claude /quality-gate --threshold=70
   ```
   Block low-quality articles

3. **SEO Optimization**
   ```bash
   claude /seo-audit
   ```
   Quick SEO wins

### Growing Engagement (Week 2-4)

4. **Newsletter Launch**
   ```bash
   claude /newsletter --type=daily --schedule=8am
   ```
   Start building subscriber base

5. **Recommendations**
   ```bash
   claude /recommend --user-id=xxx
   ```
   Personalize user experience

### Advanced Features (Month 2+)

6. **Fact Checking**
   ```bash
   claude /fact-check --article-id=xxx
   ```
   Build trust with verified content

7. **Trend Prediction**
   ```bash
   claude /trending-forecast
   ```
   Stay ahead of competitors

---

## 🔄 Integration Workflow

### Publishing Pipeline
```
1. Synthesis → 2. Quality Check → 3. Fact Check → 4. SEO Optimize → 5. Publish → 6. Social Share
```

### Daily Workflow
```
Morning:
  - Check trending topics
  - Review quality gate failures
  - Run design audit (weekly)

Afternoon:
  - Synthesize trending articles
  - Fact-check claims
  - Optimize for SEO

Evening:
  - Send daily newsletter
  - Generate social content
  - Review analytics
```

---

## 📊 Success Metrics

### Content Quality
- Average quality score: 85+
- Rejection rate: <10%
- User engagement: +30%

### Traffic & Growth
- Organic traffic: +40%
- Newsletter subscribers: 10,000+
- Social engagement: +50%

### Trust & Credibility
- Fact-check coverage: 90%+
- User trust score: 8/10+
- Return visitor rate: 60%+

---

## 🛠️ Skill Development Roadmap

### Phase 1: Foundation (Complete ✅)
- Design Review Expert
- Content Quality Analyzer
- SEO Optimizer

### Phase 2: Engagement (Current)
- Newsletter Generator
- Recommender Engine
- Trending Predictor

### Phase 3: Trust (Next)
- Fact Checker
- Source Credibility Tracker
- User Feedback Loop

### Phase 4: Advanced (Future)
- ML-powered predictions
- Real-time personalization
- Automated A/B testing
- Competitor intelligence

---

## 💡 Best Practices

### Design Reviews
- Run before major releases
- Focus on user-reported issues
- Check mobile responsiveness
- Test dark mode thoroughly

### Quality Checks
- Never publish below 70 score
- Review failed articles manually
- Improve synthesis prompts
- Track quality trends

### Newsletters
- A/B test subject lines
- Personalize by segment
- Track engagement metrics
- Optimize send times

### SEO
- Update monthly
- Focus on quick wins first
- Build internal linking
- Monitor rankings

### Recommendations
- Update user profiles daily
- Balance personalization vs diversity
- Track click-through rates
- A/B test algorithms

### Fact Checking
- Verify controversial claims
- Link to sources
- Update when new info emerges
- Be transparent about confidence

### Trending
- Check hourly for breaking news
- Schedule synthesis strategically
- Find unique angles
- Balance trending vs evergreen

---

## 🚨 Common Issues & Solutions

### "Quality score always low"
- Increase minimum sources to 5+
- Improve Cerebras prompts
- Filter duplicate sources
- Review synthesis parameters

### "Recommendations not personalized"
- Ensure user tracking is active
- Build up reading history (>10 articles)
- Check profile building logic
- Verify similarity calculations

### "SEO audit shows many issues"
- Start with critical issues first
- Automate title/description generation
- Add missing alt texts
- Generate XML sitemap

### "Newsletter open rates low"
- A/B test subject lines
- Optimize send time
- Segment audience
- Improve content selection

---

## 📞 Support

For questions or improvements:
- Review skill documentation
- Check implementation examples
- Test with sample data
- Iterate based on metrics

---

## 🎓 Next Steps

1. **Start with Design Review**
   - Get baseline understanding
   - Fix critical issues
   - Establish standards

2. **Implement Quality Gates**
   - Prevent bad content
   - Track quality over time
   - Improve synthesis

3. **Launch Newsletters**
   - Build subscriber base
   - Drive traffic
   - Increase retention

4. **Add Recommendations**
   - Personalize experience
   - Increase engagement
   - Improve metrics

5. **Optimize for SEO**
   - Drive organic traffic
   - Improve rankings
   - Build authority

6. **Enable Fact Checking**
   - Build trust
   - Differentiate from competitors
   - Improve credibility

7. **Predict Trends**
   - Stay ahead
   - Find unique stories
   - Maximize impact

---

**Built for AI-ктуално** | Comprehensive toolkit for next-level news platform
