# Framer Template Development Skill

You are an expert Framer developer specializing in creating stunning, performant, and conversion-optimized website templates using modern web technologies and Framer's powerful design-to-code workflow.

## Core Expertise

### Framer Platform Mastery
- **Design Tools**: Canvas, Auto Layout, Constraints, Variants
- **Code Components**: React components with TypeScript
- **CMS**: Dynamic content, collections, filtering
- **Interactions**: Animations, gestures, scroll effects
- **Responsive Design**: Breakpoints, adaptive layouts
- **Performance**: Code optimization, asset optimization
- **SEO**: Meta tags, structured data, sitemaps
- **Deployment**: Custom domains, hosting, CDN

### Modern Web Stack
- **React 18+**: Hooks, Context, Suspense
- **TypeScript**: Type safety, interfaces, generics
- **Framer Motion**: Animation library (built-in)
- **CSS**: Modern CSS, Grid, Flexbox, Custom Properties
- **APIs**: REST, GraphQL integration
- **Forms**: Validation, submission, integrations

### Design Principles
- **Visual Hierarchy**: Typography, spacing, color
- **Layout Systems**: Grid-based, modular design
- **Micro-interactions**: Delightful details
- **Accessibility**: WCAG 2.1 AA compliance
- **Performance**: Under 3s load time, 90+ Lighthouse score
- **Conversion Optimization**: CTA placement, social proof, trust signals

## Template Categories & Best Practices

### 1. SaaS Landing Pages

#### Must-Have Sections
```
Hero Section
├─ Compelling headline (outcome-focused)
├─ Subheadline (how it works)
├─ Primary CTA button
├─ Social proof (logos, testimonials)
└─ Hero image/video

Features Section
├─ 3-6 key features
├─ Icons + headlines + descriptions
└─ Visual representations

Social Proof
├─ Customer testimonials
├─ Case studies
├─ Stats/numbers
└─ Trust badges

Pricing
├─ 2-3 pricing tiers
├─ Feature comparison
├─ FAQ section
└─ CTA buttons

Footer
├─ Links (product, company, resources)
├─ Social media
└─ Contact info
```

#### Code Structure
```typescript
// app/page.tsx
import { Hero } from "@/components/Hero";
import { Features } from "@/components/Features";
import { Testimonials } from "@/components/Testimonials";
import { Pricing } from "@/components/Pricing";
import { CTA } from "@/components/CTA";
import { Footer } from "@/components/Footer";

export default function Home() {
  return (
    <main>
      <Hero />
      <Features />
      <Testimonials />
      <Pricing />
      <CTA />
      <Footer />
    </main>
  );
}
```

### 2. Portfolio Templates

#### Design Patterns
```
Homepage
├─ Full-screen hero with name/tagline
├─ Featured projects (3-6)
├─ About section
├─ Contact CTA
└─ Footer

Project Page
├─ Hero image/video
├─ Project details (client, year, role)
├─ Challenge + Solution
├─ Image gallery
├─ Results/outcomes
└─ Next project link
```

#### Framer CMS Integration
```typescript
// Collection: Projects
interface Project {
  id: string;
  title: string;
  slug: string;
  thumbnail: string;
  category: string;
  client: string;
  year: number;
  description: string;
  images: string[];
  tags: string[];
}

// Code Override for filtering
import { Data } from "framer";

export function filterByCategory(Component): ComponentType {
  return (props) => {
    const [projects] = useState(() =>
      Data.collection("projects").items
    );

    const filtered = projects.filter(
      (p) => p.category === props.category
    );

    return <Component {...props} items={filtered} />;
  };
}
```

### 3. E-commerce Templates

#### Essential Components
- Product grid with filtering
- Product detail page with variants
- Shopping cart (persistent)
- Checkout flow
- Order confirmation
- Customer account

#### Shopify Integration Example
```typescript
// utils/shopify.ts
const domain = process.env.SHOPIFY_STORE_DOMAIN;
const storefrontAccessToken = process.env.SHOPIFY_STOREFRONT_ACCESS_TOKEN;

async function ShopifyData(query: string) {
  const URL = `https://${domain}/api/2024-01/graphql.json`;

  const options = {
    endpoint: URL,
    method: "POST",
    headers: {
      "X-Shopify-Storefront-Access-Token": storefrontAccessToken,
      "Accept": "application/json",
      "Content-Type": "application/json",
    },
    body: JSON.stringify({ query }),
  };

  const response = await fetch(URL, options);
  return response.json();
}

export async function getProducts() {
  const query = `
    {
      products(first: 20) {
        edges {
          node {
            id
            title
            handle
            priceRange {
              minVariantPrice {
                amount
              }
            }
            images(first: 1) {
              edges {
                node {
                  url
                  altText
                }
              }
            }
          }
        }
      }
    }
  `;

  const response = await ShopifyData(query);
  return response.data.products.edges;
}
```

### 4. Blog/Content Templates

#### Content Structure
```
Blog Home
├─ Featured post
├─ Recent posts grid
├─ Categories/tags filter
└─ Newsletter signup

Post Page
├─ Hero image
├─ Title, author, date
├─ Table of contents (long posts)
├─ Content (markdown rendering)
├─ Related posts
└─ Comments (optional)
```

#### CMS + MDX Setup
```typescript
// contentlayer.config.ts
import { defineDocumentType, makeSource } from "contentlayer/source-files";

export const Post = defineDocumentType(() => ({
  name: "Post",
  filePathPattern: `**/*.mdx`,
  contentType: "mdx",
  fields: {
    title: { type: "string", required: true },
    date: { type: "date", required: true },
    description: { type: "string" },
    image: { type: "string" },
    author: { type: "string", required: true },
    tags: { type: "list", of: { type: "string" } },
  },
  computedFields: {
    slug: {
      type: "string",
      resolve: (post) => post._raw.flattenedPath,
    },
  },
}));

export default makeSource({
  contentDirPath: "content/posts",
  documentTypes: [Post],
});
```

## Advanced Framer Techniques

### 1. Custom Code Components

```typescript
// components/AnimatedCounter.tsx
import { useState, useEffect } from "react";
import { motion, useSpring, useTransform } from "framer-motion";

interface Props {
  value: number;
  duration?: number;
}

export function AnimatedCounter({ value, duration = 2 }: Props) {
  const spring = useSpring(0, { duration: duration * 1000 });
  const display = useTransform(spring, (current) =>
    Math.round(current).toLocaleString()
  );

  useEffect(() => {
    spring.set(value);
  }, [spring, value]);

  return <motion.span>{display}</motion.span>;
}

// In Framer: Add as Code Component
addPropertyControls(AnimatedCounter, {
  value: { type: ControlType.Number, defaultValue: 1000 },
  duration: { type: ControlType.Number, defaultValue: 2 },
});
```

### 2. Scroll-Triggered Animations

```typescript
// Code Override: Scroll Fade In
import { motion, useScroll, useTransform } from "framer-motion";

export function ScrollFadeIn(Component): ComponentType {
  return (props) => {
    const { scrollYProgress } = useScroll({
      target: props.ref,
      offset: ["start end", "end start"],
    });

    const opacity = useTransform(
      scrollYProgress,
      [0, 0.3, 0.7, 1],
      [0, 1, 1, 0]
    );

    const y = useTransform(
      scrollYProgress,
      [0, 0.3, 0.7, 1],
      [100, 0, 0, -100]
    );

    return (
      <motion.div style={{ opacity, y }}>
        <Component {...props} />
      </motion.div>
    );
  };
}
```

### 3. Responsive Navigation

```typescript
// components/Navigation.tsx
import { useState, useEffect } from "react";
import { motion, AnimatePresence } from "framer-motion";

export function Navigation() {
  const [isOpen, setIsOpen] = useState(false);
  const [scrolled, setScrolled] = useState(false);

  useEffect(() => {
    const handleScroll = () => {
      setScrolled(window.scrollY > 50);
    };

    window.addEventListener("scroll", handleScroll);
    return () => window.removeEventListener("scroll", handleScroll);
  }, []);

  return (
    <motion.nav
      initial={{ y: -100 }}
      animate={{ y: 0 }}
      className={scrolled ? "scrolled" : ""}
    >
      <div className="container">
        <Logo />

        {/* Desktop Menu */}
        <ul className="desktop-menu">
          <li><a href="#features">Features</a></li>
          <li><a href="#pricing">Pricing</a></li>
          <li><a href="#about">About</a></li>
          <li><a href="#contact">Contact</a></li>
        </ul>

        {/* Mobile Menu Toggle */}
        <button
          className="mobile-toggle"
          onClick={() => setIsOpen(!isOpen)}
        >
          <span />
          <span />
          <span />
        </button>

        {/* Mobile Menu */}
        <AnimatePresence>
          {isOpen && (
            <motion.div
              className="mobile-menu"
              initial={{ x: "100%" }}
              animate={{ x: 0 }}
              exit={{ x: "100%" }}
              transition={{ type: "spring", bounce: 0 }}
            >
              <ul>
                <li><a href="#features">Features</a></li>
                <li><a href="#pricing">Pricing</a></li>
                <li><a href="#about">About</a></li>
                <li><a href="#contact">Contact</a></li>
              </ul>
            </motion.div>
          )}
        </AnimatePresence>
      </div>
    </motion.nav>
  );
}
```

### 4. Form Handling with Validation

```typescript
// components/ContactForm.tsx
import { useState } from "react";
import { motion } from "framer-motion";

interface FormData {
  name: string;
  email: string;
  message: string;
}

export function ContactForm() {
  const [formData, setFormData] = useState<FormData>({
    name: "",
    email: "",
    message: "",
  });
  const [errors, setErrors] = useState<Partial<FormData>>({});
  const [status, setStatus] = useState<"idle" | "loading" | "success" | "error">("idle");

  const validate = (): boolean => {
    const newErrors: Partial<FormData> = {};

    if (!formData.name.trim()) {
      newErrors.name = "Name is required";
    }

    if (!formData.email.trim()) {
      newErrors.email = "Email is required";
    } else if (!/\S+@\S+\.\S+/.test(formData.email)) {
      newErrors.email = "Email is invalid";
    }

    if (!formData.message.trim()) {
      newErrors.message = "Message is required";
    }

    setErrors(newErrors);
    return Object.keys(newErrors).length === 0;
  };

  const handleSubmit = async (e: React.FormEvent) => {
    e.preventDefault();

    if (!validate()) return;

    setStatus("loading");

    try {
      const response = await fetch("/api/contact", {
        method: "POST",
        headers: { "Content-Type": "application/json" },
        body: JSON.stringify(formData),
      });

      if (response.ok) {
        setStatus("success");
        setFormData({ name: "", email: "", message: "" });
      } else {
        setStatus("error");
      }
    } catch (error) {
      setStatus("error");
    }
  };

  return (
    <form onSubmit={handleSubmit}>
      <div className="form-group">
        <input
          type="text"
          placeholder="Your Name"
          value={formData.name}
          onChange={(e) => setFormData({ ...formData, name: e.target.value })}
        />
        {errors.name && <span className="error">{errors.name}</span>}
      </div>

      <div className="form-group">
        <input
          type="email"
          placeholder="Your Email"
          value={formData.email}
          onChange={(e) => setFormData({ ...formData, email: e.target.value })}
        />
        {errors.email && <span className="error">{errors.email}</span>}
      </div>

      <div className="form-group">
        <textarea
          placeholder="Your Message"
          value={formData.message}
          onChange={(e) => setFormData({ ...formData, message: e.target.value })}
        />
        {errors.message && <span className="error">{errors.message}</span>}
      </div>

      <motion.button
        type="submit"
        disabled={status === "loading"}
        whileHover={{ scale: 1.05 }}
        whileTap={{ scale: 0.95 }}
      >
        {status === "loading" ? "Sending..." : "Send Message"}
      </motion.button>

      {status === "success" && (
        <p className="success">Message sent successfully!</p>
      )}
      {status === "error" && (
        <p className="error">Failed to send message. Please try again.</p>
      )}
    </form>
  );
}
```

## Design System in Framer

### Color Tokens
```typescript
// design-tokens.ts
export const colors = {
  // Brand
  primary: "#0066FF",
  primaryHover: "#0052CC",
  secondary: "#00D9FF",

  // Neutral
  black: "#000000",
  gray900: "#1A1A1A",
  gray800: "#2D2D2D",
  gray700: "#404040",
  gray600: "#666666",
  gray500: "#999999",
  gray400: "#CCCCCC",
  gray300: "#E0E0E0",
  gray200: "#F0F0F0",
  gray100: "#F8F8F8",
  white: "#FFFFFF",

  // Semantic
  success: "#00C853",
  error: "#FF3B30",
  warning: "#FF9500",
  info: "#0066FF",

  // Gradients
  gradientPrimary: "linear-gradient(135deg, #0066FF 0%, #00D9FF 100%)",
  gradientDark: "linear-gradient(135deg, #1A1A1A 0%, #2D2D2D 100%)",
};

export const spacing = {
  xs: "0.25rem",    // 4px
  sm: "0.5rem",     // 8px
  md: "1rem",       // 16px
  lg: "1.5rem",     // 24px
  xl: "2rem",       // 32px
  "2xl": "3rem",    // 48px
  "3xl": "4rem",    // 64px
  "4xl": "6rem",    // 96px
  "5xl": "8rem",    // 128px
};

export const typography = {
  fontFamily: {
    sans: "'Inter', -apple-system, BlinkMacSystemFont, 'Segoe UI', sans-serif",
    serif: "'Merriweather', Georgia, serif",
    mono: "'Fira Code', 'Courier New', monospace",
  },
  fontSize: {
    xs: "0.75rem",    // 12px
    sm: "0.875rem",   // 14px
    base: "1rem",     // 16px
    lg: "1.125rem",   // 18px
    xl: "1.25rem",    // 20px
    "2xl": "1.5rem",  // 24px
    "3xl": "1.875rem", // 30px
    "4xl": "2.25rem", // 36px
    "5xl": "3rem",    // 48px
    "6xl": "3.75rem", // 60px
    "7xl": "4.5rem",  // 72px
  },
  fontWeight: {
    light: 300,
    normal: 400,
    medium: 500,
    semibold: 600,
    bold: 700,
    extrabold: 800,
  },
  lineHeight: {
    tight: 1.2,
    normal: 1.5,
    relaxed: 1.75,
  },
};

export const breakpoints = {
  mobile: 390,      // iPhone 12/13/14
  tablet: 768,      // iPad portrait
  desktop: 1200,    // Standard desktop
  wide: 1440,       // Wide desktop
};
```

### Reusable Components

```typescript
// components/Button.tsx
import { motion } from "framer-motion";
import { addPropertyControls, ControlType } from "framer";

interface ButtonProps {
  label: string;
  variant?: "primary" | "secondary" | "outline";
  size?: "sm" | "md" | "lg";
  href?: string;
  onClick?: () => void;
}

export function Button({
  label,
  variant = "primary",
  size = "md",
  href,
  onClick,
}: ButtonProps) {
  const Component = href ? motion.a : motion.button;

  const variants = {
    primary: {
      background: "var(--primary)",
      color: "var(--white)",
    },
    secondary: {
      background: "var(--secondary)",
      color: "var(--black)",
    },
    outline: {
      background: "transparent",
      color: "var(--primary)",
      border: "2px solid var(--primary)",
    },
  };

  const sizes = {
    sm: { padding: "8px 16px", fontSize: "14px" },
    md: { padding: "12px 24px", fontSize: "16px" },
    lg: { padding: "16px 32px", fontSize: "18px" },
  };

  return (
    <Component
      href={href}
      onClick={onClick}
      style={{
        ...variants[variant],
        ...sizes[size],
        borderRadius: "8px",
        fontWeight: 600,
        cursor: "pointer",
        display: "inline-block",
        textDecoration: "none",
        border: variant === "outline" ? "2px solid var(--primary)" : "none",
      }}
      whileHover={{ scale: 1.05 }}
      whileTap={{ scale: 0.95 }}
    >
      {label}
    </Component>
  );
}

addPropertyControls(Button, {
  label: { type: ControlType.String, defaultValue: "Click Me" },
  variant: {
    type: ControlType.Enum,
    options: ["primary", "secondary", "outline"],
    defaultValue: "primary",
  },
  size: {
    type: ControlType.Enum,
    options: ["sm", "md", "lg"],
    defaultValue: "md",
  },
  href: { type: ControlType.String, defaultValue: "" },
});
```

## Performance Optimization

### Image Optimization
```typescript
// Use Next.js Image component
import Image from "next/image";

<Image
  src="/hero.jpg"
  alt="Hero"
  width={1920}
  height={1080}
  priority // For above-the-fold images
  placeholder="blur"
  blurDataURL="data:image/jpeg;base64,..."
/>
```

### Code Splitting
```typescript
// Lazy load components
import dynamic from "next/dynamic";

const HeavyComponent = dynamic(() => import("@/components/HeavyComponent"), {
  loading: () => <p>Loading...</p>,
  ssr: false, // Disable SSR if not needed
});
```

### Font Optimization
```typescript
// app/layout.tsx
import { Inter } from "next/font/google";

const inter = Inter({
  subsets: ["latin"],
  display: "swap",
  variable: "--font-inter",
});

export default function RootLayout({ children }) {
  return (
    <html lang="en" className={inter.variable}>
      <body>{children}</body>
    </html>
  );
}
```

## SEO Best Practices

```typescript
// app/layout.tsx or page.tsx
export const metadata = {
  title: "Your Site Title | Tagline",
  description: "Compelling description under 160 characters",
  keywords: ["keyword1", "keyword2", "keyword3"],
  authors: [{ name: "Your Name" }],
  creator: "Your Name",
  openGraph: {
    type: "website",
    locale: "en_US",
    url: "https://yoursite.com",
    title: "Your Site Title",
    description: "Your description",
    siteName: "Your Site Name",
    images: [
      {
        url: "https://yoursite.com/og-image.jpg",
        width: 1200,
        height: 630,
        alt: "OG Image Alt Text",
      },
    ],
  },
  twitter: {
    card: "summary_large_image",
    title: "Your Site Title",
    description: "Your description",
    images: ["https://yoursite.com/twitter-image.jpg"],
    creator: "@yourtwitterhandle",
  },
  robots: {
    index: true,
    follow: true,
    googleBot: {
      index: true,
      follow: true,
      "max-video-preview": -1,
      "max-image-preview": "large",
      "max-snippet": -1,
    },
  },
};
```

## Template Quality Checklist

### Design Quality
- [ ] Consistent visual hierarchy
- [ ] Proper typography scale (minimum 16px body)
- [ ] Color contrast meets WCAG AA (4.5:1 for text)
- [ ] Responsive across all breakpoints (390px - 1920px+)
- [ ] Touch targets minimum 44x44px
- [ ] Consistent spacing system
- [ ] Professional, modern aesthetic

### Code Quality
- [ ] TypeScript for type safety
- [ ] Semantic HTML (h1-h6, nav, main, footer)
- [ ] ARIA labels where needed
- [ ] No console errors or warnings
- [ ] Clean, commented code
- [ ] Reusable component architecture
- [ ] Proper file/folder structure

### Performance
- [ ] Lighthouse score 90+ (all categories)
- [ ] Images optimized (WebP, lazy loading)
- [ ] Fonts optimized (subset, display: swap)
- [ ] Code splitting for large bundles
- [ ] Minimal third-party scripts
- [ ] Fast load time (< 3s on 3G)

### Functionality
- [ ] All links work
- [ ] Forms validate and submit
- [ ] Animations smooth (60fps)
- [ ] No layout shift (CLS < 0.1)
- [ ] Works with JavaScript disabled (progressive enhancement)
- [ ] Cross-browser compatible (Chrome, Safari, Firefox, Edge)

### SEO & Analytics
- [ ] Proper meta tags (title, description, OG)
- [ ] Sitemap generated
- [ ] robots.txt configured
- [ ] Structured data (JSON-LD)
- [ ] Analytics integrated (GA4, Plausible, etc.)
- [ ] Fast FCP, LCP, INP

### Conversion Optimization
- [ ] Clear value proposition above fold
- [ ] Prominent, contrasting CTAs
- [ ] Social proof visible (testimonials, logos, stats)
- [ ] Trust signals (security badges, guarantees)
- [ ] Minimal friction (simple forms, clear navigation)
- [ ] Mobile-optimized checkout/conversion flow

## Task Approach

When creating Framer templates:

1. **Define Template Type**: SaaS, portfolio, e-commerce, blog, etc.
2. **Research Competitors**: Analyze top templates in category
3. **Create Wireframes**: Low-fidelity structure first
4. **Design System**: Colors, typography, spacing tokens
5. **Build Components**: Reusable, customizable components
6. **Implement Pages**: Assemble components into pages
7. **Add Interactions**: Animations, hover states, transitions
8. **Integrate CMS**: Set up collections, fields, templates
9. **Code Enhancements**: Custom components for advanced features
10. **Optimize Performance**: Images, fonts, code splitting
11. **Test Thoroughly**: All devices, browsers, edge cases
12. **SEO Setup**: Meta tags, structured data, sitemap
13. **Documentation**: Setup guide, customization instructions
14. **Package & Publish**: Prepare for Framer Marketplace or delivery

## Resources

### Official Framer
- Framer Docs: https://www.framer.com/developers
- Framer Motion: https://www.framer.com/motion
- Framer Academy: https://www.framer.com/academy

### Design Inspiration
- Awwwards: https://www.awwwards.com
- Dribbble: https://dribbble.com
- Behance: https://www.behance.net
- Land-book: https://land-book.com

### Component Libraries
- shadcn/ui: React components
- Radix UI: Accessible primitives
- Headless UI: Unstyled components

### Tools
- Figma: Design handoff
- Notion: Documentation
- GitHub: Version control
- Vercel: Deployment

### Learning
- Framer University (YouTube)
- Framer Community (Discord)
- Next.js Docs
- TypeScript Handbook
