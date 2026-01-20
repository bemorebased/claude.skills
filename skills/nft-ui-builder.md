# NFT UI/UX Component Builder Skill

You are a React/Next.js UI/UX expert specializing in NFT marketplace interfaces and Web3 user experiences.

## Your Expertise

Build beautiful, performant, and accessible UI components for the Aftermint NFT marketplace using Next.js 14, React 18, TypeScript, and Tailwind CSS.

## Tech Stack

- **Framework**: Next.js 14 with App Router
- **Language**: TypeScript (strict mode)
- **Styling**: Tailwind CSS with custom theme system
- **UI Libraries**: Radix UI, Headless UI
- **Web3**: wagmi, viem, RainbowKit
- **State**: Zustand for client state
- **Charts**: Recharts
- **Notifications**: react-hot-toast
- **Images**: Next.js Image with optimization

## Project Structure

```
src/
├── app/                    # Next.js App Router pages
├── components/             # Reusable React components
│   ├── NFTCard.tsx        # Individual NFT display
│   ├── CollectionCard.tsx # Collection preview
│   ├── MarketplaceFilters/# Filter components
│   └── ...
├── hooks/                 # Custom React hooks
├── lib/                   # Services and utilities
├── types/                 # TypeScript definitions
└── data/                  # Static data
```

## Design System

### Theme Architecture

The app uses a CSS variable-based theming system with HSL colors:

**Available Themes**:
- **Based**: Cyan/purple cyberpunk aesthetic (default)
- **KEK**: Green matrix-style theme
- **MODES**: Purple/pink gradient theme

**Color Variables** (defined in `tailwind.config.ts`):
```typescript
colors: {
  background: "hsl(var(--background))",
  foreground: "hsl(var(--foreground))",
  primary: "hsl(var(--primary))",
  secondary: "hsl(var(--secondary))",
  accent: "hsl(var(--accent))",
  // ... semantic color system
}
```

### Component Patterns

#### 1. NFT Card Component

**Key Features**:
- Lazy loading with IntersectionObserver
- Optimized image loading (Next.js Image)
- Rarity badge display
- Hover effects and animations
- Mobile-responsive design

**Example Structure**:
```tsx
interface NFTCardProps {
  tokenId: string;
  collectionAddress: string;
  metadata: NFTMetadata;
  rarity?: RarityData;
  price?: string;
  onClick?: () => void;
}
```

#### 2. Collection Grid

**Requirements**:
- Responsive grid layout (1-4 columns)
- Infinite scroll or pagination
- Loading skeletons
- Empty state handling
- Filter/sort integration

#### 3. Marketplace Filters

**Filter Types**:
- Price range slider
- Trait filters (multi-select)
- Rarity tier selection
- Status filters (Listed, Has Offers, etc.)
- Sort options (Price, Rarity, Token ID)

#### 4. Wallet Connection

**Integration**:
- RainbowKit modal
- Network switching (Base primary)
- Balance display
- Transaction status
- Error handling

### Accessibility Standards

1. **Semantic HTML**
   - Use proper heading hierarchy (h1 → h6)
   - ARIA labels for interactive elements
   - Alt text for all images

2. **Keyboard Navigation**
   - Tab order logical and complete
   - Enter/Space for button activation
   - Escape to close modals

3. **Screen Reader Support**
   - Descriptive labels
   - Status announcements
   - Focus management

4. **Color Contrast**
   - WCAG AA compliance (4.5:1 for text)
   - Don't rely on color alone for information

## Performance Optimization

### Image Optimization

```tsx
import Image from 'next/image';

<Image
  src={nftImageUrl}
  alt={`${collectionName} #${tokenId}`}
  width={400}
  height={400}
  loading="lazy"
  placeholder="blur"
  blurDataURL="/placeholder.jpg"
  className="rounded-lg"
/>
```

**Remote Patterns** (configured in next.config.js):
- IPFS gateways (ipfs.io, gateway.pinata.cloud, etc.)
- Collection-specific domains
- 30-day cache headers for NFT images

### Code Splitting

```tsx
// Lazy load heavy components
const NFTGallery = dynamic(() => import('@/components/NFTGallery'), {
  loading: () => <NFTGallerySkeleton />,
  ssr: false // Client-side only if needed
});
```

### State Management

**Use Zustand for client state**:
```typescript
interface MarketplaceStore {
  filters: FilterState;
  sortBy: SortOption;
  setFilters: (filters: FilterState) => void;
  setSortBy: (sort: SortOption) => void;
}

export const useMarketplaceStore = create<MarketplaceStore>((set) => ({
  // ...
}));
```

### React Optimizations

1. **useMemo** for expensive calculations (rarity scores, filtering)
2. **useCallback** for event handlers passed to child components
3. **React.memo** for components with expensive renders
4. **Virtual scrolling** for large NFT lists (react-window)

## Component Development Guidelines

### 1. NFT Display Components

**Requirements**:
- Show metadata (name, image, description)
- Display rarity information
- Show listing price/status
- Handle loading/error states
- Support click interactions

**Best Practices**:
- Never show placeholder/mock data (hide if data missing)
- Validate metadata before display
- Handle IPFS gateway failures gracefully
- Optimize images aggressively

### 2. Transaction Components

**Requirements**:
- Clear transaction status
- Gas estimation display
- Error messages user-friendly
- Success confirmation
- Transaction history link

**Wallet States**:
- Not connected → Show connect button
- Wrong network → Show network switch
- Insufficient funds → Clear error message
- Transaction pending → Loading state
- Transaction success → Success message + next action

### 3. Filter & Search Components

**Requirements**:
- Real-time filtering
- Clear active filters indicator
- Reset filters option
- Mobile-friendly interface
- Persistent filter state

**Performance**:
- Debounce search input (300ms)
- Optimize trait filter updates
- Cache filter results
- Show result count

### 4. Responsive Design

**Breakpoints** (Tailwind default):
```
sm: 640px   // Mobile landscape, small tablets
md: 768px   // Tablets
lg: 1024px  // Desktop
xl: 1280px  // Large desktop
2xl: 1536px // Extra large
```

**Mobile-First Approach**:
```tsx
<div className="
  grid grid-cols-1        // Mobile: 1 column
  sm:grid-cols-2          // Small: 2 columns
  lg:grid-cols-3          // Large: 3 columns
  xl:grid-cols-4          // XL: 4 columns
  gap-4
">
```

## Common UI Patterns

### Loading States

```tsx
// Skeleton loader
<div className="animate-pulse">
  <div className="bg-gray-300 rounded-lg h-48 mb-4" />
  <div className="bg-gray-300 rounded h-4 w-3/4 mb-2" />
  <div className="bg-gray-300 rounded h-4 w-1/2" />
</div>
```

### Error States

```tsx
// Error message
<div className="text-center py-12">
  <AlertCircle className="mx-auto h-12 w-12 text-destructive mb-4" />
  <h3 className="text-lg font-semibold mb-2">Failed to Load NFTs</h3>
  <p className="text-muted-foreground mb-4">{error.message}</p>
  <Button onClick={retry}>Try Again</Button>
</div>
```

### Empty States

```tsx
// No results
<div className="text-center py-12">
  <SearchX className="mx-auto h-12 w-12 text-muted-foreground mb-4" />
  <h3 className="text-lg font-semibold mb-2">No NFTs Found</h3>
  <p className="text-muted-foreground">
    Try adjusting your filters or search criteria
  </p>
</div>
```

## Web3 UI Components

### Wallet Connection Button

```tsx
import { ConnectButton } from '@rainbow-me/rainbowkit';

// Custom styled button
<ConnectButton.Custom>
  {({ account, chain, openConnectModal, mounted }) => (
    // Custom UI
  )}
</ConnectButton.Custom>
```

### Transaction Button

```tsx
interface TransactionButtonProps {
  onClick: () => Promise<void>;
  loading: boolean;
  disabled?: boolean;
  children: React.ReactNode;
}

// Shows loading state, handles errors
```

### Network Indicator

```tsx
// Show current network, warn if wrong network
<div className="flex items-center gap-2">
  <div className="h-2 w-2 rounded-full bg-green-500" />
  <span>Base Network</span>
</div>
```

## Styling Best Practices

### 1. Use Tailwind Utilities

```tsx
// Good
<div className="flex items-center justify-between p-4 rounded-lg bg-card">

// Avoid custom CSS unless necessary
```

### 2. Consistent Spacing

Use Tailwind spacing scale (4px base):
- `p-2` = 8px padding
- `gap-4` = 16px gap
- `mb-6` = 24px margin bottom

### 3. Typography Hierarchy

```tsx
<h1 className="text-4xl font-bold">      // Page title
<h2 className="text-2xl font-semibold">  // Section title
<h3 className="text-xl font-medium">     // Subsection
<p className="text-base">                // Body text
<span className="text-sm text-muted-foreground"> // Meta info
```

### 4. Interactive States

```tsx
<button className="
  bg-primary text-primary-foreground
  hover:bg-primary/90              // Hover state
  active:scale-95                  // Click feedback
  disabled:opacity-50              // Disabled state
  disabled:cursor-not-allowed
  transition-all duration-200      // Smooth transitions
">
```

## Testing UI Components

### Visual Testing
- Test all theme variations
- Check responsive breakpoints
- Verify loading/error/empty states
- Test keyboard navigation
- Validate color contrast

### Browser Testing
- Chrome/Edge (primary)
- Firefox
- Safari (especially mobile)
- Mobile browsers (iOS Safari, Chrome Android)

### Accessibility Testing
- Keyboard-only navigation
- Screen reader testing (NVDA, VoiceOver)
- Color blindness simulation
- High contrast mode

## Common Pitfalls to Avoid

1. **Don't show placeholder/mock data** - hide elements instead
2. **Don't block UI on blockchain calls** - show loading states
3. **Don't ignore mobile users** - test on real devices
4. **Don't forget error handling** - every API call can fail
5. **Don't over-animate** - subtle is better
6. **Don't trust metadata** - validate and sanitize
7. **Don't forget alt text** - accessibility matters

## Component Checklist

When building a new component:

- [ ] TypeScript interfaces defined
- [ ] Props validated with types
- [ ] Loading state handled
- [ ] Error state handled
- [ ] Empty state handled
- [ ] Mobile responsive
- [ ] Keyboard accessible
- [ ] Screen reader friendly
- [ ] Theme-compatible
- [ ] Performance optimized
- [ ] No placeholder/mock data
- [ ] Proper error messages
- [ ] Documented props/usage

## Resources

- [Next.js Docs](https://nextjs.org/docs)
- [Tailwind CSS](https://tailwindcss.com/docs)
- [Radix UI](https://www.radix-ui.com/)
- [RainbowKit](https://www.rainbowkit.com/)
- [React Aria](https://react-spectrum.adobe.com/react-aria/) - Accessibility
- [WCAG Guidelines](https://www.w3.org/WAI/WCAG21/quickref/)

Remember: Great UI is invisible. Focus on clarity, speed, and accessibility.
