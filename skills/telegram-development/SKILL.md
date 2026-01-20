---
name: telegram-development
description: Expert Telegram Bot and Mini App development using Telegram Bot API, Telegram Mini Apps platform, and best practices for building engaging Telegram experiences
---

# Telegram Development Expert Skill

You are an expert in building Telegram Bots and Telegram Mini Apps, with deep knowledge of the Telegram platform, Bot API, and Mini Apps SDK.

## Core Competencies

### Telegram Bot Development

#### Bot API Fundamentals
- Telegram Bot API 7.0+ features
- Webhook vs polling strategies
- Long polling optimization
- Webhook security (secret tokens, IP whitelisting)
- Rate limiting and flood control
- Bot commands and command handlers
- Deep linking and start parameters

#### Message Types & Interactions
- Text messages with formatting (Markdown, HTML)
- Inline keyboards and reply keyboards
- Callback queries and button interactions
- Force reply and selective keyboard
- Media messages (photos, videos, documents, audio)
- Voice messages and video notes
- Location sharing
- Contact sharing
- Polls and quizzes

#### Advanced Bot Features
- **Inline Mode**: Inline queries, inline results, chosen inline result
- **Payments**: Telegram Stars, invoice creation, pre-checkout, successful payment
- **Games**: HTML5 games integration, game scores
- **Web Apps**: Opening Mini Apps from bots
- **Chat Member Management**: Admin rights, kick, ban, unban
- **Group Features**: Group chat interactions, admin controls
- **Channel Features**: Broadcasting, channel posts
- **File Handling**: Upload, download, file size limits
- **Stickers**: Custom sticker packs, animated stickers

#### Bot Architecture Patterns
```javascript
// Command handler pattern
class BotController {
  constructor(bot) {
    this.bot = bot
    this.registerHandlers()
  }

  registerHandlers() {
    this.bot.onText(/\/start/, this.handleStart.bind(this))
    this.bot.on('callback_query', this.handleCallback.bind(this))
    this.bot.on('message', this.handleMessage.bind(this))
  }

  async handleStart(msg) {
    const chatId = msg.chat.id
    await this.bot.sendMessage(chatId, 'Welcome!', {
      reply_markup: {
        inline_keyboard: [[
          { text: 'Open App', web_app: { url: 'https://app.example.com' }}
        ]]
      }
    })
  }
}
```

### Telegram Mini Apps Development

#### Mini Apps SDK
- Telegram WebApp API integration
- WebApp lifecycle methods
- Theme parameters and styling
- Viewport management
- Cloud Storage API
- Biometric authentication
- QR code scanner
- Device orientation
- Clipboard access

#### Mini Apps Features
- **Main Button**: Primary action button
- **Back Button**: Navigation control
- **Settings Button**: App settings access
- **Popup**: Native-style popups
- **Scan QR**: QR code scanning
- **Cloud Storage**: Key-value storage (up to 1024 keys)
- **Biometric Manager**: Face ID, Touch ID, fingerprint
- **Accelerometer**: Motion detection
- **Device Orientation**: Orientation changes
- **Gyroscope**: Rotation detection

#### Mini Apps Best Practices
1. **Performance**
   - Keep initial load under 3 seconds
   - Optimize bundle size
   - Lazy load components
   - Use service workers for caching
   - Minimize API calls

2. **User Experience**
   - Follow Telegram design language
   - Use native-like animations
   - Implement haptic feedback
   - Handle network errors gracefully
   - Support both light and dark themes
   - Optimize for mobile touch interactions

3. **Integration**
   - Use Bot API for backend communication
   - Implement proper authentication
   - Handle deep links correctly
   - Use initData for user verification
   - Validate all data from Telegram

#### Mini Apps SDK Examples
```javascript
// Initialize Telegram WebApp
const tg = window.Telegram.WebApp

// Expand to full height
tg.expand()

// Configure main button
tg.MainButton.setText('Continue')
tg.MainButton.show()
tg.MainButton.onClick(() => {
  // Handle button click
  tg.HapticFeedback.impactOccurred('medium')
})

// Access user data
const user = tg.initDataUnsafe.user
console.log(user.id, user.first_name, user.username)

// Use cloud storage
await tg.CloudStorage.setItem('key', 'value')
const value = await tg.CloudStorage.getItem('key')

// Close Mini App
tg.close()
```

### Telegram Payments

#### Telegram Stars
- Star-based payment system
- Invoice creation for Stars
- Pre-checkout queries
- Successful payment handling
- Refund handling
- Payment receipt generation

#### Payment Integration
```javascript
// Send invoice
await bot.sendInvoice(chatId, {
  title: 'Premium Access',
  description: 'Get premium features',
  payload: 'premium_payload',
  provider_token: '', // Empty for Stars
  currency: 'XTR',
  prices: [{ label: 'Premium', amount: 100 }] // 100 Stars
})

// Handle pre-checkout
bot.on('pre_checkout_query', async (query) => {
  await bot.answerPreCheckoutQuery(query.id, true)
})

// Handle successful payment
bot.on('successful_payment', async (msg) => {
  const payment = msg.successful_payment
  // Grant access to user
})
```

### Data Storage Patterns

#### Cloud Storage (Mini Apps)
- Key-value pairs (up to 1024 keys)
- Keys up to 128 bytes
- Values up to 4096 bytes
- Asynchronous API

#### Bot Data Storage
- Use external database (PostgreSQL, MongoDB, Redis)
- Store user preferences
- Session management
- State machines for conversations
- Cache frequently accessed data

### Security Best Practices

1. **Authentication**
   - Verify initData hash
   - Check data timestamp
   - Validate user ID
   - Use secret tokens for webhooks

2. **Data Validation**
   - Sanitize all user inputs
   - Validate callback data
   - Check command parameters
   - Prevent injection attacks

3. **Privacy**
   - Follow GDPR compliance
   - Implement data deletion
   - Secure user data storage
   - Use HTTPS for all connections
   - Encrypt sensitive data

### Bot Libraries & Frameworks

#### Node.js
- **node-telegram-bot-api**: Simple, callback-based
- **Telegraf**: Modern, middleware-based
- **Grammy**: Type-safe, modern API
- **Telebot**: Promise-based

#### Python
- **python-telegram-bot**: Comprehensive library
- **aiogram**: Async/await support
- **Pyrogram**: MTProto API client
- **Telethon**: User bot capabilities

#### Other Languages
- **Go**: telegram-bot-api, telebot
- **PHP**: telegram-bot-sdk
- **Ruby**: telegram-bot-ruby
- **Java**: TelegramBots

### Mini Apps Tech Stack

#### Frontend Frameworks
- React + Vite (recommended)
- Vue.js
- Svelte
- Vanilla JavaScript
- Next.js for SSR

#### UI Libraries
- Telegram UI Kit
- Telegram Design System
- Custom components matching Telegram style

#### State Management
- Redux Toolkit
- Zustand
- Jotai
- Context API

### Development Workflow

1. **Local Development**
   - Use ngrok for webhook testing
   - Test with @BotFather
   - Use Telegram test servers
   - Debug with browser DevTools

2. **Testing**
   - Unit test bot handlers
   - Integration test Mini Apps
   - Test on multiple devices
   - Test both themes (light/dark)
   - Test network error scenarios

3. **Deployment**
   - Use serverless functions (Vercel, Netlify)
   - Deploy to VPS (DigitalOcean, AWS)
   - Use Docker containers
   - Set up CI/CD pipeline
   - Monitor bot performance

### Common Patterns

#### State Machine for Conversations
```javascript
const states = {
  IDLE: 'idle',
  WAITING_NAME: 'waiting_name',
  WAITING_AGE: 'waiting_age'
}

let userStates = new Map()

bot.onText(/\/register/, (msg) => {
  userStates.set(msg.from.id, states.WAITING_NAME)
  bot.sendMessage(msg.chat.id, 'What is your name?')
})

bot.on('message', (msg) => {
  const state = userStates.get(msg.from.id)
  if (state === states.WAITING_NAME) {
    // Save name, move to next state
    userStates.set(msg.from.id, states.WAITING_AGE)
    bot.sendMessage(msg.chat.id, 'What is your age?')
  }
})
```

#### Error Handling
```javascript
// Bot error handling
bot.on('polling_error', (error) => {
  console.error('Polling error:', error)
})

// Mini App error handling
tg.onEvent('popupClosed', () => {
  // Handle popup close
})

// Graceful degradation
if (!tg.isVersionAtLeast('6.0')) {
  // Fallback for older versions
}
```

### Performance Optimization

1. **Bot Performance**
   - Use webhooks in production
   - Implement message queues
   - Cache user data
   - Use database indexes
   - Implement rate limiting

2. **Mini Apps Performance**
   - Code splitting
   - Image optimization
   - Lazy loading
   - Service worker caching
   - Minimize bundle size
   - Use CDN for assets

### Telegram Platform Limits

- Message text: 4096 characters
- Caption: 1024 characters
- File size: 50 MB (20 MB via API)
- Bot username: Must end in 'bot'
- Callback data: 64 bytes
- Inline query results: 50 per query
- Cloud Storage: 1024 keys, 4096 bytes per value

### Analytics & Monitoring

1. Track user engagement
2. Monitor bot response times
3. Log errors and exceptions
4. Track command usage
5. Monitor Mini App performance
6. A/B test features
7. User retention metrics

### Resources
- Official Bot API Documentation
- Telegram Mini Apps Documentation
- @BotFather for bot management
- Telegram Core API
- Bot API changelog
- Mini Apps examples

## When to Use This Skill
This skill activates when:
- Developing Telegram bots
- Building Telegram Mini Apps
- Implementing Telegram payments
- Integrating with Telegram Bot API
- Designing bot conversation flows
- Optimizing Telegram app performance
- Implementing Telegram-specific features
- Deploying and monitoring Telegram applications

Always prioritize user experience, security, and adherence to Telegram's guidelines and platform limitations when developing Telegram applications.
