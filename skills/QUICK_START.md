# Quick Start Guide - Aftermint Skills

## 5 Essential Skills for NFT Marketplace Development

### 🔗 1. Web3 Contract Analyzer
**When to use**: Validating contracts, testing blockchain interactions
```bash
# Analyze a new NFT collection contract
"Check if contract 0x... implements ERC721 and ERC2981"

# Test contract functions
"Test the totalSupply and tokenURI functions for this contract"

# Verify multi-network support
"Validate this Ethereum contract works with our metadata service"
```

### 🔒 2. Smart Contract Security
**When to use**: Security audits, vulnerability detection
```bash
# Full security audit
"Audit the marketplace contract for vulnerabilities"

# Specific vulnerability check
"Check for reentrancy issues in the buyNFT function"

# Pre-deployment review
"Review this new collection contract before integration"
```

### 🎨 3. NFT UI Builder
**When to use**: Building React components, UI/UX improvements
```bash
# Create new components
"Build an NFT gallery component with filtering"

# Optimize existing components
"Improve performance of the NFT card component"

# Accessibility improvements
"Make the marketplace filters keyboard accessible"
```

### 🔄 4. Marketplace Workflows
**When to use**: Implementing trading flows, transactions
```bash
# Implement features
"Add bulk listing functionality"

# Fix transaction flows
"Debug the offer acceptance workflow"

# Handle errors
"Improve error handling in the purchase flow"
```

### 📊 5. Blockchain Analytics
**When to use**: Data analysis, rarity calculations, insights
```bash
# Calculate rarity
"Generate rarity scores for the new collection"

# Analyze collections
"Calculate floor price and volume trends"

# Build dashboards
"Create a collection analytics dashboard"
```

## Common Tasks & Skill Combinations

### ✅ Adding a New Collection
1. **Web3 Analyzer**: Validate contract → ERC standards
2. **Security**: Audit for vulnerabilities
3. **Analytics**: Calculate rarity & traits
4. **UI Builder**: Create collection page
5. **Workflows**: Implement trading flows

### ✅ Fixing a Bug
1. **Workflows**: Identify the flow issue
2. **Web3 Analyzer**: Test contract calls
3. **UI Builder**: Fix UI/error messages
4. **Security**: Check if bug is security-related

### ✅ Performance Optimization
1. **Analytics**: Identify slow queries
2. **UI Builder**: Optimize component rendering
3. **Web3 Analyzer**: Reduce RPC calls
4. **Workflows**: Implement caching

### ✅ Building a Feature
1. **Workflows**: Design the flow
2. **UI Builder**: Create components
3. **Web3 Analyzer**: Integrate contracts
4. **Security**: Security review
5. **Analytics**: Add tracking/metrics

## Skill Invocation Methods

### Method 1: Direct Skill Command
```bash
/skill web3-contract-analyzer
"Your specific task here"
```

### Method 2: In Conversation
```
"Using the blockchain-analytics skill, calculate rarity for collection X"
```

### Method 3: Multi-Skill Request
```
"Use web3-contract-analyzer to validate the contract, then use smart-contract-security to audit it"
```

## Skill Cheat Sheet

| Need to... | Use Skill | Quick Command |
|------------|-----------|---------------|
| Test contract | web3-contract-analyzer | "Test contract at 0x..." |
| Security audit | smart-contract-security | "Audit contract for vulnerabilities" |
| Build UI | nft-ui-builder | "Create component for..." |
| Trading flow | nft-marketplace-workflows | "Implement listing flow" |
| Calculate stats | blockchain-analytics | "Calculate collection stats" |
| Check ERC721 | web3-contract-analyzer | "Verify ERC721 compliance" |
| Find vulnerabilities | smart-contract-security | "Check for reentrancy" |
| Optimize render | nft-ui-builder | "Optimize NFT card rendering" |
| Handle errors | nft-marketplace-workflows | "Improve error handling" |
| Rarity scores | blockchain-analytics | "Calculate rarity" |

## Emergency Quick Reference

### 🚨 Contract Not Working
→ **web3-contract-analyzer** + **smart-contract-security**

### 🚨 Transaction Failing
→ **nft-marketplace-workflows** + **web3-contract-analyzer**

### 🚨 UI Performance Issues
→ **nft-ui-builder** + **blockchain-analytics**

### 🚨 Security Concern
→ **smart-contract-security** immediately

### 🚨 Data Loading Slow
→ **blockchain-analytics** + **web3-contract-analyzer**

## Pro Tips

1. **Always validate contracts** before integrating → web3-contract-analyzer
2. **Security first** → Review with smart-contract-security before deployment
3. **Performance matters** → Use blockchain-analytics for caching strategies
4. **User experience** → nft-ui-builder for loading states and errors
5. **Test flows** → Use nft-marketplace-workflows for E2E testing

## Next Steps

1. **Read the full skill docs** in this directory for detailed guidance
2. **Try combining skills** for complex tasks
3. **Refer to project CLAUDE.md** for project-specific context
4. **Check README.md** for comprehensive skill documentation

## Support

- **Skill Documentation**: `.claude/skills/*.md`
- **Project Context**: `CLAUDE.md` (root and project-specific)
- **Tech Stack**: Next.js 14, React 18, TypeScript, Tailwind, wagmi, viem
- **Network**: Base (Chain ID: 32323)

---

**Remember**: These skills are your expert assistants. Use them often and combine them for best results!
