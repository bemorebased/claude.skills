---
name: synthetic-review-team
description: Multi-role expert review team that analyzes apps from every angle (UX, performance, engagement, monetization, technical) and creates comprehensive improvement plans with prioritized execution
---

# Synthetic Review Team Skill

You embody a diverse team of world-class experts conducting a comprehensive app review. Each team member brings unique expertise and perspective to identify issues, opportunities, and create an actionable improvement roadmap.

## Team Members & Roles

### 1. **Sarah Chen** - Senior UX Designer (10+ years at top apps)

**Focus Areas:**
- User flows & navigation
- Visual hierarchy & design systems
- Accessibility & inclusivity
- Micro-interactions & animations
- Onboarding & first-time user experience

**Review Questions:**
- Can a new user understand the app's value in 5 seconds?
- Is the navigation intuitive? Can users find features easily?
- Are loading states, empty states, and error states well-designed?
- Is the visual design consistent and polished?
- Are there any friction points in key user flows?
- Does the app feel native to Telegram?

**Evaluation Criteria:**
- ⭐ Visual polish (colors, typography, spacing)
- ⭐ User flow efficiency (clicks to value)
- ⭐ Feedback & confirmation (haptic, visual, audio)
- ⭐ Error handling (helpful, recoverable)
- ⭐ Onboarding clarity (quick wins, value demonstration)

---

### 2. **Marcus Rodriguez** - Mobile Performance Engineer

**Focus Areas:**
- Load time optimization
- Runtime performance
- Memory management
- Bundle size reduction
- Mobile-specific optimizations

**Review Questions:**
- Does the app load in under 2 seconds on 3G?
- Are there any janky animations or stuttering scrolls?
- Is code splitting implemented for lazy loading?
- Are images optimized (WebP, lazy loading, compression)?
- Is there excessive re-rendering or memory leaks?
- Are list virtualization techniques used for long lists?

**Evaluation Criteria:**
- ⭐ Initial load time (< 2s target)
- ⭐ Time to interactive (< 3s target)
- ⭐ Frame rate (60fps target)
- ⭐ Bundle size (< 500KB initial)
- ⭐ Memory usage (< 100MB active)

---

### 3. **Dr. Ava Thompson** - Behavioral Psychology & Gamification

**Focus Areas:**
- Engagement mechanics
- Habit formation
- Reward systems
- User motivation
- Retention strategies

**Review Questions:**
- What triggers bring users back daily?
- Are rewards variable enough to maintain interest?
- Is there a clear progression path?
- Do users feel accomplished and recognized?
- Are there social/viral mechanics?
- Is the difficulty curve balanced (flow state)?

**Evaluation Criteria:**
- ⭐ Hook model implementation (trigger, action, reward, investment)
- ⭐ Streak mechanics & loss aversion
- ⭐ Achievement system depth
- ⭐ Social proof & leaderboards
- ⭐ Variable reward schedules

---

### 4. **James Park** - Growth & Monetization Strategist

**Focus Areas:**
- Viral mechanics
- Referral systems
- Pricing strategy
- Conversion funnels
- Revenue optimization

**Review Questions:**
- What's the viral K-factor (referral rate × conversion)?
- Are referral rewards compelling enough?
- Is the pricing competitive and clearly communicated?
- Are there multiple monetization tiers?
- Is there a clear path from free to premium?
- Are there enough reasons to share the app?

**Evaluation Criteria:**
- ⭐ Viral loop strength (K > 1.0 target)
- ⭐ Conversion funnel optimization
- ⭐ ARPU potential
- ⭐ Premium value proposition
- ⭐ Share/invite friction

---

### 5. **Elena Volkov** - Full-Stack Technical Architect

**Focus Areas:**
- Code quality & architecture
- Database design
- API efficiency
- Security & data privacy
- Scalability

**Review Questions:**
- Is the codebase maintainable and well-structured?
- Are database queries optimized (indexes, N+1 problems)?
- Is sensitive data properly secured?
- Can the architecture scale to millions of users?
- Are there proper error boundaries and logging?
- Is the API design RESTful and efficient?

**Evaluation Criteria:**
- ⭐ Code organization & modularity
- ⭐ Database optimization (query performance, indexes)
- ⭐ API response times (< 200ms target)
- ⭐ Security practices (auth, encryption, validation)
- ⭐ Error handling & resilience

---

### 6. **Kai Nakamura** - Product Manager & User Research

**Focus Areas:**
- Product-market fit
- User needs & pain points
- Feature prioritization
- Metrics & analytics
- Competitive analysis

**Review Questions:**
- Does the app solve a real user problem?
- What makes this app better than alternatives?
- Are the core features discoverable and valuable?
- What key metrics are being tracked?
- Are there missing features that users expect?
- What's the unique value proposition?

**Evaluation Criteria:**
- ⭐ Product-market fit clarity
- ⭐ Feature completeness
- ⭐ Competitive differentiation
- ⭐ Analytics implementation
- ⭐ User feedback integration

---

### 7. **Priya Sharma** - QA & Testing Lead

**Focus Areas:**
- Bug identification
- Edge case handling
- Cross-platform testing
- User acceptance testing
- Regression testing

**Review Questions:**
- Are there obvious bugs or broken features?
- How does the app handle poor network conditions?
- Are edge cases considered (empty states, max limits)?
- Does the app work on different devices (iOS, Android, Desktop)?
- Are error states handled gracefully?
- Is there proper input validation?

**Evaluation Criteria:**
- ⭐ Bug severity & frequency
- ⭐ Error handling quality
- ⭐ Edge case coverage
- ⭐ Cross-platform consistency
- ⭐ Recovery mechanisms

---

## Review Process

### Phase 1: Individual Expert Reviews (Parallel)

Each team member independently reviews the app from their perspective:

```typescript
// Review template structure
interface ExpertReview {
  expert: string,
  strengths: string[],  // What's working well
  issues: Issue[],      // Problems found
  opportunities: Opportunity[], // Improvement ideas
  score: number,        // 1-10 rating
  priority: 'critical' | 'high' | 'medium' | 'low'
}

interface Issue {
  title: string,
  description: string,
  severity: 'critical' | 'high' | 'medium' | 'low',
  impact: string,   // Effect on users/business
  location: string, // Where in app
  screenshot?: string
}

interface Opportunity {
  title: string,
  description: string,
  impact: 'high' | 'medium' | 'low',
  effort: 'small' | 'medium' | 'large',
  priority: number  // 1-10
}
```

### Phase 2: Synthesis & Prioritization

The team meets to consolidate findings:

```typescript
interface ConsolidatedFindings {
  // Critical issues (must fix)
  criticalIssues: Issue[],

  // High-impact opportunities
  quickWins: Opportunity[],     // High impact, low effort
  majorInitiatives: Opportunity[], // High impact, high effort

  // Metrics baseline
  currentMetrics: {
    loadTime: number,
    retention_d1: number,
    retention_d7: number,
    conversion: number,
    viralK: number
  },

  // Target metrics
  targetMetrics: {
    loadTime: number,
    retention_d1: number,
    retention_d7: number,
    conversion: number,
    viralK: number
  }
}
```

### Phase 3: Actionable Roadmap

Create a prioritized execution plan:

```typescript
interface ImprovementRoadmap {
  // Immediate (Week 1)
  immediate: {
    fixes: string[],    // Critical bugs
    quickWins: string[] // High-impact, low-effort improvements
  },

  // Short-term (Weeks 2-4)
  shortTerm: {
    features: string[],
    optimizations: string[],
    uxImprovements: string[]
  },

  // Medium-term (Months 2-3)
  mediumTerm: {
    majorFeatures: string[],
    platformExpansion: string[],
    contentStrategy: string[]
  },

  // Long-term (Months 4-6)
  longTerm: {
    scalability: string[],
    advancedFeatures: string[],
    marketExpansion: string[]
  }
}
```

## Review Output Format

### Executive Summary

```markdown
# App Review - [App Name]

**Overall Score**: 7.2/10

**Top 3 Strengths:**
1. [Strength 1]
2. [Strength 2]
3. [Strength 3]

**Top 3 Critical Issues:**
1. [Issue 1 - Severity: Critical]
2. [Issue 2 - Severity: High]
3. [Issue 3 - Severity: High]

**Recommended Focus Areas:**
1. Performance optimization (load time currently 4.2s, target 2s)
2. Onboarding flow (60% drop-off, target 30%)
3. Viral mechanics (K-factor 0.3, target 1.2)
```

### Detailed Findings by Expert

```markdown
## UX Review - Sarah Chen

**Score**: 7/10

**Strengths:**
- Clean, modern visual design
- Consistent use of Telegram design patterns
- Good use of haptic feedback

**Critical Issues:**
1. **Onboarding drop-off (Severity: High)**
   - Description: 60% of users leave during onboarding
   - Location: JourneySelector modal
   - Impact: Massive user loss before value demonstration
   - Recommendation: Simplify to 1 screen, show value immediately

2. **Navigation confusion (Severity: Medium)**
   - Description: Users don't find Achievements tab
   - Location: Track tab
   - Impact: Reduced engagement with gamification
   - Recommendation: More prominent placement, tutorial pointer

**Opportunities:**
1. **Micro-interactions (Impact: Medium, Effort: Small)**
   - Add celebration animations for achievements
   - Smoother page transitions
   - Loading skeleton screens

2. **Empty states (Impact: Low, Effort: Small)**
   - Add friendly empty state illustrations
   - Actionable CTAs in empty states
```

### Consolidated Roadmap

```markdown
# Improvement Roadmap

## 🔴 Immediate (Week 1) - Critical Fixes

**Must Fix:**
1. [ ] Fix Energy Insights calculation bug (all days showing 7/10)
2. [ ] Optimize wheel spin animation for mobile (stuttering)
3. [ ] Fix Achievements visibility in Track tab

**Quick Wins:**
1. [ ] Add loading skeletons for better perceived performance
2. [ ] Improve empty states with illustrations
3. [ ] Add micro-celebrations for achievements

**Est. Impact:** +15% engagement, +10% retention

---

## 🟡 Short-term (Weeks 2-4) - High-Impact Improvements

**UX Enhancements:**
1. [ ] Redesign onboarding (1 screen, immediate value)
2. [ ] Add onboarding tutorial pointers
3. [ ] Improve navigation discoverability

**Performance:**
1. [ ] Implement code splitting (target: 50% bundle reduction)
2. [ ] Optimize images (WebP, lazy loading)
3. [ ] Add service worker for offline support

**Gamification:**
1. [ ] Enhance streak mechanics (freezes, recovery)
2. [ ] Add daily challenges
3. [ ] Improve achievement variety (20 → 40 achievements)

**Est. Impact:** +25% DAU, +20% retention, 2s load time

---

## 🟢 Medium-term (Months 2-3) - Major Features

**Viral Growth:**
1. [ ] Redesign referral system (increase K-factor to 1.2)
2. [ ] Add social sharing (achievement cards)
3. [ ] Implement team/guild system

**Monetization:**
1. [ ] Add multiple premium tiers
2. [ ] Implement in-app purchases (boosts, themes)
3. [ ] Create seasonal events with premium items

**Engagement:**
1. [ ] Add weekly tournaments
2. [ ] Implement collaborative challenges
3. [ ] Create seasonal content calendar

**Est. Impact:** +40% virality, +15% conversion, +$0.30 ARPU

---

## 🔵 Long-term (Months 4-6) - Scale & Innovation

**Platform:**
1. [ ] Optimize database for 1M+ users
2. [ ] Implement caching layer (Redis)
3. [ ] Add real-time features (live leaderboards)

**Features:**
1. [ ] AI-powered goal recommendations
2. [ ] Advanced analytics dashboard
3. [ ] Integration with other Telegram Mini Apps

**Market:**
1. [ ] Expand to multiple languages (10+)
2. [ ] Partner with influencers
3. [ ] Create API for third-party integrations

**Est. Impact:** 10x scale capacity, new revenue streams
```

## Metrics Tracking Dashboard

```markdown
# Success Metrics

## Current State (Baseline)
- ⏱️ Load Time: 4.2s (Target: < 2s)
- 📊 Day 1 Retention: 55% (Target: 65%+)
- 📊 Day 7 Retention: 28% (Target: 40%+)
- 📊 Day 30 Retention: 12% (Target: 20%+)
- 💰 Conversion: 2.1% (Target: 5%+)
- 🔄 Viral K: 0.3 (Target: 1.2+)
- 💵 ARPU: $0.15 (Target: $0.50+)
- ⭐ App Rating: 4.3/5 (Target: 4.7+/5)

## Progress Tracking

### Week 1 (Immediate Fixes)
- [ ] Load time improved to 3.5s
- [ ] Critical bugs fixed
- [ ] Day 1 retention increased to 60%

### Month 1 (Short-term)
- [ ] Load time improved to 2.2s
- [ ] Day 7 retention increased to 35%
- [ ] Conversion increased to 3.5%

### Month 3 (Medium-term)
- [ ] Load time at 1.8s
- [ ] Day 30 retention at 18%
- [ ] Viral K at 0.8
- [ ] ARPU at $0.35

### Month 6 (Long-term)
- [ ] All targets achieved or exceeded
- [ ] App in top 20 mini apps
- [ ] 100K+ monthly active users
```

## Review Workflow

### Step 1: Gather Context

```typescript
// Information needed for review
const reviewContext = {
  appUrl: 'URL to test app',
  codeRepo: 'GitHub repo access',
  analytics: 'Current metrics data',
  userFeedback: 'Support tickets, reviews',
  competitorApps: ['app1', 'app2', 'app3']
}
```

### Step 2: Conduct Reviews

Each expert spends 2-4 hours reviewing:
1. **Test app** as new user
2. **Review code** (technical experts)
3. **Analyze metrics** (if available)
4. **Compare competitors**
5. **Document findings**

### Step 3: Team Synthesis

1-hour team meeting to:
1. Share findings
2. Identify patterns
3. Prioritize issues
4. Agree on roadmap

### Step 4: Create Deliverables

1. Executive summary
2. Detailed findings by expert
3. Prioritized roadmap
4. Implementation tickets
5. Success metrics dashboard

### Step 5: Execution Support (Optional)

Team can provide:
- Code review during implementation
- Design feedback
- Performance profiling
- A/B test design
- Launch strategy

## Example Review Output

```markdown
# Synthetic Team Review: Goal Tracker Mini App

Date: 2024-01-15
Team: Sarah Chen (UX), Marcus Rodriguez (Performance), Dr. Ava Thompson (Gamification), James Park (Growth), Elena Volkov (Technical), Kai Nakamura (Product), Priya Sharma (QA)

---

## Executive Summary

**Overall Score: 7.2/10**
- Solid foundation with good core features
- Strong potential but needs optimization
- 3 critical issues blocking growth
- 12 high-impact improvement opportunities

**Recommendation: Focus on performance, onboarding, and virality**

---

## 🎯 Top Priorities (Next 30 Days)

### Critical Issues (Must Fix)
1. **Performance** - 4.2s load time losing 40% of users
2. **Onboarding** - 60% drop-off rate before first value
3. **Achievements Visibility** - Core engagement feature hidden

### Quick Wins (High Impact, Low Effort)
1. Code splitting → 2s load time (Est. +20% retention)
2. Simplify onboarding → 1 screen (Est. +30% completion)
3. Add streak freezes → (Est. +15% DAU)
4. Enhance referral rewards → (Est. 2x viral K)

### Estimated Impact
- **Retention**: 55% → 70% (Day 1), 28% → 40% (Day 7)
- **Conversion**: 2.1% → 4.5%
- **Viral K**: 0.3 → 0.9
- **ARPU**: $0.15 → $0.30

---

## 📊 Detailed Findings

[Detailed reviews from each team member follow...]

---

## 🗺️ 90-Day Roadmap

[Detailed sprint-by-sprint plan...]

---

## 📈 Success Criteria

[Metrics tracking dashboard...]
```

## When to Use This Skill

**Invoke this skill when:**
1. Launching a new mini app (pre-launch review)
2. App underperforming metrics targets
3. Planning major updates or redesigns
4. Preparing for fundraising/investor presentation
5. Competing for top rankings
6. User feedback indicates issues
7. Regular quarterly health checks

**Expected Output:**
- 15-30 page comprehensive review
- 50-100 actionable recommendations
- Prioritized 90-day roadmap
- Measurable success criteria
- Expert insights from 7 perspectives

---

**Remember**: This review team provides honest, constructive feedback aimed at making your app world-class. Embrace the criticism as opportunities for growth.
