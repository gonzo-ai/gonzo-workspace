# Storefront Generator — Phase 1 Design

> Decision date: 2026-04-04
> Status: Phase 1 approved, ready for technical architecture

---

## What We're Building

A conversational AI wizard on **turkish.directory** where Turkish SMEs answer questions in Turkish, walk away with a German-compliant storefront (subdirectory: `turkish.directory/firmaismi`). 

The storefront is generated from a pre-built template, populated with AI-generated content, and served via Supabase. Users authenticate via magic link email.

## Why This Approach

- **Conversational AI form** = lowest friction for non-tech-savvy users
- **Pre-built templates** = reliable quality, fast generation, easy to iterate
- **Static HTML initially** = cheap, simple, works — upgrade later if needed
- **Supabase** = single backend for auth, database, storage — scale ready
- **Subdirectories** = simple routing, all under one domain
- **Magic link email** = free, Supabase native, no SMS cost
- **Flywheel** = each customer auto-adds to directory → more buyers → more sellers

## Key Decisions

| Decision | Choice | Why |
|----------|--------|-----|
| Entry strategy | Inside-out (build demo stores by hand first) | No existing data, need proof of concept |
| AI form style | Conversational (asks follow-ups) | Feels natural, not like filling forms |
| Templates | Pre-built 3-5 industry templates first | Reliable, proven, can evolve to AI-generated later |
| Storefront tech | Static HTML initially | Fast to ship, cheap, can upgrade later |
| Routing | Subdirectories (`turkish.directory/firmaismi`) | Simple |
| Backend | Supabase (Postgres, Auth, Storage) | All-in-one, scale-ready |
| Auth | Magic link email | Free, Supabase native, no SMS cost |
| Sequence | Build generator first → directory fills itself | Flywheel without chicken-and-egg problem |

## Full Feature Roadmap

```
PHASE 1: Storefront Generator (now)
  Turkish SME enters info → AI conversation → German storefront

PHASE 2: Auto-Directory
  Every storefront auto-adds to turkish.directory
  Real, growing directory of verified TR→DE companies

PHASE 3: Buyer Side
  German buyers search, AI interviews, matches with Turkish suppliers
  Bilingual auto-matchmaking

PHASE 4: Autonomous Optimization
  CRO agent A/B-tests storefronts
  SEO agent writes German blog content
  Everything optimizes itself
```

## Revenue Model

| Tier | Service | Price |
|------|---------|-------|
| Tier 1: Visibility | AI-optimized multilingual listing + SEO | €99/month |
| Tier 2: Expansion | Autonomous German storefront + Ad management | €999/month |
| Tier 3: Enterprise | Full-scale B2B Matchmaking Agent | 1-3% Trade Commission |

**The math:** 0.1% of $20B TR-DE trade = €20M GMV. Small commission = multi-million € business.

## Open Questions

1. How many demo storefronts to build by hand? (2-3 enough?)
2. Onboarding language flow? (Turkish first, German second, English admin?)
3. First 3-5 industry templates? (furniture, textiles, food, industrial, services?)
4. Admin panel for review/manage storefronts?
5. Pricing shown upfront or after generation?
6. Domain status — nameservers pointing somewhere, or parked?

---

*Captured from brainstorming session with Turgay via Gonzo*
