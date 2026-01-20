---
name: detect-prompt-leakage
description: Detect Bulgarian prompt terms and instructions leaking into article content
---

# Prompt Leakage Detector Skill

Detects Bulgarian prompt artifacts ("ЗАГЛАВИЕ:", "РЕЗЮМЕ:", "СЪДЪРЖАНИЕ:") and AI meta-language in article titles, summaries, and content.

## Usage

```bash
# Scan recent articles for prompt leakage
/detect-prompt-leakage --last=500

# Scan specific article by ID
/detect-prompt-leakage --article-id=xxx

# Generate detailed report with affected articles
/detect-prompt-leakage --report --format=csv

# Check only high severity issues
/detect-prompt-leakage --severity=high --last=1000
```

## What it Detects

**Bulgarian Label Patterns:**
- ЗАГЛАВИЕ:, Заглавие:, заглавие:
- РЕЗЮМЕ:, Резюме:, резюме:
- СЪДЪРЖАНИЕ:, Съдържание:, съдържание:
- СТАТИЯ:, КАТЕГОРИЯ:, ИЗТОЧНИК:, АВТОР:

**Instruction Patterns:**
- Преработена статия
- Оригинално заглавие
- Оригинално съдържание
- Инструкции:, Правила:, Структура:

**Meta-Language (AI thinking):**
- "Хм, трябва...", "Сега, трябва...", "Първо, трябва..."
- "трябва да създам", "нека създам", "ще създам"
- "напиши статия", "създай статия", "преработи статия"
- "Целта е да...", "Искам да...", "Започвам с..."

**Formatting Instructions:**
- "youtube-style", "headline-based"
- "формат на статия"
- "🔴 КРИТИЧНО", "⚠️ ВАЖНО"
- "НЕ пиши", "НЕ включвай"

## Severity Levels

- **HIGH**: Prompt leakage in title (most visible, critical)
- **MEDIUM**: Prompt leakage in summary (very visible)
- **LOW**: Prompt leakage only in content (less critical)
- **NONE**: No leakage detected

## Example Output

```
Prompt Leakage Report
=====================
Scanned: 500 articles
Affected: 47 articles (9.4%)

High Severity (Title): 12 articles
Medium Severity (Summary): 8 articles
Low Severity (Content only): 27 articles

Sample Issues:
- Article #1234: Title contains "ЗАГЛАВИЕ:"
- Article #5678: Summary contains "Преработена статия"
- Article #9012: Content contains "НЕ пиши"

Run with --report to export full CSV
```

## Implementation

Uses `lib/utils/promptLeakageDetector.ts`

The PromptLeakageDetector class scans articles using regex patterns and categorizes leakage by location and severity.
