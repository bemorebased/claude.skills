---
name: pipeline-health
description: Monitor pipeline execution, detect bottlenecks, and check RSS feed health
---

# Pipeline Health Monitor Skill

Monitors the news aggregation pipeline, identifies performance bottlenecks, analyzes failure patterns, and checks RSS feed health to ensure smooth operation of your news platform.

## Usage

```bash
# Check real-time pipeline health
/pipeline-health --real-time

# Analyze pipeline performance over period
/pipeline-health --period=last-7-days

# Find bottlenecks in pipeline execution
/pipeline-health --find-bottlenecks

# Monitor RSS feed status
/pipeline-health --check-sources

# Get detailed failure analysis
/pipeline-health --analyze-failures

# Generate full health report
/pipeline-health --full-report
```

## What it Monitors

**Pipeline Metrics:**
- Total runs and success rate
- Average execution duration
- Articles processed per run
- Success vs failure counts

**Bottleneck Detection:**
- Step-by-step timing analysis
- Identify slow operations (>20% of total time)
- Detect timeout spikes (3x+ average duration)
- Prioritize optimization opportunities

**Failure Patterns:**
- Categorize errors (Timeout, Network, Parsing, Validation, AI, Image, Database)
- Track failure frequency and percentage
- Identify affected pipeline steps
- Suggest common causes and fixes

**RSS Feed Health:**
- Consecutive failure tracking
- Last success/failure timestamps
- Average response times
- Health percentage (0-100%)
- Status: healthy / degraded / failing / down

## Example Output

```
Pipeline Health Report
=====================
Overall Status: ⚠️ DEGRADED
Period: 2025-12-11 to 2025-12-18 (7 days)

Pipeline Metrics:
----------------
Total Runs: 168 (hourly)
Success Rate: 87.5% (147/168)
Average Duration: 4.2 minutes
Articles Processed: 2,450 total (14.6 avg/run)

Bottlenecks:
-----------
1. AI Rewrite Step - 45.2% of total time (2m 15s avg)
   → HIGH PRIORITY: Optimize or parallelize this operation

2. Image Fetching Step - 28.7% of total time (1m 20s avg)
   → MEDIUM PRIORITY: Consider optimizing (increase timeout/retries)

3. Extraction Step - 15.3% of total time (38s avg)
   → Performing well

4. Aggregation Step - 6.8% of total time (17s avg)
   → Performing well

5. Clustering Step - 4.0% of total time (10s avg)
   → Performing well

Failure Patterns:
----------------
1. Validation Error - 42 failures (45.7%)
   Affected Steps: saveArticle
   Common Causes:
   - Article content too short (<500 chars)
   - Prompt-like title detected
   - Placeholder image not replaced
   Examples:
   - "VALIDATION_FAILED_TITLE: Could not extract valid title"
   - "Content length 247 chars, minimum 500 required"

2. Timeout - 28 failures (30.4%)
   Affected Steps: fetchImages, extractFromURL
   Common Causes:
   - Source site is slow or unresponsive
   - Timeout threshold too low (currently 10-15s)
   - Network congestion
   Examples:
   - "Image fetch timeout (15s): https://example.com/article"

3. Network Error - 18 failures (19.6%)
   Affected Steps: rssFetch, apiAggregation
   Common Causes:
   - Source site is down or blocking requests
   - Rate limiting by source
   - DNS resolution issues

RSS Feed Health:
---------------
🔴 DOWN (2 feeds):
1. Fakti.bg
   - 7 consecutive failures
   - Last success: 3 days ago
   - Issues: 7 consecutive failures - feed may be permanently down

2. Focus-news.net (English)
   - 6 consecutive failures
   - Last success: 2 days ago
   - Issues: 6 consecutive failures - investigate soon

⚠️ FAILING (1 feed):
3. Actualno.com
   - 3 consecutive failures
   - Slow response time (12.3s avg)
   - Last success: 18 hours ago

⚠️ DEGRADED (3 feeds):
4. Dariknews.bg - 1 recent failure
5. Vesti.bg - Slow response time (6.2s avg)
6. Google News - Moderately slow (5.8s avg)

✓ HEALTHY (6 feeds):
- BNR, BNT, БТА, Dir.bg, Mediapool.bg, Economic.bg

Recommendations:
---------------
1. ⚠️ WARNING: Pipeline health is degraded. Monitor closely.
2. Optimize "AI Rewrite Step" (45.2% of total time) - consider faster models or parallel processing
3. Address Validation Error (45.7% of failures): Article content too short (<500 chars)
4. Address Timeout (30.4% of failures): Increase timeout threshold or improve retry logic
5. Fix RSS feed: Fakti.bg (down) - 7 consecutive failures - feed may be permanently down
6. Fix RSS feed: Focus-news.net (failing) - 6 consecutive failures - investigate soon
7. Fix RSS feed: Actualno.com (failing) - Slow response time (12.3s avg)

Next Steps:
----------
- Review AI rewrite configuration (consider cheaper/faster models)
- Increase image fetch timeout from 10s to 15-20s
- Review validation thresholds (maybe lower min content length to 400 chars)
- Check if Fakti.bg and Focus-news.net feeds have moved or changed format
- Add caching layer for slow RSS feeds

Run with --full-report to export detailed CSV
```

## Alerts and Thresholds

**Critical Alerts** (immediate action):
- Success rate <50%
- >3 RSS feeds down
- Pipeline duration >10 minutes avg
- >50% of runs failing

**Warning Alerts** (monitor closely):
- Success rate <80%
- >2 RSS feeds failing
- Pipeline duration >5 minutes avg
- Any single step >40% of total time

**Info Alerts** (nice to know):
- RSS feed degraded (1 failure)
- Step duration spikes (3x+ average)
- Minor parsing errors (<5% failure rate)

## Use Cases

1. **Daily Health Check**: Quick overview of pipeline status before starting work
2. **Performance Optimization**: Identify which pipeline steps need optimization
3. **Debugging Failures**: Understand why articles are failing validation or processing
4. **RSS Feed Management**: Track which feeds are reliable vs problematic
5. **Capacity Planning**: Monitor if pipeline can handle increased load
6. **On-Call Alerts**: Check critical issues requiring immediate attention

## Integration with Other Skills

Pairs well with:
- `/detect-prompt-leakage` - Investigate validation failures
- `/detect-placeholders` - Check image quality issues
- `/analyze-ai-quality` - Analyze AI step performance

## Implementation

Uses `lib/utils/pipelineHealthMonitor.ts`

The PipelineHealthMonitor class analyzes pipeline execution data, groups errors by type, calculates performance metrics, and generates actionable health reports.

Extends functionality from:
- `lib/utils/pipelineMetrics.ts` - Pipeline metrics collection
- `services/orchestrator/pipeline.ts` - Main pipeline implementation

## Related Files

- `services/aggregation/rss.ts` - RSS feed management
- `services/orchestrator/pipeline.ts` - Pipeline execution
- `.github/workflows/generate-articles.yml` - Hourly automation
