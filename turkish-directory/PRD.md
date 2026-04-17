# Product Requirements Document — turkish.directory

> **Project:** Storefront Generator (Phase 1)
> **Domain:** turkish.directory (currently parked)
> **Status:** Approved for Design
> **Date:** 2026-04-04
> **Stakeholders:** Turgay (Founder, Superadmin)
> **Vision:** Build an AI-powered "Self-Launching Export Engine" that lets Turkish SMEs create German-compliant storefronts in minutes

---

## Table of Contents

1. [Product Overview](#1-product-overview)
2. [Target Users](#2-target-users)
3. [Core User Flows](#3-core-user-flows)
4. [Design System & Aesthetic](#4-design-system--aesthetic)
5. [Detailed Page Specs](#5-detailed-page-specs)
6. [Component Library](#6-component-library)
7. [Responsive Behavior](#7-responsive-behavior)
8. [Navigation & Routing](#8-navigation--routing)
9. [Copy & Microcopy Guidelines](#9-copy--microcopy-guidelines)
10. [Accessibility Requirements](#10-accessibility-requirements)
11. [Edge Cases & Error States](#11-edge-cases--error-states)
12. [Out of Scope for Phase 1](#12-out-of-scope-for-phase-1)
13. [Future Considerations](#13-future-considerations)

---

## 1. Product Overview

### What We're Building

**turkish.directory** is a web platform where Turkish SMEs (small-to-medium enterprises) can create a German-compliant storefront through a conversational AI onboarding wizard.

### The Concept

A Turkish business owner visits the site, answers questions in a friendly Turkish chat interface (one question at a time), and within minutes receives a professional German storefront — complete with professional copy (transcreated, not translated), Impressum, Datenschutz, and a trust-focused German aesthetic. The storefront is published as a subdirectory on turkish.directory.

### Phase 1 Scope

Single product: **Storefront Generator**. The user goes through a conversational wizard, the AI generates a storefront, and the storefront goes live. Free tier only (no payment in Phase 1).

---

## 2. Target Users

### Primary User: Turkish SME Owner / Marketing Manager

**Profile:**
- Age: 25-60
- Location: Turkey (Istanbul, Bursa, Denizli, Gaziantep, etc.)
- Tech savvy: Low to medium — comfortable with smartphones, WhatsApp, Facebook
- Language: Turkish (primary)
- Goal: Sell to Germany but doesn't know how
- Pain: Can't afford a German-language website, doesn't understand German legal requirements, doesn't know how to reach German buyers

**Design implication:** Everything must feel simple, guided, and reassuring. No complicated forms. No jargon. Think WhatsApp conversation, not tax return.

### Secondary User: German B2B Buyer (Phase 2+, but storefronts must appeal to them)

**Profile:**
- Age: 30-55
- Location: Germany (all regions)
- Tech savvy: High — comfortable with B2B platforms, procurement tools
- Language: German
- Goal: Find reliable Turkish suppliers
- Pain: Uncertainty about quality, trust, communication barriers, legal compliance

**Design implication:** Storefronts must feel professional, trustworthy, and "German" — clean, data-rich, with clear trust signals (Impressum, certifications, quality standards).

### Admin: Turgay (Superadmin)

- Can create additional admin accounts
- Full visibility into all storefronts and users
- Dashboard for management and analytics

---

## 3. Core User Flows

### Flow 1: Visitor → AI Chat → Storefront Creation (Main Path)

```
Landing Page
    ↓
Language Auto-detected (defaults to TR)
    ↓
"Start" or types in chat
    ↓
AI asks Step 1: Company name + logo upload
    ↓
Step 2: Business description
    ↓
Step 3: Industry selection (visual gallery: Furniture, Textiles, Food, Industrial, Services)
    ↓
Step 4: Products/Services + photo uploads (up to 10, 5MB each)
    ↓
Step 5: Target audience in Germany
    ↓
Step 6: Certifications, awards, quality standards
    ↓
Step 7: Contact information
    ↓
Step 8: Unique selling points
    ↓
Email capture (for magic link + notifications)
    ↓
"Generate My Storefront" button
    ↓
Loading screen + email notification ("Your storefront is being built...")
    ↓
Email: "Your storefront is ready!" with magic link
    ↓
Live storefront preview (TR/DE toggle)
```

### Flow 2: Edit Storefront

```
User opens magic link
    ↓
Storefront with floating "✏️ Edit" button on each section
    ↓
Clicks Edit → opens AI chat: "What would you like to change?"
    ↓
User describes changes in free-form Turkish
    ↓
AI regenerates section → live preview → "Publish" to confirm
```

### Flow 3: Admin Login

```
/admin
    ↓
Admin login form (email + password)
    ↓
Dashboard: Storefronts list, creation charts, user management
    ↓
Can create new admin accounts (email + password)
```

### Flow 4: German Buyer Interaction (Storefront Contact Form)

```
German buyer visits storefront (turkish.directory/bursa-mobilya)
    ↓
Scrolls to "Contact" section
    ↓
Fills inquiry form (name, email, message)
    ↓
Submits
    ↓
Two emails sent: (1) Turkish company, (2) Superadmin notification
```

---

## 4. Design System & Aesthetic

### Landing Page Aesthetic — TR (Turkish side)

- **Vibe:** Warm, modern, approachable — think "your local consultant"
- **Colors:** Inviting — consider warm tones (amber, teal, deep navy)
- **Typography:** Friendly but professional sans-serif
- **Tone:** Conversational, encouraging, "we'll do this together"
- **Inspiration:** Modern SaaS landing pages but with Mediterranean warmth
- **Key feeling:** "This is easy. I can do this in 5 minutes."

### Storefront Aesthetic — DE (German side)

- **Vibe:** Professional, clean, trust-focused — think "German Mittelstand"
- **Colors:** Conservative — navy blue, gray, white, subtle accent color per template
- **Typography:** Professional, readable — similar to what German Mittelstand companies use
- **Tone:** Formal, professional, data-driven
- **Inspiration:** German B2B platforms, industrial catalogs, corporate websites
- **Key feeling:** "This is a legitimate, reliable company I can do business with."

### Admin Dashboard Aesthetic

- **Vibe:** Functional, clean, data-forward — think Vercel, Supabase dashboards
- **Colors:** Dark or light theme — consistent with modern admin panels
- **Typography:** Developer-friendly, monospace for IDs, clean sans-serif for headers

### Language & Branding

**Brand name:** turkish.directory
**Tagline (TR):** "Alman pazarına 5 dakikada açılın"
**Tagline (DE):** "Der türkisch-deutsche Marktplatz"
**Tone of voice:**
- TR: Warm, friendly, colloquial but professional
- DE: Professional, formal (Sie), precise

---

## 5. Detailed Page Specs

### 5.1 Landing Page (turkish.directory)

#### Layout Structure

```
┌─────────────────────────────────────────┐
│ Header (logo + language toggle)         │
├─────────────────────────────────────────┤
│ Hero Section                            │
│   - Headline (TR or DE based on detect) │
│   - Subheadline                         │
│   - CTA: "Hemen Başla" / "Jetzt Start" │
├─────────────────────────────────────────┤
│ Trust Signals                           │
│   - "X Türk firması Almanya'da"         │
│   - "Ücretsiz, dakikalar içinde"         │
├─────────────────────────────────────────┤
│ How It Works (3 steps, visual)          │
│   1. Soruları cevapla                    │
│   2. Alman web siten hazır               │
│   3. Alman müşterilere ulaş              │
├─────────────────────────────────────────┤
│ Template Gallery Preview                │
│   - Show 3-5 template screenshots        │
├─────────────────────────────────────────┤
│ FAQ Section                             │
├─────────────────────────────────────────┤
│ Footer                                  │
│   - Links: About, Contact, Legal        │
└─────────────────────────────────────────┘
```

#### Hero Section

- **Headline (TR):** "Alman pazarına 5 dakikada açılın"
- **Subheadline (TR):** "Sadece şirket bilgilerinizi girin. Alman standartlarına uygun profesyonel web siteniz yapay zeka ile hazır."
- **CTA Button:** "Hemen Başla" (prominent, primary color)
- **Visual:** Either an illustration of Turkish↔German connection, or a before/after (Turkish business → German storefront)

#### Language Toggle

- Top-right corner, always visible
- Format: `🌐 TR | DE`
- Switches ALL page content instantly (no page reload)

### 5.2 AI Chat Wizard (Conversational Onboarding)

#### Layout

```
┌─────────────────────────────────────────┐
│ Header: Logo + progress indicator       │
│         "Adım 3/8"                      │
├─────────────────────────────────────────┤
│                                         │
│  AI Message Bubbles (scrollable area)   │
│                                         │
│  [AI]: Merhaba! Şirketinizin adını       │
│        söyleyebilir misiniz?             │
│                                         │
│  [AI]: Logo yüklemek ister misiniz?      │
│  [📎 Upload Button] [⬆️ Dosya Seç]     │
│                                         │
│  ────────────────────────────────────   │
│  [  Cevabınızı burada yazın...       ] │
│  [             📤 Gönder            ]   │
│  ────────────────────────────────────   │
└─────────────────────────────────────────┘
```

#### Behavior

- **One question at a time** — AI presents current step only
- **Progress indicator** at top shows current step (e.g., "Adım 3/8")
- **Input field** at bottom for text answers
- **Upload buttons** appear when relevant (logo, product photos)
- **AI can ask follow-up questions** if the answer is unclear
- **Save point after each step** — if user closes browser, progress saved
- **Resume via email link** — user gets a link to continue where they left off
- **Free-form clarifying questions allowed** — user can ask "Ne yazmalım?" or "Nasıl bir açıklama olur?"

#### Step Details for Design Reference

| Step | Content | Input Type |
|------|---------|------------|
| 1 | Company name + logo | Text input + file upload (logo, max 5MB) |
| 2 | Business description | Free text area (what do you sell?) |
| 3 | Industry category | Visual template gallery (5 cards with preview images) |
| 4 | Products/services + photos | List builder, optional photo uploads |
| 5 | Target audience in Germany | Free text or guided multiple choice |
| 6 | Certifications/awards | Text tags (ISO, CE, TSE, etc.) |
| 7 | Contact info | Structured form (email, phone, WhatsApp, address) |
| 8 | Unique selling points | Free text (what makes you different?) |

#### After Step 8

- **Email capture screen:** "Mağazanıza ulaşmak için e-posta adresiniz"
- **Generate button:** "Alman Web Sitemi Oluştur" (primary, prominent)
- **Loading animation:** While generating, show progress steps:
  - ✨ İçerikler oluşturuluyor...
  - 🇩🇪 Almanca diline çevriliyor...
  - 📋 Yasal sayfalar hazırlanıyor...
  - 🖼️ Görseller optimize ediliyor...
  - ✅ Mağazanız hazır!

### 5.3 Storefront Template Design (German Side)

#### General Structure

Every storefront uses the same 6-section layout, with visual variations per template:

```
┌─────────────────────────────────────────┐
│ Navigation Bar                          │
│ [Logo] [Firma Adı] │ TR 🇩🇪 DE  │
├─────────────────────────────────────────┤
│ HERO SECTION                            │
│ Large hero image / company banner       │
│ Company name                            │
│ Transcreated tagline                    │
│ CTA: "Anfrage senden"                   │
├─────────────────────────────────────────┤
│ ÜBER UNS (About Us)                     │
│ Company story (transcreated for DE)     │
│ Optional: company photo, team photo     │
├─────────────────────────────────────────┤
│ PRODUKTE & DIENSTLEISTUNGEN             │
│ Product cards with image + description  │
│ Grid layout (2-3 columns)               │
├─────────────────────────────────────────┤
│ WARUM UNS WÄHLEN (Why Choose Us)        │
│ Certifications (logos + names)          │
│ Awards, quality seals                   │
│ USPs as visual badges                   │
├─────────────────────────────────────────┤
│ KONTAKT                                 │
│ Contact form (Name, E-Mail, Nachricht)  │
│ Email, phone, WhatsApp icons            │
│ Address (if provided)                   │
├─────────────────────────────────────────┤
│ RECHTLICHES (Legal)                     │
│ Links to Impressum, Datenschutz, AGB    │
│ Static legal pages (German compliant)   │
├─────────────────────────────────────────┤
│ Footer                                  │
│ "Erstellt mit turkish.directory"         │
└─────────────────────────────────────────┘
```

#### Template Variations (5 for MVP)

Design 5 distinct visual templates. Each should have:

- Unique color palette (header, accents, backgrounds)
- Different hero section treatment
- Different product grid layout
- Different typography hierarchy

**Template Names & Contexts:**

1. **Furniture** — Warm, premium feel, rich imagery, elegant fonts
2. **Textiles** — Fashion-forward, clean, pattern-based accents
3. **Food** — Appetizing, warm colors, focus on freshness/quality
4. **Industrial** — Strong, structured, technical, data-heavy
5. **Services** — Clean, trustworthy, professional, B2B style

### 5.4 Email Templates (to be designed)

**Email 1: Generation Started**
- Subject: "Alman mağazanız oluşturuluyor 🇩🇪" / "Ihre deutsche Storefront wird erstellt 🇩🇪"
- Content: "We've received your information. Your storefront is being built. This usually takes 1-5 minutes."
- Link: progress status (optional)

**Email 2: Storefront Ready**
- Subject: "Alman mağazanız hazır! 🎉" / "Ihre deutsche Storefront ist fertig! 🎉"
- Content: "Your storefront is ready! Click below to view and edit."
- CTA button: "Mağazamı Görüntüle" / "Storefront ansehen"
- Magic link

**Email 3: Inquiry to Turkish Company**
- Subject: "Yeni bir Alman müşteri ilgileniyor!" / "Neue Anfrage aus Deutschland!"
- Content: Inquiry details (name, email, message, storefront URL)

**Email 4: Inquiry to Superadmin**
- Subject: "New inquiry: [storefront name]"
- Content: Full inquiry details for monitoring

### 5.5 Admin Dashboard

#### Layout

```
┌─────────────────────────────────────────┐
│ Sidebar:                                │
│  🏠 Dashboard                           │
│  📦 Storefronts                         │
│  👥 Users                               │
│  🔧 Settings                            │
│  ➕ New Admin                           │
├─────────────────────────────────────────┤
│ Main Area:                              │
│  ┌─────┐ ┌─────┐ ┌─────┐ ┌─────┐       │
│  │Total│ │Active│ │Today │ │Avg.  │       │
│  │Stores│ │Stores│ │Created│ │Score│       │
│  └─────┘ └─────┘ └─────┘ └─────┘       │
│                                         │
│ Storefronts Table:                      │
│ ┌────┬────┬──────┬──────┬──────┬───┐     │
│ │Slug│Ind.│Date  │Status│Score │⋮ │     │
│ ├────┼────┼──────┼──────┼──────┼───┤     │
│ │    │    │      │      │      │   │     │
│ └────┴────┴──────┴──────┴──────┴───┘     │
│                                         │
│ Pagination, search, filter              │
└─────────────────────────────────────────┘
```

---

## 6. Component Library

### 6.1 Buttons

- **Primary:** Solid fill, rounded corners, prominent color. Used for CTAs.
  - TR: "Hemen Başla", "Alman Web Sitemi Oluştur", "Değişiklikleri Yayınla"
  - DE: "Jetzt Start", "Deutsche Storefront erstellen", "Änderungen veröffentlichen"
- **Secondary:** Outlined or ghost. Used for secondary actions.
- **Text link:** Underlined or colored. Used for navigation, "edit" actions.

### 6.2 Form Inputs

- **Text Input:** Clean, modern, with subtle border, focus state highlighted
- **Text Area:** Multi-line, larger minimum height
- **File Upload:** Drag-and-drop zone with file size limit shown (5MB max, 10 files max)
- **Tag Input:** For certifications — type and press Enter to add as a chip/tag

### 6.3 Cards

- **Template Cards:** Preview image on top, category name below, selection checkmark when selected
- **Product Cards:** Image, name (TR + DE), description (TR + DE)
- **Storefront Cards (Admin):** Slug, industry, date, status badge, quality score

### 6.4 Status Badges

- **Pending:** Orange/amber, "Hazırlanıyor"
- **Draft:** Gray, "Taslak"
- **Published:** Green, "Yayında" / "Live"
- **Editing:** Blue, "Düzenleniyor"

### 6.5 Icons

Use a consistent icon set (Feather Icons, Heroicons, or similar):
- 🌐 Language toggle
- 📎 Attachment / upload
- 📤 Send
- ✏️ Edit
- 🔗 Link/share
- 📧 Email
- 📱 WhatsApp
- ⭐ Star / quality
- 🏢 Company
- 🏭 Industry
- ✅ Published
- ⏳ Progress

### 6.6 Chat Interface Specifics

- **AI Bubble:** Left-aligned, light background, with avatar/robot icon
- **User Bubble:** Right-aligned, primary color, white text
- **Typing Animation:** Three bouncing dots while AI is "thinking"
- **Progress Header:** Persistent "Adım 3/8" with step dots or progress bar at top

---

## 7. Responsive Behavior

### Breakpoints

- **Mobile:** < 768px (primary device for Turkish SME users)
- **Tablet:** 768px - 1024px
- **Desktop:** > 1024px (German B2B buyers, admin dashboard)

### Mobile-First Priority

The conversational wizard is **mobile-first**. Most Turkish SME owners will use their phones (WhatsApp-style interaction). The chat interface must feel natural on a phone screen.

- Chat bubbles stack vertically
- Uploads use native camera/file picker
- CTA buttons are full-width on mobile
- Storefronts scroll vertically (same sections, adapted)

### Desktop

- Chat interface has wider input area
- Storefront: multi-column grids for products
- Admin dashboard: full sidebar + data tables

---

## 8. Navigation & Routing

### Public Routes

| Route | Description |
|-------|-------------|
| `/` | Landing page (TR or DE auto-detected) |
| `/s/<slug>` | Individual storefront (German side) |
| `/impressum` | Legal page template |
| `/datenschutz` | GDPR page template |
| `/admin` | Admin login |

### Reserved Slugs (Cannot be used for storefronts)

`/about`, `/pricing`, `/contact`, `/admin`, `/legal`, `/impressum`, `/datenschutz`, `/de`, `/tr`, `/api`, `/auth`, `/blog`, `/terms`, `/privacy`, `/s`

### Language Routing

- URL does **NOT** change with language (TR/DE is client-side toggle)
- Future: multilingual support (EN, RU, AR) via config addition

---

## 9. Copy & Microcopy Guidelines

### Turkish (TR) Copy
- **Tone:** Friendly, encouraging, colloquial but professional
- **Formality:** "Sen" (informal) — we're helping you, not lecturing you
- **Example:** "Hadi başlayalım! Önce şirketinizin adını öğrenebilir miyiz?"

### German (DE) Copy
- **Tone:** Professional, precise, trustworthy
- **Formality:** "Sie" (formal) — B2B business communication
- **Example:** "Ihr Unternehmen in Deutschland. Professionell, schnell,合规gerecht."

### Microcopy Rules
- **Always explain why** — "E-posta adresinizi verin, size mağazanızın linkini göndereceğiz"
- **Always show progress** — "Adım 3/8" is always visible
- **Always provide help** — Each step has a "?" button with context-sensitive help
- **Success messages are celebratory** — "🎉 Mağazanız hazır! Bir göz atın."
- **Error messages are specific** — "Bu dosya 5MB'dan büyük. Daha küçük bir dosya yükleyin."

---

## 10. Accessibility Requirements

- WCAG 2.1 AA minimum
- Keyboard navigable (all flows)
- Screen reader compatible
- Sufficient color contrast (4.5:1 minimum)
- Form inputs have labels (visually, not just `aria-label`)
- Language attribute changes in HTML when toggling (tr/de)
- Alt text for all images in storefronts
- RTL support reserved for future Arabic addition

---

## 11. Edge Cases & Error States

### Design Needed:

1. **AI Generates Poor Content** — "İçerik tekrar oluşturuluyor..." (retry screen, max 2 attempts)
2. **Generation Timeout** — "Storefront creation is taking longer than expected. We'll notify you via email."
3. **User Drops Out Mid-Flow** — Auto-save. User sees resume option via email link.
4. **Rate Limit Reached** — "24 saat içinde en fazla 1 mağaza oluşturabilirsiniz."
5. **Slug Collision** — Auto-append suffix (bursa-mobilya-2), show user the chosen slug
6. **Large File Upload** — Reject with clear message: "Dosya 5MB'dan büyük. Maksimum 5MB."
7. **No Internet** — Graceful degradation: "İnternet bağlantınızı kontrol edin."
8. **Storefront with Empty Fields** — AI fills placeholder text. Show "Bu alanı düzenleyin" badge.

### Loading States
- Skeleton screens for storefront preview
- Progress bar during generation
- Typing animation during AI chat

---

## 12. Out of Scope for Phase 1

- Payment / billing integration
- Custom domain mapping (only turkish.directory/<slug>)
- Arabic language support (RTL layout)
- English/Russian language support
- B2B matchmaking between German buyers and Turkish suppliers
- CRO agent (A/B testing)
- SEO content generation (autonomous blogging)
- Analytics dashboard for storefront owners
- Mobile apps
- Social media integration

---

## 13. Future Considerations (Design Should Anticipate)

### Phase 2: Auto-Directory
- Homepage with search functionality for German buyers
- Storefront listings on main page
- Filtering by industry, certification, location

### Phase 3: Buyer Side
- German buyer onboarding flow (in German)
- AI interview for buyer needs
- Matchmaking interface

### Phase 4: Autonomous Optimization
- Per-storefront analytics
- A/B test results display
- SEO content scheduling

### Multilingual Expansion
- Language toggle with 5+ languages (TR, DE, EN, RU, AR)
- Arabic requires RTL layout consideration in the design system
- Template designs should support RTL as a future requirement

### Admin Role Expansion
- Different admin role levels (viewer, editor, superadmin)
- Analytics with charts and export functionality
- Bulk operations on storefronts

---

## Technical Context for Designer

- **Frontend Framework:** Astro (static site generation, excellent for SEO)
- **Deployed on:** Vercel (automatic deployments from Git)
- **Backend:** Supabase (Database, Auth, Storage)
- **Email Service:** Resend API
- **AI Provider:** Qwen 3.6 Plus (primary, OpenRouter), fallback MiniMax M2.7
- **Storefront URLs:** turkish.directory/<auto-slug>
- **Image Optimization:** Automatic via Supabase Storage

---

## Deliverables Expected from Designer

1. **Landing Page** — Mobile + Desktop mockups (TR version)
2. **AI Chat Wizard** — Mobile + Desktop mockups (all 8 steps shown)
3. **Template Gallery** — How the 5 templates look to the user
4. **Storefront Templates** — 5 distinct designs (Desktop + Mobile), German aesthetic
5. **Email Templates** — 4 email designs (HTML-friendly)
6. **Admin Dashboard** — Desktop mockup
7. **Component Library** — Buttons, inputs, cards, badges, chat bubbles
8. **Loading & Error States** — For each major interaction
9. **Style Guide** — Colors, typography, spacing, icon set

---

*PRD compiled 2026-04-04 by Gonzo (OpenClaw)*
*Approved by Turgay (Founder, Superadmin)*
