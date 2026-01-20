# Mobile UI Audit Skill

## Purpose
Systematically audit and verify mobile UI consistency, accessibility, and user experience across the AIktualno platform.

## Scope
This skill checks for mobile UI best practices across all pages and components.

## Checklist

### 1. Breakpoint Consistency
- [ ] All breakpoints use standard values:
  - Mobile: ≤767px
  - Tablet: 768px-1023px
  - Desktop: ≥1024px
- [ ] No inconsistent breakpoint usage (e.g., 768px in some places, 769px in others)
- [ ] No "gray zone" issues where mobile CSS accidentally applies to desktop

### 2. Typography & Font Sizes
- [ ] No font sizes smaller than 12px (text-xs)
- [ ] Line height at least 1.5 for body text
- [ ] Font weights appropriate for screen sizes
- [ ] Text remains readable on small screens

### 3. Touch Targets
- [ ] All interactive elements (buttons, links) minimum 44x44px
- [ ] Adequate spacing between touch targets
- [ ] Form inputs easy to tap (min height 44px)
- [ ] No overlapping touch areas

### 4. Navigation
- [ ] Consistent navigation pattern across all pages
- [ ] Bottom navigation bar visible on mobile (≤767px)
- [ ] Bottom nav hidden on tablet/desktop (≥768px)
- [ ] No conflicting mobile menu implementations

### 5. Layout & Spacing
- [ ] Content doesn't hide behind bottom navigation
- [ ] Proper padding-bottom on main content (4rem + safe-area-inset)
- [ ] No horizontal overflow on small screens
- [ ] Responsive grid layouts work correctly

### 6. Performance
- [ ] Images optimized for mobile viewports
- [ ] Lazy loading implemented where appropriate
- [ ] No layout shifts (CLS issues)
- [ ] Smooth scrolling on mobile devices

### 7. Accessibility
- [ ] Color contrast ratios meet WCAG AA standards
- [ ] Focus indicators visible on all interactive elements
- [ ] Tap feedback on buttons/links
- [ ] Text size can be adjusted without breaking layout

### 8. iOS/Android Specific
- [ ] Safe area insets respected (notches, home indicator)
- [ ] Momentum scrolling enabled (-webkit-overflow-scrolling: touch)
- [ ] Tap highlight color set appropriately
- [ ] Text size adjustment disabled (prevent auto-zoom on orientation change)

## Testing Devices

### Required Tests
1. **iPhone SE** (375px) - Smallest mobile
2. **iPhone 14 Pro** (393px) - Modern iPhone
3. **Samsung Galaxy S21** (360px) - Modern Android
4. **iPad Mini** (768px) - Tablet breakpoint
5. **Desktop** (1440px+) - Ensure no mobile styles leak

### Test Scenarios
- Portrait and landscape orientations
- Different theme modes (light/dark/teal)
- With and without browser UI (full screen)
- Scroll behavior and snap points
- Form inputs and keyboard interaction

## Lighthouse Targets

### Performance
- Mobile Performance Score: > 90
- First Contentful Paint: < 1.8s
- Time to Interactive: < 3.8s
- Cumulative Layout Shift: < 0.1

### Accessibility
- Accessibility Score: > 95
- Color Contrast: AAA where possible
- Touch Targets: All ≥ 44px
- Heading Hierarchy: Proper nesting

## Common Issues to Check

### Font Size Violations
```bash
# Search for inaccessible font sizes
grep -r "text-\[9px\]" components/ app/
grep -r "text-\[10px\]" components/ app/
grep -r "text-\[11px\]" components/ app/
```

### Breakpoint Inconsistencies
```bash
# Check for inconsistent breakpoints
grep -r "@media (max-width: 768px)" app/ components/
grep -r "lg:hidden" components/layout/
```

### Touch Target Issues
```bash
# Find elements without minimum touch size
grep -r "p-1\b" components/
grep -r "h-6\b" components/
grep -r "w-6\b" components/
```

## Remediation Steps

### If font sizes < 12px found:
1. Replace with text-xs (12px) minimum
2. Update responsive variants (sm:, md:, lg:)
3. Test readability on actual devices

### If breakpoint issues found:
1. Standardize to 767px (mobile), 1023px (tablet)
2. Use mobile-first media queries
3. Ensure desktop overrides work

### If touch target issues found:
1. Add min-w-touch, min-h-touch classes
2. Increase padding on small buttons
3. Add spacing between adjacent touch targets

## Files to Review

### Critical Components
- `components/layout/BottomNav.tsx`
- `components/layout/Header.tsx`
- `components/articles/ArticleCard.tsx`

### Critical Pages
- `app/page.tsx` (homepage)
- `app/topics/page.tsx`
- `app/category/[category]/page.tsx`

### Global Styles
- `app/globals.css`
- `app/topics/topics.module.css`
- `tailwind.config.ts`

## Success Criteria

✅ Zero font sizes < 12px
✅ Zero touch targets < 44px
✅ Consistent breakpoints across all files
✅ Bottom navigation works on mobile
✅ Lighthouse accessibility score > 95
✅ No mobile view on desktop issues
✅ Smooth scroll performance on mobile
