# Claude AI Usage Guide for Skills & Agents

This document provides instructions for AI assistants (particularly Claude) on how to effectively use the skills and agents in this repository.

## Purpose

This repository contains specialized skills and agents that augment AI capabilities for software development tasks. Each skill provides domain expertise, best practices, and structured guidance for specific types of work.

## How to Use Skills

### 1. Skill Discovery

When a user requests help, first identify which skills are relevant:

**Analysis Questions:**
- What domain does this task belong to? (Web3, mobile, web, content, etc.)
- What type of work is required? (Development, review, optimization, analysis)
- Are there security or quality concerns?
- Does this require specialized domain knowledge?

### 2. Skill Selection

**Single Skill Tasks:**
```
User: "Can you review the UI design for my NFT marketplace?"
→ Use: design-review skill
```

**Multi-Skill Tasks:**
```
User: "Build and audit a new NFT smart contract"
→ Use:
  1. smart-contract-security (audit existing patterns)
  2. contract-auditor (implementation guidance)
  3. code-quality-enforcer agent (final review)
```

**Complex Workflows:**
```
User: "Launch a new NFT collection with full marketplace integration"
→ Use sequential skills:
  1. web3-contract-analyzer (validate contract)
  2. contract-security-auditor agent (security audit)
  3. nft-ui-builder (build interface)
  4. nft-marketplace-workflows (implement trading)
  5. code-quality-enforcer agent (final review)
```

### 3. Skill Application

**Read the skill content:**
- Load the relevant skill markdown file
- Understand the context, requirements, and best practices
- Note any project-specific considerations

**Apply the guidance:**
- Follow the skill's structured approach
- Reference examples and patterns provided
- Maintain consistency with skill standards
- Adapt guidance to the specific project context

**Combine multiple skills:**
- Apply skills in logical sequence
- Ensure consistency across skill boundaries
- Use agents to validate cross-skill implementations

## Skill Categories & When to Use

### Development Skills (Proactive Use)

Use these when implementing features:

- `nft-ui-builder` - Building any NFT-related UI components
- `nft-marketplace-workflows` - Implementing marketplace features
- `ios-app`, `ios-development` - iOS development tasks
- `telegram-development` - Telegram bot/mini app development
- `ui-ux-design` - Designing new interfaces

### Review Skills (On Request or Completion)

Use these when reviewing or auditing:

- `design-review` - After UI implementation
- `mobile-ui-audit` - For mobile interfaces
- `contract-auditor` - Before contract deployment
- `ux-reviewer` agent - After feature completion

### Quality & Security Skills (Always Check)

Use these for every production code:

- `code-quality-enforcer` agent - Always before committing
- `smart-contract-security` - Always for contracts
- `placeholder-detector` - Before deployment
- `prompt-leakage-detector` - For AI-integrated features

### Analysis Skills (As Needed)

Use these for insights and optimization:

- `blockchain-analytics` - For NFT collection analysis
- `performance-monitor` agent - When performance issues arise
- `trending-predictor` - For content strategy
- `seo-optimizer` - Before publishing content

### Content Skills (Publishing Workflows)

Use these for content creation:

- `content-quality` - Before publishing any content
- `fact-checker` - For factual claims
- `ai-quality-analyzer` - For AI-generated content
- `newsletter-generator` - For email campaigns

## Agents vs Skills

### Skills
- **What:** Domain expertise and guidance
- **When:** During implementation or review
- **How:** Reference and follow the skill guidelines
- **Example:** "Following the nft-ui-builder skill, I'll create a component with proper error states..."

### Agents
- **What:** Autonomous workflow executors
- **When:** For complex, multi-step processes
- **How:** Invoke agent and let it orchestrate the work
- **Example:** "I'll use the code-quality-enforcer agent to perform a comprehensive review..."

## Best Practices for AI Assistants

### 1. Always Announce Skill Usage

**Good:**
```
"I'll use the design-review skill to audit your marketplace interface.
Based on the skill guidelines, I'll check for:
- Design consistency
- Accessibility
- Mobile responsiveness
- User experience patterns"
```

**Bad:**
```
"Let me review your design."
(No mention of using specialized skill)
```

### 2. Apply Skill Standards

Each skill defines quality standards. Always:
- Follow the skill's checklist
- Apply the skill's best practices
- Use the skill's terminology
- Reference the skill's examples

### 3. Combine Skills Logically

**Sequential Application:**
```
1. contract-auditor (ensure contract is valid)
2. nft-marketplace-workflows (implement trading logic)
3. code-quality-enforcer (final quality check)
```

**Parallel Application:**
```
- design-review (UI audit)
- mobile-ui-audit (mobile-specific audit)
Both provide complementary perspectives
```

### 4. Adapt to Context

Skills may reference example projects for illustration. Always:
- Extract the general principles
- Adapt to the current project
- Maintain the skill's quality standards
- Apply domain best practices

### 5. Quality Gates

Always use quality-focused skills before:
- Committing code
- Deploying contracts
- Publishing content
- Launching features

## Skill Invocation Patterns

### Pattern 1: Direct Reference
```
"Using the {skill-name} skill, I'll..."
```

### Pattern 2: Implicit Application
```
"I'll review your smart contract for security vulnerabilities."
(Internally applying smart-contract-security skill)
```

### Pattern 3: Multi-Skill Workflow
```
"I'll use a combination of skills:
1. {skill-1} for {purpose}
2. {skill-2} for {purpose}
3. {agent-1} for final validation"
```

## Common Task → Skill Mappings

| User Request | Primary Skill(s) | Supporting Skills/Agents |
|--------------|------------------|-------------------------|
| "Build NFT marketplace" | nft-marketplace-workflows | nft-ui-builder, contract-auditor |
| "Review my code" | code-quality-enforcer | design-review, ux-reviewer |
| "Audit smart contract" | contract-auditor | smart-contract-security |
| "Optimize mobile app" | mobile-performance | mobile-ui-audit, ux-reviewer |
| "Create newsletter" | newsletter-generator | content-quality, seo-optimizer |
| "Check for placeholders" | placeholder-detector | code-quality-enforcer |
| "Build iOS app" | ios-development | mobile-performance, ux-reviewer |
| "Design UI" | ui-ux-design | design-review, mobile-ui-audit |
| "Analyze NFT collection" | blockchain-analytics | web3-contract-analyzer |
| "Fact-check article" | fact-checker | content-quality |

## Workflow Examples

### Example 1: New NFT Feature

```
User: "Add a bulk listing feature to the marketplace"

AI Response:
"I'll implement the bulk listing feature using the nft-marketplace-workflows skill.

Based on the skill guidelines, I'll:
1. Design the transaction batching logic
2. Implement approval handling
3. Add proper error recovery
4. Include gas optimization

[Implementation work...]

Now I'll use the code-quality-enforcer agent to review the implementation.

[Quality review...]

The feature is complete and passes quality checks."
```

### Example 2: Security Audit

```
User: "Is this contract safe to deploy?"

AI Response:
"I'll perform a comprehensive security audit using:
1. smart-contract-security skill - for vulnerability detection
2. contract-security-auditor agent - for systematic review

[Audit process...]

Security Analysis:
- Critical Issues: [findings]
- Recommendations: [suggestions]
- Security Score: [rating]

Based on the smart-contract-security skill standards, I recommend
addressing [X] before deployment."
```

### Example 3: Content Publishing

```
User: "Publish this article"

AI Response:
"Before publishing, I'll run it through our quality pipeline:

1. content-quality skill - quality assessment
2. fact-checker skill - verify claims
3. ai-quality-analyzer skill - check AI-generated content
4. seo-optimizer skill - optimize for discovery

[Quality checks...]

Quality Report:
- Content Score: 85/100 ✓
- Facts Verified: 12/12 ✓
- SEO Optimization: Complete ✓
- Ready to publish: YES"
```

## Error Handling

When skills identify issues:

1. **Clearly state the problem** - Reference skill standards
2. **Provide specific fixes** - Use skill best practices
3. **Explain the rationale** - Cite skill guidelines
4. **Verify the fix** - Re-apply skill validation

## Skill Maintenance

When using skills:

- **Check for conflicts** - Some skills may have overlapping guidance
- **Prioritize security** - Security skills always take precedence
- **Maintain standards** - Skills define minimum quality bars
- **Suggest improvements** - If you find skill gaps, note them

## Important Reminders

1. **Skills are expertise, not rules** - Adapt intelligently to context
2. **Quality is non-negotiable** - Always apply quality skills
3. **Security first** - Never skip security skills for production code
4. **User transparency** - Always explain which skills you're using
5. **Continuous learning** - Skills evolve, stay updated

## Anti-Patterns (What NOT to Do)

❌ Skip security skills for "quick" deployments
❌ Ignore skill standards because "it works"
❌ Apply only one skill when multiple are relevant
❌ Use skills without understanding the context
❌ Forget to validate with quality agents
❌ Leave placeholders or TODOs in production code

## Skills as Context Enhancement

Think of skills as:
- **Memory aids** - Comprehensive domain knowledge
- **Quality gates** - Standards enforcement
- **Best practices** - Proven patterns and approaches
- **Consistency tools** - Maintain standards across projects

## Final Guidelines

1. **Be proactive** - Suggest relevant skills before users ask
2. **Be thorough** - Apply all relevant skills for the task
3. **Be transparent** - Explain skill usage and findings
4. **Be consistent** - Follow skill standards uniformly
5. **Be helpful** - Use skills to provide better assistance

---

**Remember:** These skills exist to make you a more effective assistant. Use them liberally, combine them wisely, and always maintain the quality standards they define.
