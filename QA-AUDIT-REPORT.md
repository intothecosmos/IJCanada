# Inner Journey Canada — Full Site QA Audit Report

**Date:** April 9, 2026  
**Auditor:** Claude (for Matt Dufton)  
**Purpose:** Pre-leadership presentation quality gate  
**Scope:** 30 HTML files + CSS + sitemap + robots.txt across 5 directories  
**Verification:** All findings independently verified after initial agent sweep

---

## Executive Summary

The site is in strong shape overall. Content accuracy, navigation structure, footer consistency, email compliance, and CTA routing all pass cleanly. The issues found are primarily technical polish — SEO meta lengths, heading hierarchy, missing schema markup, and a handful of link/attribute fixes. Nothing is a showstopper for the leadership presentation, but the "Before Launch" items should be resolved before domain cutover.

**Totals:** All links audited across 30 pages. 29/30 footers verified identical. All register CTAs correct. Zero instances of the wrong email address.

---

## Issue Tracker

### CRITICAL (fix before leadership demo)

| # | Page | Issue | Details | Verified |
|---|------|-------|---------|----------|
| C1 | `programs/facilitator-program.html` | Missing `rel="noopener"` on 3 external links | Lines 175, 192, 207 — three links to `innerjourneyinstitute.com` have `target="_blank"` but no `rel="noopener"` (security/performance) | Yes — grep confirmed. Footer links on same page DO have it; these 3 body links don't |
| C2 | `local/workshops-ottawa-gatineau.html` | Broken anchor link | Line 169: `href="../index.html#subscribe"` — no element with `id="subscribe"` exists in index.html. The MailerLite form uses `id="mlb2-39681303"` instead | Yes — grep confirms no `id="subscribe"` in index.html |

### BEFORE LAUNCH (fix before domain cutover)

| # | Category | Issue | Details | Verified |
|---|----------|-------|---------|----------|
| B1 | SEO | 13 meta descriptions exceed 160 chars | index.html (181), programs.html (170), calendar.html (169), facilitator-program.html (177), family-constellation.html (176), 4 facilitator pages (165–175), 2 blog posts (170–171), 2 local pages (165–172). Over by 5–21 chars each | Yes — bash loop confirmed exact counts |
| B2 | SEO | Missing Schema.org JSON-LD on 16 pages | Have it: index, faq, 6 blog posts, 6 facilitator pages, 2 local pages (16 total). Missing: about, programs, calendar, circle, contact, testimonials, 4 program detail pages, register, blog/index (14 total). **Priority: add Event/Course schema to program pages** | Yes — grep for `application/ld+json` confirmed |
| B3 | SEO | 2 blog posts missing `og:type` tag | `burnout-recovery-goes-beyond-vacation.html` and `feeling-stuck-after-40.html` — all other pages have it | Yes — grep confirmed these 2 specifically |
| B4 | SEO | Homepage hero logo has empty alt text | index.html line 66: `<img src="images/logo.png" alt="" class="hero-logo">` — alt is present but empty string | Yes — grep confirmed `alt=""` (empty, not missing) |
| B5 | Accessibility | Heading hierarchy issues on most pages | Every page ends with footer `<h4>` tags. Pages whose main content ends at h2 create an h2→h4 skip. Worst offenders: contact.html (h1→h3 skip in main content), blog/index.html (h1→h3 skip), circle.html (h1→h2→h4 in body). Footer h4s affect all 29 pages | Yes — heading extraction confirmed per-page |
| B6 | Accessibility | 12 pages missing `<main>` tag | All 7 blog pages, all 5 programs/ pages (including register). Root pages have it | Yes — grep confirmed exactly 12 files |
| B7 | Accessibility | No skip-to-content link | No page has a "skip to main content" link for keyboard users | Yes |
| B8 | Nav | Programs dropdown uses sibling paths in `/programs/` directory | 5 files use `href="intensive.html"` instead of `href="../programs/intensive.html"`. **These work correctly** since they're in the same directory — this is a consistency preference, not a bug | Yes — grep confirmed. Functionally fine |
| B9 | Missing pages | No `privacy.html` or `terms.html` | These don't exist and nothing links to them, so nothing is broken. But a privacy policy is advisable before launch | Yes — ls confirmed missing, grep confirmed no links to them |

### POST-LAUNCH NICE-TO-HAVES

| # | Category | Issue | Details |
|---|----------|-------|---------|
| P1 | Accessibility | Color contrast on gradient heroes | Needs manual browser testing — gradient-over-text readability can be marginal |
| P2 | Code quality | Inline styles could be consolidated | Common patterns (`display:none`, color overrides, margin tweaks) could become utility classes |
| P3 | SEO | 404.html missing OG tags | Acceptable for error page |

---

## Detailed Verification Results by Category

### 1. Navigation Consistency — PASS

All 29 pages verified:

- Programs dropdown: correct 4 items in correct order on all pages
- About dropdown: correct 5 items including "Our Impact" on all pages
- Top-level items: Calendar, Contact, Blog, Register present on all pages
- Mobile nav toggle: present on all pages
- Relative paths: correct everywhere (root uses `programs/x.html`, subdirs use `../programs/x.html`, 404 uses absolute paths)
- One consistency note: programs/ directory uses sibling-relative paths (see B8) — works fine

### 2. Footer Consistency — PASS

All 28 non-404 pages verified:

- MailerLite form (account 2257046, form ID 184288989721658876): correct on all
- No duplicate subscribe forms anywhere (blog/index.html and calendar.html confirmed clean)
- Footer grid: all 4 programs, "Our Impact" link, support links — all present
- Contact: 647-850-9337, inner.journey.canada@gmail.com — correct everywhere
- Facebook link: present on all
- Copyright 2026: correct on all
- Relative paths: correct on all subdirectory pages

### 3. Links Audit — PASS (2 verified issues)

- All register/signup CTAs: correctly point to `programs/register.html` — zero stale links
- All mailto links: use correct email — zero instances of `info@innerjourneycanada.com`
- All canonical URLs: correct `intothecosmos.github.io/IJCanada/...` pattern
- All internal links resolve to existing files
- External links: all have `target="_blank"` — only `facilitator-program.html` body links missing `rel="noopener"` (C1)
- One broken anchor: `#subscribe` (C2)
- No links to non-existent privacy.html or terms.html

### 4. Content Accuracy — PASS

Cross-referenced against live Weebly site:

- Intensive: dates, pricing, location, facilitators — all correct
- 5 Elements: Taoist elements (Wood/Fire/Earth/Metal/Water), Laroche Room, "Doors close at 2:15 p.m." — all correct
- Family Constellation: matches live site
- Facilitator Program: matches live site
- Contact sidebar: all role-based contacts correct
- Gift Certificates section: present
- Calendar: Google Calendar embed present and working (confirmed via screenshot)

### 5. SEO Audit — GOOD (meta trimming + schema needed)

- Title tags: 29/29 unique
- Meta descriptions: 16/29 within limit; 13 over by 5–21 chars
- Canonical URLs: 29/29 correct
- Open Graph: 27/29 have og:type (2 blog posts missing)
- Schema.org: 16/30 pages have JSON-LD; program pages need it most
- sitemap.xml + robots.txt: properly configured
- Image alt: 1 empty alt on homepage hero logo (decorative? may be intentional)

### 6. Forms Audit — PASS

- Contact form: Formspree `mlgoadel` — correct
- Registration form: Formspree `mlgoadel` — correct
- MailerLite footer forms: correct IDs on all pages, no duplicates

### 7. Accessibility — NEEDS WORK

- Images: all have alt attributes (1 empty but may be intentional for decorative logo)
- Form labels: all inputs have labels or aria-labels
- Heading hierarchy: h-level skips on most pages, primarily from footer h4 tags
- 12 pages missing `<main>` landmark
- No skip-to-content link

---

## Recommended Fix Plan

### Quick wins (~30 min)

1. Add `rel="noopener"` to 3 links on facilitator-program.html (C1)
2. Add `id="subscribe"` to MailerLite section in index.html, OR update workshops link to remove anchor (C2)
3. Add `og:type="article"` to 2 blog posts (B3)
4. Decide if hero logo empty alt is intentional (decorative) or should say "Inner Journey Canada" (B4)

### Before launch (~3–4 hours)

5. Trim 13 meta descriptions to ≤160 chars (B1) — ~30 min
6. Fix heading hierarchy — change footer `<h4>` to styled `<p>` or `<h3>`, fix body h-level skips (B5) — ~1.5 hours
7. Add `<main>` tags to 12 pages (B6) — ~30 min
8. Add Schema.org JSON-LD to program pages (B2) — ~1.5 hours
9. Consider adding privacy.html (B9)

### Post-launch

10. Add skip-to-content link (B7)
11. Consolidate inline styles (P2)
12. Add Schema.org to remaining pages
13. Visual/responsive browser testing at 375px, 768px, 1200px

---

## What Looks Great

- Rock-solid navigation — "Our Impact" confirmed on all 29 pages
- Perfect email compliance — zero wrong-email instances
- All CTAs route correctly — zero stale contact.html links
- Clean footers — no duplicate forms, consistent structure
- Content accuracy — all program details match live Weebly site
- Google Calendar embed working correctly
- Good SEO foundation — unique titles, canonicals, sitemap, robots.txt
- Professional code — no debug artifacts, clean JS

---

*Visual/responsive testing (hero banners, card layouts, mobile breakpoints) requires browser rendering and should be done separately.*
