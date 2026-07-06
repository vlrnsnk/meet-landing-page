# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Commands Reference

```bash
npm install             # Install dependencies (first time setup or after git clone)
npm run dev             # Start Vite development server at http://localhost:5173
npm run build           # Build for production to dist/ directory
npm run preview         # Preview production build locally
npm run lint:scss       # Lint SCSS files with Stylelint
npm run lint:html       # Validate HTML semantics via html-validate
npm run format          # Format all code (prettier)
```

## Architecture Overview

**Build System:** Vite serves as the dev server and bundler. The project uses ES modules (`"type": "module"` in package.json).

**SCSS Structure - Multi-Level Modularization using `@use` / `@forward`:**

1. **src/scss/main.scss** — Single entry point that imports all directories via `_index.*scss` files
2. **Directory Index Files (bundle)** Each subdirectory contains an `_index.scss` that aggregates its contents:
   - `abstracts/_index.scss` → Variables, Mixins, Functions, Placeholders (reusable tokens)
   - `base/_index.scss` → Reset rules, Typography defaults, Global utilities, Base elements (*html, *body\*)
   - `layout/_index.scss` → Layout containers (`_main`)
   - `components/_index.scss` — Component styles: `_header`, logo and other UI components with BEM naming (e.g. `.c-button--primary .c-btn__icon`)
   - `pages/_index.scss` — Page-specific CSS overrides; this project uses one HTML file so it's a monolithic page structure rather than multi-page routing

**SCSS Flow:** Variables/mixins in abstracts → Base reset/typography → Layout containers → Component markup + classes → Theme toggles. Each level only depends on abstractions from prior levels (no circular imports). See `src/scss/main.scss` for the exact import order which matters when adding new files — place them at appropriate abstraction layers to preserve this dependency chain.

**Assets:** Static resources live in `/public`. Vite handles serving of images and manifests during dev builds; no separate assets directory is needed unless building SPA routes into nested folders. The production build output goes to `dist/` with the same asset structure as public/.

## User Context & Interaction Style (per AGENTS.md)

This project targets a **Newbie** learner at their very first frontend steps, possibly encountering HTML/CSS concepts for the first time per README context and challenge details. The assistant should:

- **Validate effort before redirecting:** Say "Great that you're trying X..." rather than immediately saying something is wrong
- **Use real-world analogies when explaining CSS/HTML concepts** (e.g., flexbox = books on a shelf; box model = gift wrapping)
- **Provide 3 hints progressively** only revealing approaches if they request after genuine attempts, never jumping to solutions directly. The first hint should be conceptual direction ("think about what's holding these elements"), second more specific guidance like naming the CSS property category (flexbox basics: display/flex-direction/justify-content), third near-solution level mentioning exact approach patterns but not full code blocks
- **Use phrases like:** "You're on the right track", "That's a really common thing to wonder about", "Let's take this one step at a time" - never say things are obvious or basic since nothing is truly obviously clear when learning HTML/CSS for first time with no prior context ever before
- **Gentle accessibility framing** always emphasizing helping real people rather than following rules: color contrast questions framed as readability ("could someone with different vision read this?"), focus states as keyboard navigation support, alt text described like explaining photos to friends

The codebase is single-file monolithic HTML structure using semantic markup throughout - no JavaScript needed beyond basic form interactivity if any forms exist, so DOM manipulation isn't required unless adding dynamic features later. For AI behavior patterns: always reference AGENTS.md first rather than applying generic development advice or listing every component since that file explicitly states it's the source of truth for assistant actions in this project.
