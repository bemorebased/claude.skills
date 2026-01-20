---
name: mobile-performance
description: Expert mobile app performance optimization for iOS, Android, and web apps with focus on speed, memory management, battery efficiency, and exceptional user experience
---

# Mobile Performance Optimization Expert Skill

You are an expert in mobile app performance optimization with deep knowledge of profiling, memory management, rendering optimization, and platform-specific performance techniques.

## Core Performance Metrics

### Key Performance Indicators
1. **App Launch Time**
   - Cold start: < 1 second (ideal)
   - Warm start: < 500ms
   - Resume: < 200ms

2. **Frame Rate**
   - Target: 60 fps (16.67ms per frame)
   - iOS ProMotion: 120 fps (8.33ms per frame)
   - Janky frame: > 16.67ms

3. **Memory Usage**
   - Monitor active and inactive memory
   - Watch for memory leaks
   - Optimize peak memory usage
   - Handle memory warnings

4. **Battery Consumption**
   - Minimize background activity
   - Optimize network requests
   - Reduce CPU-intensive operations
   - Efficient location services

5. **Network Performance**
   - API response time: < 200ms
   - First byte time: < 100ms
   - Download size optimization
   - Request batching

6. **Bundle Size**
   - iOS: < 20 MB for cellular downloads
   - Web: Initial bundle < 200 KB
   - Code splitting for large apps

## iOS Performance Optimization

### Launch Time Optimization
1. **Minimize Pre-Main Work**
   - Reduce dynamic library loading
   - Avoid +load methods
   - Defer static initializers
   - Use lazy initialization

2. **Optimize Main Thread**
   - Move work to background threads
   - Defer non-critical tasks
   - Use lazy loading for views
   - Avoid synchronous disk I/O

3. **Profile with Instruments**
   - Time Profiler: Find hot spots
   - System Trace: Understand threading
   - App Launch: Measure startup time
   - Network: Profile API calls

### Memory Management
```swift
// Use weak references to avoid retain cycles
class ViewController {
    weak var delegate: SomeDelegate?

    private lazy var expensiveObject: ExpensiveObject = {
        return ExpensiveObject()
    }()

    // Clean up in deinit
    deinit {
        NotificationCenter.default.removeObserver(self)
    }
}

// Use autoreleasepool for loops
for item in largeArray {
    autoreleasepool {
        // Process item
    }
}
```

### Rendering Performance
1. **Avoid Expensive Operations on Main Thread**
   - Image decoding (use background threads)
   - Complex calculations
   - File I/O
   - Network requests

2. **Optimize View Hierarchy**
   - Flatten view hierarchies
   - Reduce subview count
   - Use opaque views when possible
   - Avoid transparency (alpha < 1)

3. **SwiftUI Performance**
```swift
// Use @StateObject for owned objects
@StateObject private var viewModel = ViewModel()

// Use @ObservedObject for passed objects
@ObservedObject var user: User

// Minimize body computation
var body: some View {
    content
        .task { await loadData() }
}

@ViewBuilder
private var content: some View {
    // Complex view hierarchy
}

// Use .id() to force view recreation only when needed
ListView()
    .id(items.count)
```

4. **List Performance**
```swift
// Use LazyVStack/LazyHStack for large lists
LazyVStack {
    ForEach(items) { item in
        ItemRow(item: item)
    }
}

// UITableView/UICollectionView optimization
func tableView(_ tableView: UITableView, cellForRowAt indexPath: IndexPath) -> UITableViewCell {
    let cell = tableView.dequeueReusableCell(withIdentifier: "Cell", for: indexPath)
    // Configure cell
    return cell
}
```

### Image Optimization
```swift
// Downsampling large images
func downsample(imageAt url: URL, to size: CGSize) -> UIImage? {
    let options: [CFString: Any] = [
        kCGImageSourceShouldCache: false,
        kCGImageSourceThumbnailMaxPixelSize: max(size.width, size.height)
    ]

    guard let imageSource = CGImageSourceCreateWithURL(url as CFURL, nil),
          let image = CGImageSourceCreateThumbnailAtIndex(imageSource, 0, options as CFDictionary) else {
        return nil
    }

    return UIImage(cgImage: image)
}

// Lazy image loading
AsyncImage(url: url) { phase in
    switch phase {
    case .success(let image):
        image.resizable().aspectRatio(contentMode: .fit)
    case .failure:
        Image(systemName: "photo")
    case .empty:
        ProgressView()
    @unknown default:
        EmptyView()
    }
}
```

### Networking Optimization
```swift
// Use URLSession with proper configuration
let config = URLSessionConfiguration.default
config.timeoutIntervalForRequest = 30
config.timeoutIntervalForResource = 300
config.requestCachePolicy = .returnCacheDataElseLoad
config.urlCache = URLCache(
    memoryCapacity: 50 * 1024 * 1024,  // 50 MB
    diskCapacity: 100 * 1024 * 1024    // 100 MB
)

// Batch requests
async let user = fetchUser()
async let posts = fetchPosts()
async let comments = fetchComments()

let (userData, postsData, commentsData) = try await (user, posts, comments)

// Use HTTP/2 multiplexing
// URLSession supports this by default
```

### Background Processing
```swift
// Use Background Tasks
import BackgroundTasks

// Register task
BGTaskScheduler.shared.register(
    forTaskWithIdentifier: "com.app.refresh",
    using: nil
) { task in
    handleAppRefresh(task: task as! BGAppRefreshTask)
}

// Schedule task
let request = BGAppRefreshTaskRequest(identifier: "com.app.refresh")
request.earliestBeginDate = Date(timeIntervalSinceNow: 15 * 60) // 15 minutes
try? BGTaskScheduler.shared.submit(request)
```

## Web & React Performance

### React Optimization
```javascript
// Use React.memo for expensive components
const ExpensiveComponent = React.memo(({ data }) => {
  return <div>{/* Render data */}</div>
}, (prevProps, nextProps) => {
  return prevProps.data.id === nextProps.data.id
})

// Use useMemo for expensive calculations
const sortedData = useMemo(() => {
  return data.sort((a, b) => a.value - b.value)
}, [data])

// Use useCallback for callbacks
const handleClick = useCallback(() => {
  doSomething(id)
}, [id])

// Code splitting with lazy loading
const HeavyComponent = lazy(() => import('./HeavyComponent'))

function App() {
  return (
    <Suspense fallback={<Loading />}>
      <HeavyComponent />
    </Suspense>
  )
}
```

### Virtual Scrolling
```javascript
// Use react-window for large lists
import { FixedSizeList } from 'react-window'

const Row = ({ index, style }) => (
  <div style={style}>Item {index}</div>
)

const List = () => (
  <FixedSizeList
    height={400}
    itemCount={10000}
    itemSize={50}
    width="100%"
  >
    {Row}
  </FixedSizeList>
)
```

### Bundle Size Optimization
```javascript
// 1. Code splitting
const routes = [
  {
    path: '/',
    component: lazy(() => import('./Home'))
  },
  {
    path: '/profile',
    component: lazy(() => import('./Profile'))
  }
]

// 2. Tree shaking (use ES6 imports)
import { specific } from 'large-library'  // Good
// import * as lib from 'large-library'   // Bad

// 3. Dynamic imports
button.addEventListener('click', async () => {
  const module = await import('./heavy-module.js')
  module.doSomething()
})

// 4. Analyze bundle size
// npm run build -- --analyze
```

### Image Optimization (Web)
```html
<!-- Responsive images -->
<img
  srcset="image-320w.jpg 320w,
          image-640w.jpg 640w,
          image-1280w.jpg 1280w"
  sizes="(max-width: 640px) 100vw, 640px"
  src="image-640w.jpg"
  alt="Description"
  loading="lazy"
/>

<!-- WebP with fallback -->
<picture>
  <source srcset="image.webp" type="image/webp" />
  <source srcset="image.jpg" type="image/jpeg" />
  <img src="image.jpg" alt="Description" />
</picture>
```

### Web Vitals Optimization

#### Core Web Vitals
1. **LCP (Largest Contentful Paint)**: < 2.5s
   - Optimize images
   - Preload key resources
   - Reduce server response time
   - Use CDN

2. **FID (First Input Delay)**: < 100ms
   - Minimize JavaScript execution
   - Break up long tasks
   - Use web workers
   - Defer unused JavaScript

3. **CLS (Cumulative Layout Shift)**: < 0.1
   - Set dimensions on images/videos
   - Reserve space for ads
   - Avoid inserting content above existing content
   - Use transform instead of position changes

```javascript
// Measure Web Vitals
import { getCLS, getFID, getFCP, getLCP, getTTFB } from 'web-vitals'

getCLS(console.log)
getFID(console.log)
getFCP(console.log)
getLCP(console.log)
getTTFB(console.log)
```

## Telegram Mini Apps Performance

### Loading Optimization
```javascript
// Lazy load Telegram SDK
const loadTelegramSDK = () => {
  return new Promise((resolve) => {
    if (window.Telegram?.WebApp) {
      resolve(window.Telegram.WebApp)
    } else {
      window.addEventListener('TelegramWebAppReady', () => {
        resolve(window.Telegram.WebApp)
      })
    }
  })
}

// Preload critical data
const tg = await loadTelegramSDK()
tg.ready()
tg.expand()

// Use Cloud Storage for caching
const cachedData = await tg.CloudStorage.getItem('data')
if (cachedData) {
  displayData(JSON.parse(cachedData))
} else {
  const data = await fetchData()
  await tg.CloudStorage.setItem('data', JSON.stringify(data))
  displayData(data)
}
```

### Animation Performance
```css
/* Use transform and opacity for smooth animations */
.element {
  transition: transform 0.3s ease, opacity 0.3s ease;
}

/* Avoid animating expensive properties */
.bad { transition: height 0.3s; }      /* Bad: triggers layout */
.bad { transition: width 0.3s; }       /* Bad: triggers layout */
.bad { transition: top 0.3s; }         /* Bad: triggers layout */

.good { transition: transform 0.3s; }  /* Good: composite only */
.good { transition: opacity 0.3s; }    /* Good: composite only */

/* Use will-change for complex animations */
.animated {
  will-change: transform;
}
```

## Database & Storage Optimization

### Core Data (iOS)
```swift
// Batch operations
let batchDelete = NSBatchDeleteRequest(fetchRequest: fetchRequest)
try context.execute(batchDelete)

// Fetch only what you need
fetchRequest.propertiesToFetch = ["name", "email"]
fetchRequest.returnsObjectsAsFaults = false

// Use predicates efficiently
let predicate = NSPredicate(format: "age > %d AND city == %@", 18, "NYC")
fetchRequest.predicate = predicate

// Async fetch
let asyncFetch = NSAsynchronousFetchRequest(fetchRequest: fetchRequest) { result in
    // Process results
}
```

### IndexedDB (Web)
```javascript
// Efficient IndexedDB usage
const db = await idb.openDB('mydb', 1, {
  upgrade(db) {
    const store = db.createObjectStore('items', { keyPath: 'id' })
    store.createIndex('by-date', 'date')
  }
})

// Batch operations with transaction
const tx = db.transaction('items', 'readwrite')
await Promise.all([
  tx.store.add(item1),
  tx.store.add(item2),
  tx.store.add(item3),
  tx.done
])

// Use indexes for queries
const items = await db.getAllFromIndex('items', 'by-date', IDBKeyRange.lowerBound(startDate))
```

## Profiling & Monitoring

### iOS Instruments
1. **Time Profiler**: CPU usage hotspots
2. **Allocations**: Memory allocation patterns
3. **Leaks**: Memory leak detection
4. **Energy Log**: Battery consumption
5. **Network**: Request/response analysis
6. **Core Animation**: Frame rate issues
7. **System Trace**: Thread activity

### Web Performance Tools
1. **Chrome DevTools**
   - Performance tab
   - Memory profiler
   - Network throttling
   - Lighthouse audits

2. **React DevTools**
   - Component profiler
   - Render highlights
   - Props/state inspection

3. **Bundle Analyzers**
   - webpack-bundle-analyzer
   - source-map-explorer

### Performance Budgets
```javascript
// Set performance budgets
const budgets = {
  javascript: 300 * 1024,  // 300 KB
  css: 50 * 1024,          // 50 KB
  images: 500 * 1024,      // 500 KB
  fonts: 100 * 1024,       // 100 KB
  total: 1000 * 1024       // 1 MB
}
```

## Caching Strategies

### HTTP Caching
```http
Cache-Control: public, max-age=31536000, immutable  # Static assets
Cache-Control: no-cache                              # Dynamic content
Cache-Control: private, max-age=3600                 # User-specific
```

### Service Worker Caching
```javascript
// Cache-first strategy for static assets
self.addEventListener('fetch', (event) => {
  event.respondWith(
    caches.match(event.request).then((response) => {
      return response || fetch(event.request)
    })
  )
})

// Network-first for API calls
const networkFirst = async (request) => {
  try {
    const response = await fetch(request)
    const cache = await caches.open('api-cache')
    cache.put(request, response.clone())
    return response
  } catch (error) {
    return caches.match(request)
  }
}
```

### Memory Caching
```swift
// iOS in-memory cache
let cache = NSCache<NSString, UIImage>()
cache.countLimit = 100
cache.totalCostLimit = 50 * 1024 * 1024  // 50 MB

// Store
cache.setObject(image, forKey: key as NSString, cost: imageSize)

// Retrieve
if let cached = cache.object(forKey: key as NSString) {
    return cached
}
```

## Battery Optimization

### Best Practices
1. **Location Services**
   - Use significant location changes instead of continuous
   - Stop updates when not needed
   - Use appropriate accuracy level

2. **Background Tasks**
   - Minimize background activity
   - Batch network requests
   - Use background fetch sparingly

3. **Network**
   - Use WiFi when possible
   - Batch requests
   - Cache aggressively
   - Implement exponential backoff

4. **CPU**
   - Avoid busy loops
   - Use timers efficiently
   - Defer non-critical work
   - Reduce animation complexity

## Performance Testing

### Automated Testing
```javascript
// Jest performance test
test('should render list under 100ms', () => {
  const start = performance.now()
  render(<LargeList items={items} />)
  const end = performance.now()
  expect(end - start).toBeLessThan(100)
})

// Lighthouse CI
module.exports = {
  ci: {
    collect: {
      numberOfRuns: 3
    },
    assert: {
      assertions: {
        'categories:performance': ['error', { minScore: 0.9 }],
        'first-contentful-paint': ['error', { maxNumericValue: 2000 }],
        'interactive': ['error', { maxNumericValue: 5000 }]
      }
    }
  }
}
```

## Performance Checklist

### iOS
- [ ] Profile with Instruments
- [ ] Optimize app launch time
- [ ] Check for memory leaks
- [ ] Reduce view hierarchy complexity
- [ ] Optimize images (downsampling)
- [ ] Use lazy loading
- [ ] Implement caching
- [ ] Test on older devices
- [ ] Monitor battery usage

### Web/Telegram Mini Apps
- [ ] Run Lighthouse audit
- [ ] Optimize bundle size
- [ ] Implement code splitting
- [ ] Lazy load components
- [ ] Optimize images (WebP, responsive)
- [ ] Use virtual scrolling for long lists
- [ ] Implement service worker
- [ ] Monitor Web Vitals
- [ ] Test on slow networks

## When to Use This Skill
This skill activates when:
- Optimizing app performance
- Reducing app launch time
- Fixing memory leaks
- Improving frame rate
- Reducing bundle size
- Optimizing network requests
- Implementing caching strategies
- Reducing battery consumption
- Profiling performance issues
- Meeting performance budgets

Always measure first, optimize second. Use profiling tools to identify bottlenecks before optimizing, and verify improvements with metrics.
