---
name: analyze-ai-quality
description: Analyze AI model performance across providers (OpenRouter, Cerebras, OpenAI, Gemini)
---

# AI Quality Analyzer Skill

Analyzes AI model performance, tracks quality trends, identifies problematic prompts, and recommends improvements for your news platform's AI rewriting system.

## Usage

```bash
# Analyze provider performance over last 30 days
/analyze-ai-quality --period=last-30-days

# Compare specific providers
/analyze-ai-quality --compare=openrouter,cerebras,gemini

# Track quality trends over time
/analyze-ai-quality --trends --period=last-7-days

# Identify problematic prompts by category/mode
/analyze-ai-quality --find-issues --category=all

# Get improvement suggestions
/analyze-ai-quality --suggest-improvements
```

## Metrics Tracked

**Provider Performance:**
- Total articles processed
- Successful rewrites vs failures
- Prompt leakage count and rate
- Average content length
- Average quality score (credibility)
- Success rate

**Quality Trends:**
- Daily prompt leakage rate
- Daily placeholder image rate
- Daily average quality scores
- Trend direction (improving/declining)

**Problematic Prompts:**
- Leakage rate by category/mode/provider
- Common issues per combination
- Example articles with issues
- High-priority problem areas (>20% leakage)

**Model Comparisons:**
- Success rate comparison
- Leakage rate comparison (lower is better)
- Quality score comparison (higher is better)
- Speed comparison (if tracked)

## Example Output

```
AI Quality Analysis Report
==========================
Period: 2025-12-11 to 2025-12-18 (7 days)
Total Articles Analyzed: 2,450

Provider Performance:
---------------------
OpenRouter:
  Articles: 1,200 (49.0%)
  Success Rate: 98.5%
  Leakage Rate: 12.3% ⚠️ HIGH
  Avg Quality: 75/100
  Avg Length: 850 chars

Cerebras:
  Articles: 950 (38.8%)
  Success Rate: 97.2%
  Leakage Rate: 18.7% 🔴 CRITICAL
  Avg Quality: 72/100
  Avg Length: 720 chars

Gemini:
  Articles: 300 (12.2%)
  Success Rate: 99.1%
  Leakage Rate: 3.2% ✓ GOOD
  Avg Quality: 82/100
  Avg Length: 920 chars

Best Provider: Gemini (lowest leakage, highest quality)
Worst Provider: Cerebras (highest leakage)

Quality Trends:
--------------
Improving: quality scores +5%, placeholder images -8%
Declining: prompt leakage +3%

Summary: Overall improvement in quality and images, but prompt
leakage increasing. Action needed.

Problematic Prompts:
-------------------
HIGH PRIORITY:
1. технологии/news_standard/cerebras - 24.5% leakage
   Issues: Title contains "ЗАГЛАВИЕ:", Content contains "резюме:"

2. българия/full/openrouter - 19.2% leakage
   Issues: Summary contains "Преработена статия"

MEDIUM PRIORITY:
3. свят/summary/openrouter - 11.8% leakage

Recommendations:
---------------
1. CRITICAL: Cerebras has 18.7% leakage - simplify prompts or switch provider
2. Switch to Gemini for higher quality (82 avg) and lower leakage (3.2%)
3. Review prompts for технологии/news_standard combination
4. Add post-processing validation for all providers
5. Consider JSON output format for cheaper models (Cerebras, OpenRouter)

Run with --report to export detailed CSV
```

## Improvement Suggestions

Based on analysis, the tool provides prioritized suggestions:

**HIGH PRIORITY** (>20% leakage):
- Simplify prompt structure
- Remove explicit labels ("ЗАГЛАВИЕ:", "РЕЗЮМЕ:")
- Switch to JSON output format
- Consider changing provider

**MEDIUM PRIORITY** (10-20% leakage):
- Review and enhance regex patterns
- Add Bulgarian pattern variations
- Improve cleaning functions

**LOW PRIORITY** (5-10% leakage):
- Add post-processing validation
- Fine-tune existing patterns

## Use Cases

1. **Monthly Review**: Check provider performance and make decisions about which AI models to use
2. **Quality Monitoring**: Track if platform quality is improving or declining over time
3. **Debugging**: Identify specific category/mode/provider combinations that produce low-quality articles
4. **Cost Optimization**: Compare free providers (OpenRouter, Cerebras) vs paid (OpenAI) to find best value
5. **Prompt Engineering**: Get data-driven insights for improving prompt templates

## Integration with Other Skills

Pairs well with:
- `/detect-prompt-leakage` - See specific articles with issues
- `/detect-placeholders` - Check image quality alongside AI quality
- `/pipeline-health` - Get full platform health overview

## Implementation

Uses `/Users/sheshe/aiktualno/lib/utils/aiQualityAnalyzer.ts`

The AIQualityAnalyzer class analyzes articles from the database, groups by provider/category/mode, and generates comprehensive performance reports.

Integrates with:
- `lib/utils/promptLeakageDetector.ts` - Detect leakage in analyzed articles
- `lib/utils/placeholderImageDetector.ts` - Check image quality
- Database schema: `articles.isRewritten`, `articles.rewriteProvider`, `articles.rewriteMode`, `articles.credibilityScore`
