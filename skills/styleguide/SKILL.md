---
name: styleguide
description: Generate a static HTML brand/styleguide exploration page for any project. Explores the codebase for existing design patterns, asks multi-select questions about aesthetic direction, and produces a self-contained HTML file with multiple visual theme options (color palettes, typography, UI components, and wireframes). Use when the user wants to explore visual directions, brand options, or redesign ideas.
argument-hint: "[output-filename]"
allowed-tools: Read, Glob, Grep, Write, AskUserQuestion
---

# Styleguide Generator

Generate a standalone, static HTML brand/styleguide exploration page with multiple visual theme options for the current project. This file is temporary and independent of the main codebase.

## Phase 1 — Explore the Codebase

Before asking any questions, silently explore the project to understand the current design baseline:

- Search for CSS files, stylesheets, design tokens, Tailwind config, theme files
- Look for color variables, font imports, spacing scales, border-radius patterns
- Check for any existing brand guidelines, readme mentions of design, or style comments
- Note the framework/stack in use (Astro, Next.js, plain HTML, etc.)
- Note any existing fonts (Google Fonts imports, local fonts, font-face declarations)

Summarize your findings briefly to the user (2–4 lines), then proceed to Phase 2.

## Phase 2 — Ask Clarifying Questions

Use the AskUserQuestion tool to ask the following. **All questions must use multiSelect: true** so the user can pick multiple answers if they're torn or want hybrid options reflected in the output.

Ask all questions in a single AskUserQuestion call (up to 4 at a time), then follow up with a second call if needed.

**Round 1 — big picture:**

1. **Vibe / purpose** — What should the site feel like to visitors?
   - Personal portfolio (expressive, shows personality)
   - Professional brand (polished, credibility-focused)
   - Creative playground (experimental, bold, artsy)
   - Community / open source hub (welcoming, collaborative)

2. **Aesthetic mood** — Which directions are you drawn to? (pick all that feel right)
   - Minimal & clean (whitespace, understated, refined)
   - Bold & editorial (big type, strong contrast, magazine-like)
   - Dark & moody (dark backgrounds, depth, glows)
   - Warm & organic (earthy tones, rounded, human)
   - Retro / nostalgic (vintage print, old web, pixel charm)
   - Futuristic / techy (grids, data aesthetics, precision)

**Round 2 — specifics:**

3. **Color direction** — What palette families interest you?
   - Neutrals + single bold accent (monochrome pop)
   - Warm tones (ambers, oranges, terracottas, creams)
   - Cool tones (teals, cyans, blues, slate)
   - Earth tones (olive, rust, sand, forest)
   - High contrast neons on dark (electric accents)
   - Pastels / muted (soft, desaturated, airy)

4. **Typography style** — What type direction excites you?
   - Expressive display + clean body (bold headlines, readable prose)
   - All serif (editorial, literary, refined)
   - All sans-serif / geometric (modern, systematic)
   - Monospace / techy (developer-adjacent, handcrafted feel)
   - Mixed: serif headlines + sans body
   - Mixed: sans headlines + serif body

## Phase 3 — Design the Options

Based on the user's answers, design **3 distinct theme options**. Each should be a genuinely different direction — not minor variations. If the user selected multiple mood/color preferences, distribute them across the options (e.g., one option leans toward their first pick, another toward their second, a third is a creative hybrid).

For each option, define:
- A short evocative name (2–4 words, e.g. "Dark Editorial", "Warm Monochrome", "Electric Blueprint")
- A 1–2 sentence description of the mood and feel
- A color palette (6 swatches: background, surface, primary text, secondary/muted text, accent, accent-dim)
- Typography pairing (display font + body font from Google Fonts, or system stack)
- Border radius personality (sharp/0px, subtle/4–6px, rounded/8–12px, pill/full)
- Shadow personality (none, subtle, dramatic)

## Phase 4 — Generate the HTML File

Determine the output filename:
- If `$ARGUMENTS` is provided, use that as the filename
- Otherwise, default to `styleguide.html` in the project root

Generate a **single, self-contained static HTML file** with:

### Required Sections (per option)

Each option must show:

1. **Color palette** — Swatches with hex values and role labels (Background, Surface, Text, Muted, Accent, Accent Dim)

2. **Typography specimen** — 4 rows:
   - Display/H1: large, full weight, shows the headline font
   - H2/Subheading: medium size
   - Body paragraph: readable text, 2–3 sentences
   - Label/Caption: small, metadata style (monospace or light)
   Use Google Fonts CDN links in the `<head>` for all non-system fonts.

3. **UI components** — Show realistic previews of:
   - Navigation bar or sidebar nav with active state
   - Primary and secondary/ghost buttons
   - Tags or chips
   - A project/content card (image placeholder + title + description)

4. **Layout wireframe** — A simplified browser-chrome wireframe showing the approximate page layout using colored blocks. Use opacity/color fills from the palette rather than placeholder gray blocks.

### HTML File Requirements

- Fully self-contained (no external CSS files, no JS frameworks)
- All fonts loaded via Google Fonts `<link>` in `<head>`
- Responsive (works at 1200px+ desktop, degrades gracefully on mobile)
- Page header at the top with: title ("Brand Style Exploration"), project name (infer from package.json, README, or directory name), date
- Jump navigation pills linking to each option section
- Each option in its own `<section>` with a clear visual separator
- A "How to view this file" footer with server examples (see below)
- The file should be visually impressive and high-fidelity — not a rough sketch

### "How to View This File" Footer

Include a styled footer section with copy-pasteable commands for running a local server:

```
# Python 3
python3 -m http.server 8080

# Node / npx
npx serve . --port 8080

# Node / http-server
npx http-server . -p 8080 -o

# PHP
php -S localhost:8080

# VS Code Simple Browser
# Command palette → "Simple Browser: Show"
# Enter: file:///ABSOLUTE/PATH/TO/styleguide.html
```

Replace `styleguide.html` with the actual filename used. Replace the absolute path with the real path based on the project's working directory.

Show instructions for opening in VS Code Simple Browser separately, as it doesn't require a server (direct file:// URL).

## Phase 5 — Offer Regeneration

After generating the file, tell the user:
- The file path
- The quick-open command for VS Code Simple Browser (with the full `file://` URL)
- All 3 server command options (Python, npx serve, npx http-server)

Then ask a final question using AskUserQuestion:

**"Would you like to regenerate any of the options?"**
- Regenerate Option A — [name]
- Regenerate Option B — [name]
- Regenerate Option C — [name]
- All look good — done!

Use multiSelect: true. If the user selects one or more options to regenerate, ask follow-up questions to understand what they want changed for each selected option (mood shift? different color family? different fonts?), then rewrite only those sections in the HTML file and re-present the regeneration offer.

If the user selects "All look good", confirm the file is ready and remind them it's a temporary file to delete when done.

## Notes

- Never commit this file to git. Mention that it's excluded if the project has a .gitignore, or suggest adding `styleguide.html` to .gitignore.
- Keep all styles inline (`<style>` block in `<head>`). No external CSS files.
- Use CSS custom properties (variables) per-option for easy modification later.
- The wireframe should use the option's actual color palette — not generic gray boxes.
- Typography samples should use realistic content, not Lorem Ipsum. Use the site's domain/context (e.g., the person's name, project names, realistic nav items).
