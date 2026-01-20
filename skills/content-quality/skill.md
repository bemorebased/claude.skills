# Content Quality Analyzer

Advanced AI-powered content quality analysis for synthesized articles. Detects issues, scores quality, and prevents publication of low-quality content.

## Purpose

Ensure every synthesized article meets high editorial standards before publication. Detect repetition, incoherence, poor structure, and insufficient source diversity.

## Core Capabilities

### 1. Repetition Detection
- Identify duplicate paragraphs/sentences
- Detect similar phrasing (semantic similarity)
- Flag circular reasoning
- Catch copy-paste errors from sources

### 2. Coherence Scoring
- Logical flow between paragraphs
- Topic consistency throughout article
- Proper transitions and structure
- Introduction → Body → Conclusion flow

### 3. Source Quality Analysis
- Source diversity (multiple providers)
- Source credibility assessment
- Geographic/political balance
- Timestamp recency

### 4. Readability Metrics
- Flesch reading ease
- Average sentence length
- Vocabulary complexity
- Paragraph structure

### 5. Factual Consistency
- Check for contradictions within article
- Cross-reference facts across sources
- Flag unsubstantiated claims
- Identify opinion vs. fact

### 6. SEO & Engagement
- Title effectiveness
- Meta description quality
- Keyword usage
- Hook strength

## Usage

### Analyze Single Article
```bash
claude /quality-check --article-id=xxx
```

### Analyze Before Publishing
```bash
claude /quality-check --draft --topic="изкуствен интелект"
```

### Bulk Analysis
```bash
claude /quality-check --date=today
claude /quality-check --last=10
```

### Quality Gate (Block Publishing)
```bash
claude /quality-gate --threshold=70
# Returns: PASS/FAIL with detailed report
```

## Quality Score Breakdown

**Total Score: 0-100**

- **Content Quality (40 points)**
  - Originality: 15 pts (no repetition)
  - Coherence: 15 pts (logical flow)
  - Depth: 10 pts (substantial analysis)

- **Source Quality (30 points)**
  - Diversity: 10 pts (3+ providers)
  - Credibility: 10 pts (reliable sources)
  - Freshness: 10 pts (recent articles)

- **Readability (20 points)**
  - Clarity: 10 pts (easy to understand)
  - Structure: 10 pts (proper formatting)

- **Engagement (10 points)**
  - Title: 5 pts (compelling, accurate)
  - Hook: 5 pts (strong opening)

## Thresholds

- **90-100**: Excellent - Publish immediately
- **70-89**: Good - Minor edits recommended
- **50-69**: Fair - Significant revision needed
- **0-49**: Poor - Reject and regenerate

## Task Execution

When invoked:

1. **Load Article Data**
   - Fetch article content from database or draft
   - Load source information
   - Get synthesis metadata

2. **Run Analysis Pipeline**
   ```
   → Repetition Detection
   → Coherence Analysis
   → Source Quality Check
   → Readability Scoring
   → Fact Consistency
   → SEO Optimization
   ```

3. **Generate Detailed Report**
   - Overall score and grade
   - Issues found with severity
   - Specific line/paragraph references
   - Improvement suggestions
   - Source analysis

4. **Actionable Recommendations**
   - If score < 70: Suggest regeneration with better prompts
   - If 70-89: List specific edits to make
   - If 90+: Approve for publishing

5. **Auto-Fix (Optional)**
   - Remove repetitive paragraphs
   - Improve transitions
   - Enhance title/summary
   - Restructure if needed

## Detection Algorithms

### Repetition Detection
```javascript
// N-gram similarity
- Extract 3-5 word sequences
- Compare across paragraphs
- Flag if >80% similarity

// Semantic similarity
- Embed sentences with AI
- Calculate cosine similarity
- Flag if >0.9 similarity
```

### Coherence Scoring
```javascript
// Topic modeling
- Extract main topics per paragraph
- Check topic continuity
- Identify abrupt topic shifts

// Transition quality
- Detect transition words
- Check logical connectors
- Identify paragraph flow
```

### Source Diversity
```javascript
// Provider distribution
- Count sources per provider
- Penalize if >50% from one provider

// Content uniqueness
- Check if sources report same facts
- Identify unique angles/perspectives
```

## Output Format

```json
{
  "articleId": "xxx",
  "overallScore": 75,
  "grade": "Good",
  "passed": true,
  "timestamp": "2025-01-22T...",

  "scores": {
    "content": {
      "originality": 12,
      "coherence": 13,
      "depth": 8,
      "total": 33
    },
    "sources": {
      "diversity": 9,
      "credibility": 8,
      "freshness": 10,
      "total": 27
    },
    "readability": {
      "clarity": 8,
      "structure": 7,
      "total": 15
    },
    "engagement": {
      "title": 4,
      "hook": 4,
      "total": 8
    }
  },

  "issues": [
    {
      "severity": "major",
      "category": "repetition",
      "description": "Paragraph 3 and 5 are 85% similar",
      "location": "paragraphs 3, 5",
      "suggestion": "Remove paragraph 5 or rephrase significantly"
    },
    {
      "severity": "minor",
      "category": "readability",
      "description": "Average sentence length: 32 words (recommended: <25)",
      "location": "paragraph 2",
      "suggestion": "Break long sentences into shorter ones"
    }
  ],

  "sources": {
    "total": 7,
    "providers": {
      "google_news": 2,
      "thenewsapi": 3,
      "newsdata": 2
    },
    "diversity": "good",
    "averageAge": "4 hours"
  },

  "recommendations": [
    "Remove repetitive paragraph 5",
    "Improve sentence variety in paragraph 2",
    "Add concluding paragraph for better structure"
  ],

  "decision": {
    "action": "approve_with_edits",
    "confidence": 0.85,
    "reasoning": "Good quality overall but minor improvements will enhance readability"
  }
}
```

## Integration Points

### 1. Synthesis Pipeline
Add quality check after synthesis, before saving:
```typescript
// In articleSynthesizer.ts
const result = await synthesize(topic)
const quality = await qualityAnalyzer.check(result)

if (quality.score < 70) {
  // Regenerate with improved prompts
  return await synthesize(topic, { improvedPrompts: true })
}
```

### 2. Admin UI
Show quality score in admin panel:
```typescript
// Display quality badge
<QualityBadge score={article.qualityScore} />

// Show detailed report
<QualityReport data={article.qualityAnalysis} />
```

### 3. API Endpoint
Create `/api/admin/quality/analyze` endpoint:
```typescript
POST /api/admin/quality/analyze
{
  "articleId": "xxx",
  "autoFix": true
}

Response: { quality report }
```

### 4. Automated Quality Gate
Run on cron job for all new articles:
```bash
# Every hour, check articles from last hour
0 * * * * claude /quality-check --last-hour --auto-reject
```

## Success Metrics

- **Rejection rate** for low-quality articles
- **Average quality score** trend over time
- **User engagement** correlation with quality scores
- **Repetition incidents** reduced to near zero
- **Editorial review time** decreased by 70%

## Future Enhancements

- ML model trained on user engagement data
- Multilingual quality analysis
- Industry-specific quality standards
- Plagiarism detection against published articles
- Real-time quality preview during synthesis
