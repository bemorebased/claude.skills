# Multi-Source Fact Checker

Advanced fact-checking system that verifies claims across multiple sources and flags inconsistencies.

## Purpose

Build trust and credibility by automatically verifying factual claims, detecting contradictions, and providing source transparency.

## Core Capabilities

### 1. Claim Extraction
- Identify factual statements in articles
- Separate facts from opinions
- Extract dates, numbers, names, events
- Classify claim types (statistical, historical, quote, etc.)

### 2. Cross-Reference Verification
- Search across source articles for same claim
- Compare reported facts
- Identify contradictions
- Calculate claim confidence

### 3. Source Credibility
- Rate source reliability
- Check source bias
- Verify publication dates
- Track source consistency over time

### 4. Confidence Scoring
- **Verified (90-100%)**: Multiple reliable sources agree
- **Likely (70-89%)**: Most sources agree
- **Disputed (30-69%)**: Sources contradict
- **Unverified (<30%)**: No corroboration found

### 5. Citation Linking
- Link claims to specific sources
- Provide inline citations
- Generate source references
- Enable fact verification UI

## Usage

### Check Single Article
```bash
claude /fact-check --article-id=xxx
```

### Check Specific Claim
```bash
claude /fact-check --claim="България инвестира €50М в AI"
```

### Batch Verification
```bash
claude /fact-check --date=today
```

### Live Verification During Synthesis
```bash
# Integrated into synthesis pipeline
claude /synthesize --topic=xxx --fact-check=true
```

## Fact Extraction Pipeline

```javascript
// 1. Parse article into sentences
const sentences = nlp.sentences(article.content)

// 2. Identify factual claims
const claims = sentences.filter(s =>
  s.hasDates() ||
  s.hasNumbers() ||
  s.hasProperNouns() ||
  s.hasVerbs(['said', 'announced', 'reported'])
)

// 3. Extract structured facts
const facts = claims.map(claim => ({
  text: claim.text,
  type: classifyClaimType(claim),
  entities: extractEntities(claim),
  metadata: {
    confidence: 0, // To be calculated
    sources: [],
    verified: false
  }
}))
```

## Verification Algorithm

```javascript
async function verifyClaim(claim, sources) {
  const results = []

  // Search each source for similar claims
  for (const source of sources) {
    const matches = semanticSearch(claim, source.content)

    for (const match of matches) {
      const similarity = calculateSimilarity(claim, match)

      if (similarity > 0.8) {
        results.push({
          source: source.name,
          claim: match.text,
          similarity,
          agrees: !detectContradiction(claim, match)
        })
      }
    }
  }

  // Calculate confidence
  const agreeing = results.filter(r => r.agrees).length
  const disagreeing = results.filter(r => !r.agrees).length

  const confidence = results.length > 0
    ? (agreeing / results.length) * 100
    : 0

  return {
    claim: claim.text,
    confidence,
    status: getStatus(confidence),
    sources: results,
    contradictions: results.filter(r => !r.agrees)
  }
}
```

## Contradiction Detection

```javascript
function detectContradiction(claim1, claim2) {
  // Extract numbers
  const nums1 = extractNumbers(claim1)
  const nums2 = extractNumbers(claim2)

  // Check if numbers differ significantly
  if (nums1.length > 0 && nums2.length > 0) {
    const diff = Math.abs(nums1[0] - nums2[0])
    const percentDiff = diff / Math.max(nums1[0], nums2[0])

    if (percentDiff > 0.1) return true // >10% difference
  }

  // Check for negation
  if (hasNegation(claim1) !== hasNegation(claim2)) {
    return true
  }

  // Check dates
  const dates1 = extractDates(claim1)
  const dates2 = extractDates(claim2)

  if (dates1[0] && dates2[0] && dates1[0] !== dates2[0]) {
    return true
  }

  return false
}
```

## UI Integration

### Inline Fact Badges
```jsx
<p>
  България инвестира €50М в AI
  <FactBadge
    status="verified"
    confidence={95}
    sources={3}
    tooltip="Verified by 3 sources"
  />
</p>
```

### Fact Details Modal
```jsx
<FactDetails>
  <h3>Claim: "България инвестира €50М в AI"</h3>

  <StatusBadge status="verified" confidence={95} />

  <Sources>
    <Source
      name="Dnevnik.bg"
      agrees={true}
      excerpt="Правителството одобри €50М за AI..."
    />
    <Source
      name="Bgonair"
      agrees={true}
      excerpt="Инвестиция от 50 милиона евро..."
    />
  </Sources>

  <Timeline>
    Verified: 2025-01-22 14:30
    Sources checked: 3
    Last updated: 2 hours ago
  </Timeline>
</FactDetails>
```

## Output Format

```json
{
  "articleId": "xxx",
  "checkedAt": "2025-01-22T14:30:00Z",
  "totalClaims": 12,
  "verified": 8,
  "disputed": 2,
  "unverified": 2,

  "claims": [
    {
      "id": "claim-1",
      "text": "България инвестира €50М в AI",
      "type": "financial",
      "status": "verified",
      "confidence": 95,

      "evidence": [
        {
          "source": "Dnevnik.bg",
          "url": "https://...",
          "excerpt": "Правителството одобри €50М...",
          "agrees": true,
          "similarity": 0.92
        },
        {
          "source": "Bgonair",
          "url": "https://...",
          "excerpt": "Инвестиция от 50 милиона...",
          "agrees": true,
          "similarity": 0.88
        }
      ],

      "entities": {
        "location": "България",
        "amount": "€50М",
        "topic": "AI"
      }
    },
    {
      "id": "claim-2",
      "text": "Проектът стартира през март",
      "type": "temporal",
      "status": "disputed",
      "confidence": 40,

      "evidence": [
        {
          "source": "Source A",
          "excerpt": "старт през март",
          "agrees": true
        },
        {
          "source": "Source B",
          "excerpt": "очаква се през април",
          "agrees": false
        }
      ],

      "contradictions": [
        {
          "claim1": "март",
          "claim2": "април",
          "difference": "1 месец"
        }
      ]
    }
  ],

  "summary": {
    "overallReliability": "high",
    "majorContradictions": 1,
    "unsupportedClaims": 2,
    "recommendation": "publish_with_notes"
  }
}
```

## Integration Points

### 1. Article Display
Show fact-check results on article pages:
```tsx
{article.factCheck && (
  <FactCheckSummary
    verified={article.factCheck.verified}
    disputed={article.factCheck.disputed}
    onDetailsClick={() => showFactModal()}
  />
)}
```

### 2. Synthesis Pipeline
Add fact-checking step:
```typescript
const synthesized = await synthesize(topic)
const factCheck = await factChecker.verify(synthesized, sources)

if (factCheck.disputed > 3) {
  console.warn('Multiple disputed claims - review needed')
}
```

### 3. Database Schema
```sql
ALTER TABLE articles ADD COLUMN fact_check JSONB;
CREATE INDEX idx_fact_check ON articles USING GIN (fact_check);
```

## Success Metrics

- **Fact-check coverage**: % of articles checked
- **Claim verification rate**: % of claims verified
- **User trust score**: Measured via surveys
- **Correction rate**: How often we catch errors
- **Transparency**: % of claims with source links
