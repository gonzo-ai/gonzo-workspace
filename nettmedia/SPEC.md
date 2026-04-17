# SPEC.md — Storefront Generator (Phase 1)

> turkish.directory — Agentic Export Engine
> Created: 2026-04-04
> Status: Spec complete, ready for technical architecture

---

## 1. Product Overview

**Turkish.directory** starts as a conversational AI Storefront Generator. A Turkish SME visits the site, answers questions step-by-step in a chat interface (in Turkish), and within minutes receives a German-compliant storefront. The storefront is hosted on turkish.directory, serves both TR and DE languages, and includes legal compliance pages.

Phase 1 is **100% free**. No monetization is built in. The goal is to prove the product works, get real users, and grow the directory. Later, a 3-tier pricing model applies.

---

## 2. User Journey (Step-by-Step)

### 2.1 Landing
1. User visits `turkish.directory`
2. Browser language is auto-detected
3. If language is Turkish → AI greets in Turkish
4. If language is German → AI greets in German  
5. Otherwise → defaults to Turkish with a language switcher visible
6. User can switch between **TR** and **DE** at any time via language toggle button (🌐 TR | DE)

### 2.2 Onboarding (Conversational Wizard)
7. User clicks "Start" or types in chat
8. AI presents a **guided, step-by-step flow** — one question at a time, but user can ask free-form clarifying questions in Turkish
9. Progress saves automatically after each step

**Collection Steps (8 total):**
1. Company name + logo upload
2. Business description (what do you sell?)
3. Industry category selection (from gallery of 5 templates)
4. Products/Services list + optional photo uploads (up to 10 images, 5MB each)
5. Target audience in Germany (who are you selling to?)
6. Certifications, awards, quality standards
7. Contact information (email, phone, WhatsApp, address)
8. Unique selling points (why should Germans buy from you?)

### 2.3 Generation
9. User clicks "Generate My Storefront"
10. System starts async generation (visible: loading spinner with progress)
11. User receives email: "Your German storefront is being built..."
12. Generation completes (1-5 minutes usually)
13. User receives email: "Your storefront is ready! [Magic Link]"
14. User clicks magic link → views live storefront
15. User can toggle between TR/DE to review both versions

### 2.4 Post-Generation Editing
16. Storefront shows floating "✏️ Edit" button on each section
17. Clicking opens AI chat: "What would you like to change about this section?"
18. User describes changes in free-form Turkish
19. AI regenerates that section only → live preview → "Publish" to confirm
20. Changes save to Supabase, storefront updates

---

## 3. Frontend Architecture

### 3.1 Framework
- **Astro** — static site generation, optimal SEO, native multilingual support, fast pages
- Frontend deployed on **Vercel** (free tier initially)

### 3.2 URL Structure
- Storefronts: `turkish.directory/<storefront-slug>` (no prefix needed)
- Landing page: `turkish.directory`
- Future safe: storefront slugs are validated against reserved routes (`/about`, `/pricing`, `/admin`, etc.)
- Multi-language support built-in: Astro's i18n routing. Language switcher changes content, not URL.

### 3.3 Template Categories (5 for MVP — expandable)
- Furniture
- Textiles
- Food
- Industrial
- Services

Templates are selectable from a **visual gallery** during onboarding (user picks, not AI). Each template includes:
- Pre-designed German-aesthetic layout (trust-focused, data-rich, minimalist)
- Section layout matching the 6-storefront-content-sections model
- Mobile-responsive (German users browse on mobile too)

### 3.4 Storefront Content Sections
Every generated storefront includes these 6 sections:
1. **Hero** — Company name, transcreated tagline (DE/ TR), hero image/logo
2. **About Us** — Company story, AI-transcreated for German audience
3. **Products/Services** — Catalog with descriptions, photos
4. **Why Choose Us** — Certifications, awards, USPs (German trust signals)
5. **Contact** — Email, phone, WhatsApp, inquiry form
6. **Legal** — Impressum (template + filled data), Datenschutzerklärung (GDPR template + filled data), AGB placeholder

### 3.5 Image Handling
- Upload via Supabase Storage
- **AI auto-optimizes:** compresses, crops, enhances for German storefront aesthetic
- Auto-selects best hero image from uploaded set
- Max: **5MB per image, 10 images total** per storefront
- v2: AI generates complementary stock-style images (Phase 1: real photos only)

### 3.6 Language Support
- **Phase 1:** TR + DE only
- Language switcher: toggle button (🌐 TR | DE)
- Content changes dynamically without URL change (Astro i18n + client-side)
- RTL (Arabic) and additional languages (EN, RU) — **not in Phase 1**, architecture supports future addition via config

---

## 4. Backend Architecture

### 4.1 Supabase Cloud
- **Postgres** — database for storefront data, users, templates
- **Auth** — magic link email authentication
- **Storage** — image uploads, generated storefront files
- Free tier initially (500MB DB, 1GB storage). Upgrades at ~100+ users ($25/mo)

### 4.2 Database Schema (Core Tables)
```sql
-- Storefronts
id (uuid) PK
user_id (uuid) FK → auth.users
slug (text) UNIQUE — URL-safe, validated against reserved routes
company_name (text)
company_name_de (text) — AI-transcreated
industry (text) — template category
template_id (text) — which template was used
about_tr (text)
about_de (text) — AI-transcreated
products (jsonb) — array of { name_tr, name_de, description_tr, description_de, image_urls[] }
contact (jsonb) — { email, phone, whatsapp, address_tr, address_de }
certifications (text[])
usps (text[]) — unique selling points
quality_score (integer) — 1-5, AI self-review score
status (enum: 'pending', 'draft', 'published', 'editing')
created_at (timestamptz)
updated_at (timestamptz)

-- Conversations (for resuming progress)
id (uuid) PK
user_id (uuid)
current_step (integer) — which step user is on
answers (jsonb) — partial data collected so far
created_at (timestamptz)

-- Templates
id (text) PK — 'furniture', 'textiles', 'food', 'industrial', 'services'
name (text)
preview_image_url (text)
config (jsonb) — template-specific settings
```

### 4.3 Auth Flow
- Magic link email only (Supabase native auth)
- No passwords, no SMS, no Google login
- User enters email → Supabase sends link → click → authenticated
- Link expires after 24h

---

## 5. AI Generation Pipeline

### 5.1 Models
- **Primary: Qwen 3.6 Plus** (OpenRouter) — copywriting, translation, transcreation (TR↔DE), HTML generation
- **Model is swappable** — no lock-in. DeepSeek or others can be dropped in later without architectural changes

### 5.2 Generation Steps
1. **Copywriting** — AI generates German storefront content from user answers (hero tagline, about page, product descriptions, USPs)
2. **Transcreation** — AI reviews copy for German tone (formal, professional, trust-building) vs Turkish (relationship-driven)
3. **HTML Generation** — AI generates complete HTML file from chosen template + content
4. **Image Optimization** — AI compresses/enhances uploaded images, selects best hero
5. **Legal Pages** — Template-based Impressum/Datenschutzerklärung with user data filled into placeholders
6. **Quality Gate** — AI scores its own output (1-5). If score < 4: regenerate
7. **Deploy** — Store generated HTML in Supabase Storage
8. **Serve** — Astro page renders storefront from template + data

### 5.3 System Prompt (Transcreation)
The AI understands:
- **Turkish business communication:** warm, relationship-driven, emotional
- **German business expectations:** data-focused, trust-signal-heavy, formal, professional
- The AI must **transcreate** — not translate. German copy should sound like it was written by a native German marketing professional, not a translator.

---

## 6. Error Handling

### 6.1 AI Quality
- Systematic self-review: after generation, AI scores output 1-5
- Score ≥ 4 → publish. Score < 4 → regenerate once
- Max 2 regeneration attempts
- If still < 4: user gets email "Your storefront needs manual review. We'll notify you when ready."

### 6.2 Timeouts/Failures
- Auto-retry 2x on AI API timeout
- After 2 failures: email user "We're still working on it. This should be ready within 24 hours."
- Admin notification sent for manual follow-up

### 6.3 Dropouts
- Progress auto-saves after every step
- If user closes browser → they get email: "Your storefront is almost ready! Continue here: [magic link]"
- Link takes them back to exactly the step they left off

### 6.4 Rate Limiting
- Max 3 storefronts per email address
- hCaptcha on landing page (first page only)
- Max 1 storefront generation per 24h per user

---

## 7. Admin Dashboard

Admin has full visibility into all storefronts and users:

- **List View** — All storefronts with status, creation date, user email, quality score
- **Search** — By company name, user email, industry category
- **Management** — Publish/unpublish storefronts, edit manually if needed
- **Analytics** — Number of storefronts, active users, average generation time
- Admin access via `turkish.directory/admin` (magic link auth, separate from regular users)

---

## 8. Reserved Routes

These paths cannot be used as storefront slugs:
`/about`, `/pricing`, `/contact`, `/admin`, `/legal`, `/impressum`, `/datenschutz`, `/s`, `/de`, `/tr`, `/api`, `/auth`, `/blog`, `/terms`, `/privacy`

If user tries to use a reserved slug (e.g., company name is "About"), auto-append suffix: `/about-store`, `/about-shop`, etc.

---

## 9. Infrastructure

| Component | Service | Plan | Cost |
|-----------|---------|------|------|
| Frontend | Vercel | Free | $0 |
| Backend | Supabase Cloud | Free → Pro at scale | $0 → $25/mo |
| AI Copy | Qwen 3.6 Plus (OpenRouter) | Free tier | $0 |

**Total Phase 1 monthly cost: $0-25**

---

## 10. Deployment & CI/CD

- Frontend: Deployed via Vercel (GitHub integration — push to `main` = deploy)
- Supabase migrations: manual via CLI or dashboard
- Environment variables managed via Vercel + Supabase dashboard
- No complex CI/CD. Keep it simple.

---

## 11. Phase 2-4 Roadmap (Not in Scope for This Spec)

**Phase 2: Auto-Directory** — Every storefront auto-lists on turkish.directory homepage. Search/browse functionality for German buyers.

**Phase 3: Buyer Side** — German buyers land, search for suppliers, AI interviews them, matches with Turkish companies.

**Phase 4: Autonomous Optimization** — CRO agent (A/B tests), SEO agent (writes German blogs), self-optimizing storefronts.

---

## 12. Open Questions

1. **Domain status** — Where are nameservers pointing? Is turkish.directory currently parked or does it have a site?
2. **Email service** — Supabase sends magic links via their default sender. When do we switch to a custom domain email (noreply@turkish.directory)?
3. **Admin auth** — Same magic link system as regular users, or a separate admin-only login?
4. **Storefront slug strategy** — Auto-generate from company name (`bursa-furniture`) or let user choose?
5. **Inquiry form on storefronts** — Does the inquiry form send email to the Turkish company? Needs email routing setup.
6. **Legal review** — Have we had a German lawyer review the Impressum/Datenschutz templates? (Should do this before going live)

---

*Spec completed via interview with Turgay on 2026-04-04*
*Next step: Technical architecture review → Sprint planning*
