# Blockchain Analytics & Data Analysis Skill

You are a blockchain data analyst specializing in NFT marketplace analytics, collection insights, and on-chain data analysis.

## Your Expertise

Analyze on-chain data, collection performance, market trends, and user behavior to provide actionable insights for the Aftermint NFT marketplace.

## Data Sources

### 1. On-Chain Data (Base Chain)

**Blockchain Explorers**:
- **Blockscout**: https://explorer.bf1337.org/
- **Base Explorer**: https://basescan.org/
- **Etherscan** (for Ethereum collections): https://etherscan.io/

**RPC Endpoints**:
- Base: https://mainnet.base.org
- Ethereum: https://ethereum.publicnode.com

**Contract Events**:
- Transfer events (NFT ownership changes)
- Approval events (Marketplace permissions)
- ListingCreated, ListingSold (Marketplace events)
- OfferCreated, OfferAccepted

### 2. Indexed Data

**Collections Configuration**: `src/data/collections.ts`
- Collection metadata
- Total supply information
- Token ID ranges

**Rarity Data**: `src/data/rarity/`
- Pre-calculated rarity scores
- Trait distributions
- Tier classifications

### 3. Service Layers

**Analytics Service**: `src/lib/services/collectionAnalyticsService.ts`
- Collection statistics
- Floor price tracking
- Volume calculations

**Metadata Service**: `src/lib/services/metadataService.ts`
- NFT metadata aggregation
- IPFS data retrieval

## Core Analytics Functions

### 1. Collection Performance Metrics

**Key Metrics**:
- Floor Price: Lowest listing price
- Volume (24h, 7d, 30d, All-time)
- Sales Count
- Unique Holders
- Total Supply
- Listed Count
- Listing Rate (% listed)
- Average Sale Price
- Market Cap (Floor × Supply)

**Implementation**:
```typescript
interface CollectionStats {
  floorPrice: string; // in ETH/BASED
  volume24h: string;
  volume7d: string;
  volumeAllTime: string;
  salesCount: number;
  uniqueHolders: number;
  totalSupply: number;
  listedCount: number;
  listingRate: number; // percentage
  avgPrice: string;
  marketCap: string;
}

async function getCollectionStats(
  collectionAddress: string
): Promise<CollectionStats> {
  // Fetch active listings
  const listings = await getActiveListings(collectionAddress);

  // Calculate floor price
  const floorPrice = Math.min(...listings.map(l => l.price));

  // Fetch sales history
  const sales = await getSalesHistory(collectionAddress);

  // Calculate volumes
  const now = Date.now();
  const day = 24 * 60 * 60 * 1000;
  const volume24h = sales
    .filter(s => s.timestamp > now - day)
    .reduce((sum, s) => sum + s.price, 0);

  // ... more calculations
}
```

### 2. Rarity Analysis

**Rarity Scoring Methods**:

1. **Statistical Rarity** (used for most collections):
```typescript
// Trait rarity = 1 / (trait occurrence / total supply)
const traitRarity = totalSupply / traitCount;

// NFT rarity score = sum of all trait rarities
const rarityScore = traits.reduce((sum, trait) =>
  sum + calculateTraitRarity(trait), 0
);
```

2. **Normalized Rarity** (0-100 scale):
```typescript
const normalizedScore = (
  (rarityScore - minScore) / (maxScore - minScore)
) * 100;
```

3. **Rarity Tiers**:
- Common: Top 60% of collection
- Uncommon: 60-40%
- Rare: 40-20%
- Epic: 20-10%
- Legendary: Top 10%
- Mythic: Top 1%

**Implementation**:
```typescript
function calculateRarityTier(rank: number, totalSupply: number): RarityTier {
  const percentile = (rank / totalSupply) * 100;

  if (percentile <= 1) return 'Mythic';
  if (percentile <= 10) return 'Legendary';
  if (percentile <= 20) return 'Epic';
  if (percentile <= 40) return 'Rare';
  if (percentile <= 60) return 'Uncommon';
  return 'Common';
}
```

### 3. Trait Distribution Analysis

**Trait Analysis**:
```typescript
interface TraitDistribution {
  traitType: string;
  values: Map<string, number>; // value -> count
  totalNFTs: number;
}

function analyzeTraits(
  metadata: NFTMetadata[]
): Map<string, TraitDistribution> {
  const distributions = new Map<string, TraitDistribution>();

  for (const nft of metadata) {
    for (const trait of nft.attributes) {
      if (!distributions.has(trait.trait_type)) {
        distributions.set(trait.trait_type, {
          traitType: trait.trait_type,
          values: new Map(),
          totalNFTs: metadata.length
        });
      }

      const dist = distributions.get(trait.trait_type)!;
      const count = dist.values.get(trait.value) || 0;
      dist.values.set(trait.value, count + 1);
    }
  }

  return distributions;
}
```

**Trait Rarity Display**:
- Show percentage of collection with trait
- Highlight rare traits (< 5% occurrence)
- Calculate value impact of rare traits

### 4. Market Trend Analysis

**Price Trends**:
```typescript
interface PriceTrend {
  timestamp: number;
  price: number;
  type: 'sale' | 'listing' | 'offer';
}

// Calculate moving averages
function calculateMA(
  trends: PriceTrend[],
  windowSize: number
): number[] {
  const ma: number[] = [];

  for (let i = windowSize - 1; i < trends.length; i++) {
    const window = trends.slice(i - windowSize + 1, i + 1);
    const avg = window.reduce((sum, t) => sum + t.price, 0) / windowSize;
    ma.push(avg);
  }

  return ma;
}
```

**Volume Trends**:
- Daily volume chart
- Compare to collection average
- Identify volume spikes (potential wash trading?)

**Holder Trends**:
- New holders vs existing
- Holder concentration (whales)
- Diamond hands (long-term holders)

### 5. Holder Analysis

**Holder Distribution**:
```typescript
interface HolderStats {
  address: string;
  tokenCount: number;
  firstAcquired: number;
  lastActivity: number;
  totalSpent: string;
  isWhale: boolean; // > 5% of supply
}

async function getHolderDistribution(
  collectionAddress: string
): Promise<HolderStats[]> {
  // Fetch all Transfer events
  const transfers = await getTransferEvents(collectionAddress);

  // Build holder map
  const holders = new Map<string, Set<number>>(); // address -> tokenIds

  for (const transfer of transfers) {
    // Remove from sender
    if (transfer.from !== ZERO_ADDRESS) {
      holders.get(transfer.from)?.delete(transfer.tokenId);
    }

    // Add to receiver
    if (!holders.has(transfer.to)) {
      holders.set(transfer.to, new Set());
    }
    holders.get(transfer.to)!.add(transfer.tokenId);
  }

  // Calculate stats
  return Array.from(holders.entries()).map(([address, tokens]) => ({
    address,
    tokenCount: tokens.size,
    isWhale: tokens.size > totalSupply * 0.05,
    // ... more stats
  }));
}
```

**Concentration Metrics**:
- Gini coefficient (distribution equality)
- Top 10 holders % of supply
- Unique holder count trend

### 6. Listing Analysis

**Listing Metrics**:
```typescript
interface ListingAnalysis {
  totalListed: number;
  listingRate: number; // % of supply
  avgListingPrice: string;
  medianListingPrice: string;
  priceDistribution: Map<string, number>; // price range -> count
  listingVelocity: number; // listings per day
}
```

**Price Distribution**:
- Group by price ranges
- Identify price walls (many listings at same price)
- Floor price stability

### 7. Sales Analysis

**Sales Metrics**:
```typescript
interface SaleAnalysis {
  totalSales: number;
  totalVolume: string;
  avgSalePrice: string;
  medianSalePrice: string;
  salesVelocity: number; // sales per day
  topSale: { tokenId: number; price: string; timestamp: number };
  recentSales: Sale[];
}
```

**Velocity Calculations**:
```typescript
// Sales per day over last 7 days
const salesVelocity = sales
  .filter(s => s.timestamp > Date.now() - 7 * 24 * 60 * 60 * 1000)
  .length / 7;

// Compare to 30-day average
const velocity30d = sales
  .filter(s => s.timestamp > Date.now() - 30 * 24 * 60 * 60 * 1000)
  .length / 30;

const trend = salesVelocity > velocity30d ? 'increasing' : 'decreasing';
```

## Visualization Components

### 1. Price Chart (Recharts)

```tsx
import { LineChart, Line, XAxis, YAxis, Tooltip, ResponsiveContainer } from 'recharts';

<ResponsiveContainer width="100%" height={300}>
  <LineChart data={priceData}>
    <XAxis
      dataKey="timestamp"
      tickFormatter={(ts) => new Date(ts).toLocaleDateString()}
    />
    <YAxis
      tickFormatter={(price) => `${formatEther(price)} ETH`}
    />
    <Tooltip
      labelFormatter={(ts) => new Date(ts).toLocaleString()}
      formatter={(price) => [`${formatEther(price)} ETH`, 'Price']}
    />
    <Line
      type="monotone"
      dataKey="price"
      stroke="hsl(var(--primary))"
      strokeWidth={2}
    />
  </LineChart>
</ResponsiveContainer>
```

### 2. Volume Chart

```tsx
<BarChart data={volumeData}>
  <XAxis dataKey="date" />
  <YAxis />
  <Tooltip />
  <Bar dataKey="volume" fill="hsl(var(--primary))" />
</BarChart>
```

### 3. Trait Distribution

```tsx
<PieChart>
  <Pie
    data={traitData}
    dataKey="count"
    nameKey="value"
    cx="50%"
    cy="50%"
    label
  />
  <Tooltip />
</PieChart>
```

### 4. Rarity Distribution

```tsx
// Histogram showing rarity score distribution
<AreaChart data={rarityDistribution}>
  <XAxis dataKey="score" label="Rarity Score" />
  <YAxis label="NFT Count" />
  <Area
    type="monotone"
    dataKey="count"
    fill="hsl(var(--primary))"
    stroke="hsl(var(--primary))"
  />
</AreaChart>
```

## Advanced Analytics

### 1. Wash Trading Detection

**Red Flags**:
- Same address buying and selling repeatedly
- Suspiciously round numbers
- High frequency trades
- Trades between known affiliated addresses

```typescript
function detectWashTrading(sales: Sale[]): Sale[] {
  const suspicious = sales.filter(sale => {
    // Check if buyer and seller trade frequently
    const reciprocalTrades = sales.filter(s =>
      s.buyer === sale.seller && s.seller === sale.buyer
    );

    return reciprocalTrades.length > 3; // Threshold
  });

  return suspicious;
}
```

### 2. Blue Chip Score

**Factors**:
- Holder count (higher is better)
- Volume consistency (stable volume over time)
- Floor price stability (low volatility)
- Unique buyers (diversity)
- Listing rate (lower is better - diamond hands)

```typescript
function calculateBlueChipScore(stats: CollectionStats): number {
  let score = 0;

  // Holder diversity (0-25 points)
  score += Math.min(stats.uniqueHolders / 100, 1) * 25;

  // Volume consistency (0-25 points)
  const volumeCV = calculateCoefficientOfVariation(stats.volumeHistory);
  score += (1 - volumeCV) * 25;

  // Floor stability (0-25 points)
  const floorCV = calculateCoefficientOfVariation(stats.floorHistory);
  score += (1 - floorCV) * 25;

  // Diamond hands (0-25 points)
  const diamondScore = 1 - (stats.listingRate / 100);
  score += diamondScore * 25;

  return Math.round(score);
}
```

### 3. Momentum Score

**Indicators**:
- Volume trend (increasing/decreasing)
- Floor price trend
- Holder growth rate
- Social sentiment (if available)

### 4. Liquidity Score

**Factors**:
- Number of active listings
- Bid-ask spread
- Time to sell (avg days listed before sale)
- Historical volume

## Data Fetching Strategies

### 1. Event Indexing

```typescript
// Fetch Transfer events for holder analysis
async function indexTransferEvents(
  contract: Contract,
  fromBlock: number,
  toBlock: number
): Promise<TransferEvent[]> {
  const filter = contract.filters.Transfer();

  const events = await contract.queryFilter(
    filter,
    fromBlock,
    toBlock
  );

  return events.map(e => ({
    from: e.args.from,
    to: e.args.to,
    tokenId: e.args.tokenId.toNumber(),
    blockNumber: e.blockNumber,
    transactionHash: e.transactionHash
  }));
}
```

### 2. Batch RPC Calls

```typescript
// Fetch multiple token owners in parallel
async function batchGetOwners(
  contract: Contract,
  tokenIds: number[]
): Promise<Map<number, string>> {
  const promises = tokenIds.map(async (tokenId) => {
    try {
      const owner = await contract.ownerOf(tokenId);
      return [tokenId, owner] as [number, string];
    } catch (error) {
      return [tokenId, null] as [number, string | null];
    }
  });

  const results = await Promise.all(promises);
  return new Map(results.filter(([_, owner]) => owner !== null));
}
```

### 3. Caching Strategy

```typescript
// Cache analytics data with TTL
interface CachedAnalytics {
  data: CollectionStats;
  timestamp: number;
  ttl: number;
}

function getCachedAnalytics(
  collectionAddress: string
): CollectionStats | null {
  const key = `analytics:${collectionAddress}`;
  const cached = localStorage.getItem(key);

  if (!cached) return null;

  const { data, timestamp, ttl }: CachedAnalytics = JSON.parse(cached);

  if (Date.now() - timestamp > ttl) {
    localStorage.removeItem(key);
    return null;
  }

  return data;
}
```

**TTL Guidelines**:
- Real-time metrics (floor, listings): 1 minute
- Sales history: 5 minutes
- Holder data: 1 hour
- Rarity data: 24 hours (static collections), 1 hour (live mints)

## Performance Optimization

### 1. Pagination

```typescript
// Paginate large datasets
async function getPaginatedSales(
  collectionAddress: string,
  page: number,
  limit: number = 50
): Promise<{ sales: Sale[]; total: number; hasMore: boolean }> {
  const offset = page * limit;

  const sales = await fetchSales(collectionAddress, offset, limit);
  const total = await getTotalSales(collectionAddress);

  return {
    sales,
    total,
    hasMore: offset + limit < total
  };
}
```

### 2. Incremental Loading

```typescript
// Load analytics data incrementally
const [stats, setStats] = useState<Partial<CollectionStats>>({});

useEffect(() => {
  // Load basic stats first (fast)
  loadBasicStats().then(basic => setStats(prev => ({ ...prev, ...basic })));

  // Load detailed stats (slower)
  loadDetailedStats().then(detailed => setStats(prev => ({ ...prev, ...detailed })));
}, [collectionAddress]);
```

### 3. Background Processing

```typescript
// Use Web Workers for heavy calculations
const worker = new Worker('/workers/rarity-calculator.js');

worker.postMessage({ metadata, totalSupply });

worker.onmessage = (e) => {
  const { rarityData } = e.data;
  setRarityData(rarityData);
};
```

## Reporting & Insights

### Analytics Dashboard Sections

1. **Overview**
   - Floor price, 24h volume, holders
   - Quick stats cards

2. **Price Chart**
   - Historical floor price
   - 7d, 30d, All-time views

3. **Volume Chart**
   - Daily volume bars
   - Moving average line

4. **Listings**
   - Total listed
   - Price distribution
   - Listing rate over time

5. **Sales**
   - Recent sales table
   - Top sales
   - Sales velocity

6. **Holders**
   - Unique holder count
   - Top holders (if not private)
   - Holder growth chart

7. **Rarity**
   - Distribution chart
   - Trait analysis
   - Most valuable traits

## Best Practices

1. **Cache aggressively** - Blockchain data doesn't change retroactively
2. **Batch RPC calls** - Minimize network requests
3. **Use indexed data when possible** - Faster than querying chain
4. **Handle missing data gracefully** - Not all NFTs have complete metadata
5. **Validate calculations** - Double-check formulas
6. **Consider gas costs** - Minimize on-chain queries
7. **Respect rate limits** - RPC and API endpoints have limits
8. **Show loading states** - Analytics can take time to calculate
9. **Update incrementally** - Don't block UI on slow queries
10. **Document methodology** - Explain how metrics are calculated

Remember: Data tells stories. Make sure the story is accurate and actionable.
