# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## What This Is

Static landing page for "AutoMate" — an AI automation agency website. Single `index.html` file, no build step, no dependencies to install.

## Development

Open `index.html` directly in a browser. No server required (though a local server avoids CORS issues if testing the chat webhook).

## Architecture

Everything lives in one `index.html`:

- **Styling**: Tailwind CSS via CDN (`cdn.tailwindcss.com`) + inline `<style>` block for animations (float, scroll-reveal, dropdowns, mobile menu, language toggle)
- **Icons**: Lucide via CDN (`unpkg.com/lucide`). After any DOM change that adds icons, call `lucide.createIcons()`
- **i18n**: Client-side ES/EN translation system. All translatable text uses `data-i18n` attributes (or `data-i18n-placeholder` for inputs). Translation strings stored in `translations` object in the `<script>` block. `switchLang(lang)` applies translations via `innerHTML`
- **Chat widget**: Bottom-right floating chat that POSTs `{ chatInput: text }` to an n8n webhook (`N8N_WEBHOOK_URL` constant). Expects JSON response with `output` property. Has fallback message when webhook URL is empty
- **Scroll reveal**: IntersectionObserver-based `.reveal` class with `.reveal-delay-N` variants (1-6, each +0.1s)

## Key Patterns

- WhatsApp number: `50253638941` — hardcoded in multiple `wa.me` links and updated dynamically in `switchLang()`
- Google Calendar booking link: `https://calendar.app.google/eMvTYs113hdH7R157` — used in nav CTA, hero, and contact section
- Adding new translatable content: add `data-i18n="key_name"` to the element, then add entries to both `translations.es` and `translations.en`
- Mobile menu and desktop nav have separate DOM trees with duplicated service links — changes to nav items must be made in both places
