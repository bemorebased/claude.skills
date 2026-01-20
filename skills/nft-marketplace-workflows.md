# NFT Marketplace Workflows Skill

You are an expert in NFT marketplace operations, user flows, and transaction workflows for the Aftermint platform.

## Your Role

Guide implementation and troubleshooting of end-to-end marketplace workflows including listing, buying, offering, and collection management.

## Core Marketplace Workflows

### 1. NFT Listing Flow

**User Journey**:
1. User owns NFT → Views NFT details
2. Clicks "List for Sale"
3. Sets listing price in ETH/BASED
4. Approves marketplace contract (if needed)
5. Creates listing transaction
6. Confirms success → NFT shown as listed

**Technical Implementation**:

```typescript
// Step 1: Check approval
const isApproved = await checkApproval(nftContract, marketplaceAddress, tokenId);

if (!isApproved) {
  // Step 2: Request approval
  const approveTx = await setApprovalForAll(nftContract, marketplaceAddress, true);
  await approveTx.wait();
}

// Step 3: Create listing
const listingTx = await marketplaceContract.createListing(
  nftContract,
  tokenId,
  priceInWei,
  duration
);

await listingTx.wait();
```

**Error Handling**:
- User rejects approval → Show "Approval required" message
- User rejects listing → Return to NFT page
- Insufficient gas → Show gas estimation error
- Already listed → Show existing listing
- Not owner → Show ownership error

**State Management**:
```typescript
enum ListingState {
  IDLE,
  CHECKING_APPROVAL,
  REQUESTING_APPROVAL,
  APPROVING,
  CREATING_LISTING,
  CONFIRMING,
  SUCCESS,
  ERROR
}
```

### 2. NFT Purchase Flow

**User Journey**:
1. User browses marketplace → Finds listed NFT
2. Views listing details (price, seller, etc.)
3. Clicks "Buy Now"
4. Reviews transaction details
5. Confirms purchase
6. Waits for confirmation
7. Success → NFT transferred to buyer

**Technical Implementation**:

```typescript
// Step 1: Validate listing
const listing = await marketplaceContract.getListing(listingId);
if (!listing.active) throw new Error("Listing no longer active");

// Step 2: Check buyer balance
const balance = await provider.getBalance(buyerAddress);
if (balance.lt(listing.price)) throw new Error("Insufficient balance");

// Step 3: Execute purchase
const purchaseTx = await marketplaceContract.buyNFT(
  listingId,
  { value: listing.price }
);

await purchaseTx.wait();

// Step 4: Update UI
toast.success("NFT purchased successfully!");
```

**Error Handling**:
- Listing sold/cancelled → "This NFT is no longer available"
- Insufficient funds → "Insufficient balance" + current balance
- User rejects → Return to listing
- Network error → Retry option
- Transaction reverted → Show revert reason

### 3. Make Offer Flow

**User Journey**:
1. User views unlisted NFT or wants to bid lower
2. Clicks "Make Offer"
3. Sets offer amount and expiration
4. Approves payment token (if needed)
5. Creates offer
6. Waits for seller acceptance

**Technical Implementation**:

```typescript
// Step 1: Check token approval (if using ERC20)
if (paymentToken !== NATIVE_TOKEN) {
  const allowance = await tokenContract.allowance(buyer, marketplace);
  if (allowance.lt(offerAmount)) {
    const approveTx = await tokenContract.approve(marketplace, offerAmount);
    await approveTx.wait();
  }
}

// Step 2: Create offer
const offerTx = await marketplaceContract.makeOffer(
  nftContract,
  tokenId,
  offerAmount,
  paymentToken,
  expirationTime
);

await offerTx.wait();
```

**Offer States**:
- Pending: Waiting for seller response
- Accepted: Seller accepted, NFT transferred
- Rejected: Seller declined
- Expired: Time limit passed
- Cancelled: Buyer cancelled

### 4. Accept Offer Flow

**User Journey** (Seller):
1. Seller receives notification of offer
2. Reviews offer details
3. Compares with other offers/listing price
4. Decides to accept
5. Approves marketplace (if needed)
6. Accepts offer transaction
7. NFT transferred, payment received

**Technical Implementation**:

```typescript
// Step 1: Validate offer
const offer = await marketplaceContract.getOffer(offerId);
if (offer.expired) throw new Error("Offer expired");

// Step 2: Check approval
const isApproved = await nftContract.isApprovedForAll(seller, marketplace);
if (!isApproved) {
  const approveTx = await nftContract.setApprovalForAll(marketplace, true);
  await approveTx.wait();
}

// Step 3: Accept offer
const acceptTx = await marketplaceContract.acceptOffer(offerId);
await acceptTx.wait();
```

### 5. Cancel Listing Flow

**User Journey**:
1. Seller views their listed NFT
2. Clicks "Cancel Listing"
3. Confirms cancellation
4. Transaction confirmed
5. NFT unlisted

**Technical Implementation**:

```typescript
const cancelTx = await marketplaceContract.cancelListing(listingId);
await cancelTx.wait();
```

**Important**:
- Seller must still own NFT
- No pending accepted offers
- Gas cost for cancellation

### 6. Collection Discovery Flow

**User Journey**:
1. User lands on homepage
2. Browses featured/trending collections
3. Clicks collection card
4. Views collection page with filters
5. Applies filters (traits, price, rarity)
6. Sorts results
7. Clicks NFT to view details

**Technical Implementation**:

```typescript
// Step 1: Fetch collection data
const collection = await getCollection(collectionAddress);
const stats = await getCollectionStats(collectionAddress);

// Step 2: Fetch NFTs with filters
const nfts = await getNFTsForCollection(collectionAddress, {
  traits: selectedTraits,
  priceRange: [minPrice, maxPrice],
  rarityTiers: selectedTiers,
  listedOnly: showListedOnly,
  sortBy: sortOption, // price, rarity, tokenId
  page: currentPage,
  limit: 50
});

// Step 3: Calculate rarity (if not cached)
const rarityData = await getRarityForCollection(collectionAddress);
```

**Performance Considerations**:
- Paginate results (50 NFTs per page)
- Cache rarity data in localStorage
- Lazy load NFT images
- Debounce filter changes
- Show loading skeletons

## Advanced Workflows

### 7. Bulk Listing Flow

**User Journey**:
1. User selects multiple owned NFTs
2. Sets price (individual or bulk)
3. Approves marketplace once
4. Batch create listings
5. Shows progress for each

**Benefits**:
- Single approval for all
- Gas savings vs individual listings
- Better UX for sellers

### 8. Collection Offers Flow

**User Journey**:
1. Buyer makes offer on entire collection
2. Sets price per NFT
3. Any seller can accept for their NFT
4. Buyer receives random NFT from collection

**Use Case**:
- Floor price buying
- Collection sweeping
- Bulk acquisition

### 9. Auction Flow (Future)

**Phases**:
1. Auction creation
2. Bidding period
3. Bid increments validation
4. Auction end/settlement
5. NFT transfer to winner

### 10. Royalty Payment Flow

**Implementation**:
```typescript
// Step 1: Check royalty support (ERC2981)
const hasRoyalty = await nftContract.supportsInterface('0x2a55205a');

if (hasRoyalty) {
  // Step 2: Get royalty info
  const [receiver, amount] = await nftContract.royaltyInfo(tokenId, salePrice);

  // Step 3: Split payment
  // - Royalty to creator
  // - Marketplace fee
  // - Remaining to seller
}
```

## State Management Patterns

### Marketplace Store (Zustand)

```typescript
interface MarketplaceStore {
  // Listings
  listings: Map<string, Listing>;
  addListing: (listing: Listing) => void;
  removeListing: (listingId: string) => void;

  // Offers
  offers: Map<string, Offer>;
  addOffer: (offer: Offer) => void;
  updateOfferStatus: (offerId: string, status: OfferStatus) => void;

  // Filters
  filters: FilterState;
  setFilters: (filters: FilterState) => void;

  // Transaction state
  pendingTx: Transaction | null;
  setPendingTx: (tx: Transaction | null) => void;
}
```

## Error Recovery Patterns

### Transaction Failed

```typescript
try {
  const tx = await contract.method(...args);
  await tx.wait();
} catch (error) {
  if (error.code === 'ACTION_REJECTED') {
    // User rejected - no retry
    toast.error('Transaction cancelled');
  } else if (error.code === 'INSUFFICIENT_FUNDS') {
    // Show balance error
    toast.error(`Insufficient funds. Need ${formatEther(required)}`);
  } else if (error.code === 'UNPREDICTABLE_GAS_LIMIT') {
    // Transaction would fail - show reason
    const reason = parseRevertReason(error);
    toast.error(`Transaction would fail: ${reason}`);
  } else {
    // Unknown error - offer retry
    toast.error('Transaction failed', {
      action: { label: 'Retry', onClick: retry }
    });
  }
}
```

### Network Issues

```typescript
// Retry with exponential backoff
async function fetchWithRetry(fn: () => Promise<any>, maxRetries = 3) {
  for (let i = 0; i < maxRetries; i++) {
    try {
      return await fn();
    } catch (error) {
      if (i === maxRetries - 1) throw error;
      await delay(2 ** i * 1000); // 1s, 2s, 4s
    }
  }
}
```

### IPFS Gateway Failures

```typescript
const IPFS_GATEWAYS = [
  'https://ipfs.io/ipfs/',
  'https://gateway.pinata.cloud/ipfs/',
  'https://cloudflare-ipfs.com/ipfs/',
  'https://gateway.ipfs.io/ipfs/'
];

async function fetchFromIPFS(hash: string) {
  for (const gateway of IPFS_GATEWAYS) {
    try {
      const response = await fetch(gateway + hash, { timeout: 5000 });
      if (response.ok) return await response.json();
    } catch (error) {
      console.warn(`Gateway ${gateway} failed, trying next...`);
    }
  }
  throw new Error('All IPFS gateways failed');
}
```

## Data Synchronization

### Real-time Updates

**Event Listening**:
```typescript
// Listen for marketplace events
marketplaceContract.on('ListingCreated', (listingId, seller, nftAddress, tokenId, price) => {
  // Update UI with new listing
  store.addListing({ listingId, seller, nftAddress, tokenId, price });
});

marketplaceContract.on('NFTSold', (listingId, buyer, price) => {
  // Remove sold listing
  store.removeListing(listingId);
  toast.success('NFT sold!');
});
```

**Polling Strategy**:
```typescript
// Poll for updates every 30 seconds
useEffect(() => {
  const interval = setInterval(async () => {
    await refreshListings();
    await refreshOffers();
  }, 30000);

  return () => clearInterval(interval);
}, []);
```

### Cache Strategy

**Rarity Data**:
- Cache in localStorage
- Version by collection + timestamp
- TTL: 24 hours for static, 1 hour for dynamic

**Metadata**:
- Cache in memory (Map)
- Cache in localStorage for favorites
- TTL: 1 hour

**Listings**:
- Don't cache (always fresh from chain)
- Or cache with 30s TTL

## User Notifications

### Notification Types

1. **Transaction Status**
   - Pending: "Transaction submitted..."
   - Success: "NFT purchased successfully!"
   - Failed: "Transaction failed: [reason]"

2. **Offer Updates**
   - New offer received
   - Offer accepted
   - Offer expired
   - Counter-offer received

3. **Listing Updates**
   - NFT listed successfully
   - Listing sold
   - Listing expired

4. **Wallet Events**
   - Connected
   - Disconnected
   - Network switched
   - Account changed

### Toast Patterns

```typescript
// Success with action
toast.success('NFT listed!', {
  action: {
    label: 'View',
    onClick: () => router.push(`/nft/${address}/${tokenId}`)
  }
});

// Error with retry
toast.error('Failed to fetch metadata', {
  action: {
    label: 'Retry',
    onClick: retry
  }
});

// Loading
const toastId = toast.loading('Listing NFT...');
// Later
toast.success('Listed!', { id: toastId });
```

## Analytics & Tracking

### User Actions to Track

1. Wallet connected/disconnected
2. NFT viewed
3. Listing created
4. Purchase completed
5. Offer made/accepted
6. Filters applied
7. Search performed
8. Collection viewed

### Performance Metrics

1. Time to load collection page
2. Time to load NFT metadata
3. Time to complete transaction
4. IPFS gateway response times
5. RPC call latency

## Best Practices

1. **Always validate on-chain data** - Don't trust cached/indexed data for critical operations
2. **Handle all error cases** - Network, wallet, contract, user rejection
3. **Provide clear feedback** - Loading states, success/error messages
4. **Optimize for mobile** - Touch targets, simplified flows
5. **Test with real data** - Use testnet with real transactions
6. **Monitor gas costs** - Alert users to high gas prices
7. **Implement retry logic** - Network calls can fail
8. **Cache aggressively** - But invalidate correctly
9. **Log errors properly** - Help debug issues
10. **Test edge cases** - Expired offers, sold listings, etc.

## Testing Workflows

### Manual Testing Checklist

- [ ] List NFT (with approval)
- [ ] List NFT (already approved)
- [ ] Cancel listing
- [ ] Buy listed NFT
- [ ] Make offer
- [ ] Accept offer (with approval)
- [ ] Cancel offer
- [ ] Reject transaction
- [ ] Switch networks mid-flow
- [ ] Disconnect wallet mid-flow

### Edge Cases

- NFT sold between view and purchase
- Offer expires during acceptance
- User doesn't own NFT during listing
- Insufficient balance during purchase
- Price changed between view and buy
- Multiple offers on same NFT
- Concurrent acceptances (race condition)

## Troubleshooting Guide

### Common Issues

**"Transaction would fail"**
- Check approval status
- Verify ownership
- Confirm listing is active
- Check balance

**"Insufficient funds"**
- Calculate total cost (price + gas)
- Show balance vs required
- Suggest smaller purchase

**"NFT not found"**
- Verify contract address
- Check token ID exists
- Validate network

**"Metadata failed to load"**
- Try alternate IPFS gateways
- Check tokenURI format
- Validate IPFS hash

**"Rarity not calculated"**
- Check collection in STATIC_COLLECTIONS
- Verify rarity data files exist
- Fall back to calculated rarity

Remember: Every transaction can fail. Plan for it.
