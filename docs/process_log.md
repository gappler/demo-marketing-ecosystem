---
title: Process Log
version: 1.0
---

## 2026-04-11 — Switch body font to Inter

**Prompt:** Change the body font-family from '-apple-system, BlinkMacSystemFont, Segoe UI, Roboto, sans-serif' to 'Inter, sans-serif'. Per CLAUDE.md, Inter is the primary font for all demos.

**Built:** Updated the `body` rule in `index.html` to `font-family: 'Inter', sans-serif;`, removing the system-font stack. Inter is already loaded via the Google Fonts link in `<head>`.

**Key decisions:**
- Used `'Inter', sans-serif` (matching the minimal form the user provided) rather than keeping the `-apple-system`/`BlinkMacSystemFont` chain as a fallback — per the new CLAUDE.md Fonts rule, system fonts must not be used as the primary font, and a bare `sans-serif` fallback handles the Google Fonts load failure case without reintroducing platform system fonts.
- Left the explicit `font-family: 'Inter', ...` declarations on `.hero-title`, `.hero-subtitle`, and the footer untouched — they're now redundant with the body rule, but removing them would be a no-op cleanup outside the scope of this task.

**Files modified:**
- `index.html`

## 2026-04-11 — Add Fonts section to CLAUDE.md

**Prompt:** Add to CLAUDE.md under a Fonts section: 'All demo pages and the main website use Inter as the primary font. Never use system fonts as the primary font. Load Inter from Google Fonts. Body text: Inter 400. Bold text: Inter 700. Headings: Inter 600 or 700.'

**Built:** Appended a new "Fonts" section to `CLAUDE.md` after the Logo and Layout Standards section, with the Inter-as-primary-font rule, the prohibition on system fonts, the Google Fonts loading requirement, and the three weight usage rules as a bulleted list.

**Key decisions:**
- Placed the Fonts section after Logo and Layout Standards since both are visual/branding standards that apply cross-repo — they belong grouped together.
- Formatted the three weight rules as a bulleted list for scannability rather than inline text.

**Files modified:**
- `CLAUDE.md`

## 2026-04-11 — Match hero title styling to strategic planning demo

**Prompt:** Change the hero title font-weight from 700 to 600, and add Inter as the font family for the hero title. Match the hero styling from the strategic planning demo.

**Built:** Updated `.hero-title` and `.hero-subtitle` in `index.html` to match `../demo-strategic-planning/index.html` hero typography:
- `.hero-title`: added `font-family: 'Inter', -apple-system, BlinkMacSystemFont, sans-serif`, changed `font-weight` from 700 to 600 (user override — the strategic planning demo uses 700, but the explicit instruction was 600), changed `font-size` from 2.4rem to 32px, `letter-spacing` from -.02em to -0.5px, `margin-bottom` from 16px to 8px. Kept `margin-top: 20px` unchanged.
- `.hero-subtitle`: added Inter font family, changed `font-size` from 1.1rem to 15px, swapped `color: #aaa` for `color: #fff; opacity: 0.8`, `max-width` from 680px to 600px, removed the `28px` bottom margin, `line-height` from 1.6 to 1.7.

**Key decisions:**
- Followed the user's explicit font-weight instruction (600) rather than the literal strategic planning value (700), per the CLAUDE.md instruction priority (user instructions override).
- Also brought the subtitle into alignment with the strategic planning demo's `hero p` rule since the user asked to "match the hero styling" beyond just the title weight/family — matching the subtitle keeps the two demos visually consistent.
- Inter is already loaded via the Google Fonts link added earlier for the footer, so no additional font import was needed.

**Files modified:**
- `index.html`

## 2026-04-11 — Drop intro heading

**Prompt:** Drop the heading, the "solution isn't more people" paragraph, and the third paragraph listing each tool.

**Built:** Removed the `<h2>Building a Marketing Ecosystem with Claude Code</h2>` from the `.intro` block in `index.html`. The "solution isn't more people" and tool-listing paragraphs were already removed in the previous intro-tightening pass, so this change was heading-only.

**Files modified:**
- `index.html`

## 2026-04-11 — Tighten intro copy

**Prompt:** Change the intro to a two-paragraph version: one short paragraph summarizing Yowie + the project, and the existing italic GitHub repo paragraph.

**Built:** Replaced the three-paragraph intro body in `index.html` with two paragraphs:
1. A single-paragraph description of Yowie and the project (no more `<strong>` callouts, no "three layers" breakdown, no channels enumeration).
2. The existing italic GitHub repo paragraph, preserved verbatim with the `<a>` tag and `target="_blank"` / `rel="noopener"` attributes.

**Key decisions:**
- Kept the `<h2>Building a Marketing Ecosystem with Claude Code</h2>` heading untouched since the prompt only asked to change the intro body.
- Removed the inline `<strong>` emphasis that had been on "Yowie", "Brand Configuration Layer", "Research & Brief Builder", and "Content Engine" — the new copy is too short to justify bolded callouts.

**Files modified:**
- `index.html`

## 2026-04-11 — Renumber tools 1–4 starting with Architecture Map

**Prompt:** Number the Architecture Map as tool 1. Renumber Brand Config Viewer to 2, Brief Builder to 3, Content Engine to 4. Update the sticky nav to match. Change the Architecture Map's number badge from gold dot to the standard numbered circle style.

**Built:** In `index.html`:
- Renumbered the four `.tool-section` blocks: Architecture Map → `id="tool-1"` with `.tool-number` "1", Brand Config Viewer → `tool-2`/"2", Brief Builder → `tool-3`/"3", Content Engine → `tool-4`/"4".
- Removed the inline gold-dot override (`style="background: #e2c87d; color: #1a1a1a;"` with `&bull;`) from the Architecture Map badge so it uses the default dark numbered-circle style that matches the other three tools.
- Updated the `.tool-nav` links to `1. Architecture Map · 2. Brand Config Viewer · 3. Brief Builder · 4. Content Engine`, with hrefs pointing to the new tool-1 through tool-4 anchors.

**Files modified:**
- `index.html`

## 2026-04-11 — Copy Agency State logo assets into repo

**Prompt:** Copy the logo assets from the agency-state repo into this repo's assets folder: mark.svg, agency-state-wordmark.svg, and agency-state-wordmark-white.svg. Then check that the img src paths in the HTML match the file locations.

**Built:**
- Created `assets/` directory at repo root.
- Copied `mark.svg`, `agency-state-wordmark.svg`, and `agency-state-wordmark-white.svg` from `/Users/gregappler/Claude Code Projects/agency-state/assets/` into `/Users/gregappler/Claude Code Projects/demo-marketing-ecosystem/assets/`.
- Verified the three `<img src="assets/...">` references in `index.html` (hero wordmark at line 246, footer mark at 369, footer wordmark at 371) all resolve to the newly copied files.

**Key decisions:**
- Copied the files as-is rather than symlinking, so this repo is self-contained and doesn't depend on the sibling agency-state repo's path existing on other machines.

**Files modified:**
- `assets/mark.svg` (new)
- `assets/agency-state-wordmark.svg` (new)
- `assets/agency-state-wordmark-white.svg` (new)

## 2026-04-11 — Tune iframe heights per tool

**Prompt:** Check each tool iframe. The architecture map, brand config viewer, brief builder, and content engine may need different heights. Adjust each iframe height so the key content is visible without scrolling inside the preview.

**Built:** Added per-iframe inline `height` overrides in `index.html`, replacing the shared 600px default from `.tool-preview iframe`:
- `architecture_map.html`: 900px (full foundation node + pipeline row + legend/footer)
- `brand_config_viewer.html`: 850px (selector bar + initial two rule cards)
- `brief_builder.html`: 750px (pipeline diagram + sidebar/main with first competitive section)
- `content_engine.html`: 800px (pipeline + sidebar + brief summary + first content card)

**Key decisions:**
- Dispatched an Explore agent to read the four tool files and estimate the visual fold for each, rather than guessing from line counts.
- Used inline `style="height: Xpx"` overrides on each `<iframe>` instead of adding per-tool classes — the CSS rule already sets `height: 600px`, and inline styles are a one-line override per iframe, cheaper than adding four new class definitions.
- Left the 600px base rule in `.tool-preview iframe` untouched so that any future iframe added without an override still has a sensible default.

**Files modified:**
- `index.html`

## 2026-04-11 — Update page title tag

**Prompt:** Change the title tag to 'Demo: Yowie Marketing Ecosystem — Agency State'

**Built:** Updated the `<title>` in `index.html` from "Yowie Marketing Ecosystem — Guided Tour" to "Demo: Yowie Marketing Ecosystem — Agency State".

**Files modified:**
- `index.html`

## 2026-04-11 — Replace footer with Agency State footer

**Prompt:** Replace the footer with the Agency State footer — monogram + clickable wordmark SVG on the left, 'Built with Claude Code' in Inter on the right. Copy the footer implementation from the strategic planning demo. Remove Yowie branding from the footer.

**Built:** Replaced the dark Yowie footer in `index.html` with the Agency State footer pattern from `../demo-strategic-planning/index.html`:
- New `<footer>` markup: `.footer-left` contains `assets/mark.svg` (56×56) + a clickable `.footer-wordmark-link` wrapping `assets/agency-state-wordmark.svg` (linked to `https://agencystate.ai`, opens in new tab). `.footer-right` is a `<p>` with "Built with Claude Code".
- New footer CSS: white background, `#E0DFDC` top border, 3rem vertical padding, max-width 1200px container with flex space-between. `.footer-right` font-size 0.8125rem, color `#5C5C5C`. Mobile breakpoint stacks the two sides vertically.
- Footer font family set to Inter regardless of the rest of the demo's fonts, per the CLAUDE.md logo-and-layout standard.
- Added Google Fonts `<link>` for Inter (weights 400/600/700) to the `<head>`, since Inter was not previously loaded — without this the `font-family: 'Inter'` declaration would silently fall through to system sans.
- Removed all Yowie branding copy ("Yowie is a simulated brand…", tool count line, etc.).

**Key decisions:**
- Matched the strategic planning demo's footer implementation exactly (selectors, sizes, spacing, mobile breakpoint) so both demos render identically.
- Added the Google Fonts link rather than inlining an `@import` or @font-face block — matches how the strategic planning demo loads Inter.
- Used the `<footer>` element (matching the source) rather than the previous `<div class="footer">`, and dropped the unused `.footer` class.

**Files modified:**
- `index.html`

## 2026-04-11 — Update CTA copy

**Prompt:** Change the CTA title to 'Build tools like these for your team.' Change the description to 'Agency State teaches professionals how to capture collective knowledge and put AI to work with it.'

**Built:** Updated the `.cta` block in `index.html`:
- Title changed from "Learn how to build for your team." to "Build tools like these for your team."
- Description changed to "Agency State teaches professionals how to capture collective knowledge and put AI to work with it."

**Files modified:**
- `index.html`

## 2026-04-11 — Remove 'How This Was Built' section

**Prompt:** Remove the entire 'How This Was Built' section including the 'What This Means for Your Team' subsection. The CTA should follow directly after the last tool.

**Built:** Removed from `index.html`:
- The `<div class="how-built" id="how-built">` block and its preceding `.divider` (including both `<h2>How This Was Built</h2>` and `<h3>What This Means for Your Team</h3>` subsections).
- The `<a href="#how-built">How It Was Built</a>` link in `.tool-nav` (now ended cleanly after the Content Engine link).
- The orphaned `.how-built` CSS rules, since nothing references them anymore.

**Key decisions:**
- Also removed the now-dead nav link and CSS rather than leaving orphaned references — the user asked to remove the section, and leaving the nav link pointing to a missing anchor would be a user-visible break.

**Files modified:**
- `index.html`

## 2026-04-11 — Add italic repo reference paragraph after intro

**Prompt:** Add an italic paragraph after the intro section: 'Each tool serves a unique purpose but they all share the same data and inform each other. See the GitHub repo for brand, data, and related context.' Link 'GitHub repo' to https://github.com/gappler/demo-marketing-ecosystem.

**Built:** Added a new italic `<p><em>…</em></p>` at the end of the `.intro` block in `index.html` with the specified copy and a link on "GitHub repo" to the repo URL (`target="_blank"`, `rel="noopener"`).

**Key decisions:**
- Placed the paragraph inside the existing `.intro` div rather than after it so it inherits the intro's typography and width.
- Used `<em>` for the italicization rather than adding a new class, keeping the change purely content-level.

**Files modified:**
- `index.html`

## 2026-04-11 — Restyle tool prompts as italic inline text

**Prompt:** Change the prompt styling. Remove the dark background from .tool-prompt. Display prompts as italic text with a 'Prompt:' label in bold, secondary text color. Remove the ::before pseudo-element that creates the floating PROMPT label. Match the strategic planning demo's prompt styling.

**Built:** Restyled `.tool-prompt` in `index.html`:
- Removed dark background, padding, border-radius, and position: relative.
- Removed the `::before` floating "PROMPT" label pseudo-element.
- Set color `#555`, italic, 0.95rem, line-height 1.7, max-width 800px.
- Added `.tool-prompt-label` class for a bold, non-italic "Prompt:" inline label in the same secondary color.
- Updated `.tool-prompt em` to preserve italic (nested em tags in the prompt text — already-italic copy inside an italic paragraph remains italic, consistent with the rest of the type).
- Added `<span class="tool-prompt-label">Prompt:</span>` to each of the three tool prompt blocks (brand config viewer, brief builder, content engine).

**Key decisions:**
- Kept `.tool-prompt em` italic rather than inverting, since in the new flat style there's no contrast purpose served by un-italicizing nested emphasis.
- Used an inline span for the label rather than a block element so the label sits inline with the prompt text.

**Files modified:**
- `index.html`

## 2026-04-11 — Update hero with Agency State wordmark and new content

**Prompt:** Update the hero. Add the Agency State white wordmark SVG in the top left, position absolute, linked to agencystate.ai. Change the hero content to new title/subtitle. Remove the 'Yowie' brand label. Remove the stats strip. Add 20px margin-top to the title for spacing below the wordmark.

**Built:** Restyled the `.hero` in `index.html`:
- Added `.hero-wordmark` absolute-positioned link (top 32px / left 40px) wrapping `assets/agency-state-wordmark-white.svg`, linked to `https://agencystate.ai`.
- Replaced the hero-brand + title with a single title: "Demo: Yowie Marketing Ecosystem".
- Replaced the subtitle with the new DTC outdoor gear copy.
- Removed the `.hero-brand`, `.hero-stats`, and all `.hero-stat-*` CSS and the corresponding HTML block.
- Added `margin-top: 20px` to `.hero-title` for spacing below the wordmark.
- Set `.hero` to `position: relative` to anchor the absolute wordmark.

**Key decisions:**
- Used `position: absolute` on the wordmark (not fixed) per the CLAUDE.md header standard — scrolls with the hero.
- Kept the wordmark height at 24px — small enough to sit above the centered title without crowding, readable against the dark background.
- Added `rel="noopener"` to the external link for security.
- Noted to user that `assets/` directory does not yet exist; SVG files need to be added.

**Files modified:**
- `index.html`

## 2026-04-11 — Add logo and layout standards to CLAUDE.md

**Prompt:** Add a section to CLAUDE.md covering logo and layout standards for use across all demo repos (logo assets, header, footer specs).

**Built:** New "Logo and Layout Standards" section appended to `CLAUDE.md` documenting the three SVG assets, header placement/link/positioning rules, and footer layout with typography.

**Key decisions:**
- Kept the section structured with three subsections (Assets, Header, Footer) to match the other rule sections in the file.
- Wrote `https://agencystate.ai` as explicit URLs rather than bare domain for clarity in the spec.
- Preserved the user's exact requirements verbatim (absolute vs fixed, Inter font for footer, color hex, font size).

**Files modified:**
- `CLAUDE.md`

# Process Log

## 2026-04-09 — Architecture Map & Guided Tour

**Prompt:** Build the index.html guided tour page and architecture map for the ecosystem.

**What was built:** Architecture map (`tools/architecture_map.html`) showing the full pipeline from Brand Configuration through Brief Builder, human approval, Content Engine, human approval, to Publish & Distribute. Guided tour page (`index.html`) with dark hero header, sticky navigation, intro section, architecture map embed, and three tool showcases with browser chrome previews, prompts, and problem statements. Includes "How this was built" methodology section and CTA to agencystate.ai.

**Key decisions:**
- Architecture map shows foundation layer (Brand Config) feeding the 5-stage pipeline
- AI nodes in teal, human decision gates in gold with review checklists, delivery destinations as output
- Guided tour follows Sidebar Creative demo pattern: dark hero, light body, browser chrome previews
- Architecture map shown first (before numbered tools) to establish the full pipeline context
- Each tool section includes the problem it solves, the prompt that built it, and an iframe preview

**Files created or modified:**
- `tools/architecture_map.html` (created)
- `index.html` (created)
- `docs/process_log.md` (modified)

## 2026-04-09 — Research & Brief Builder

**Prompt:** Build the Research & Brief Builder for Yowie. This component is the upstream intelligence feed — it takes raw inputs and produces structured campaign briefs that feed directly into the Content Engine. Demonstrate it with competitive research, audience analysis, and reasoning chains. Make it interactive with a pipeline diagram showing human decision points.

**What was built:** An interactive single-file HTML tool (`tools/brief_builder.html`) that demonstrates Yowie's research-to-brief pipeline. Contains 3 pre-generated research briefs: Slab 35QT cooler acquisition (vs. YETI, Stanley, RTIC, Igloo), Lowline 2P cold-weather launch (vs. MSR, Hilleberg, Big Agnes, REI), and Foldout soft cooler cross-sell to existing Slab buyers. Each brief includes competitive landscape analysis, audience research, reasoning chains explaining every brief decision, and a structured output brief compatible with the Content Engine.

**Key decisions:**
- Pipeline diagram at top with 5 stages — AI stages in teal, human review gate in gold
- Clickable pipeline stages scroll to corresponding sections with highlight animation
- 3 briefs chosen to demonstrate different campaign objectives: acquisition, launch, cross-sell
- Competitive research covers 4 real competitors per brief with pricing, positioning, strengths, weaknesses
- Reasoning section makes the research-to-brief logic chain explicit and visible
- Output brief matches Content Engine schema exactly — same field structure
- Human review section with checklist makes the decision gate concrete
- "AI produces drafts. Humans make decisions." tagline reinforces the workflow

**Files created or modified:**
- `tools/brief_builder.html` (created)
- `docs/superpowers/specs/2026-04-09-brief-builder-design.md` (created)
- `docs/superpowers/plans/2026-04-09-brief-builder.md` (created)
- `docs/process_log.md` (modified)
- `tutorials/04_brief_builder.md` (created)

---

## 2026-04-09 — Content Engine

**Prompt:** Build the Content Engine for Yowie. It reads the Brand Configuration Layer and takes a structured campaign brief as input. The brief includes: product line, audience segment, channel, content type, campaign stage, primary message, and optional fields. The engine produces on-brand, segment-specific, channel-ready content. Demonstrate four output modes: single item, segment variants, channel variants, and a full campaign package. Output as an interactive HTML tool.

**What was built:** An interactive single-file HTML tool (`tools/content_engine.html`) that demonstrates Yowie's content generation pipeline. Contains 4 pre-generated campaign briefs covering all product lines, with AI-written copy for every audience × channel combination. Four output modes show increasing complexity: single item, segment variants (same message adapted for different audiences), channel variants (same audience across email/social/product page), and full campaign package with metadata.

**Key decisions:**
- Hybrid approach: pre-generated AI copy at build time (not live generation), embedded as JSON in the HTML file
- 4 briefs chosen to map to launch phases: product launch, cross-segment, acquisition, seasonal
- Each content card includes rules applied, pillar used, and rationale — the tool teaches the system logic
- "What changed" annotations between variants explain adaptation decisions
- Matched existing brand_config_viewer.html dark theme for visual consistency
- Sidebar + content panel layout matching the established interaction pattern

**Files created or modified:**
- `tools/content_engine.html` (created)
- `docs/superpowers/specs/2026-04-09-content-engine-design.md` (created)
- `docs/superpowers/plans/2026-04-09-content-engine.md` (created)
- `docs/process_log.md` (modified)
- `tutorials/03_content_engine.md` (created)

---

## 2026-04-09 — Brand Configuration Layer

**Prompt:** Build the Brand Configuration Layer for Yowie. This is not a document — it's the rules engine that governs every piece of content the ecosystem produces. Read the brand definition and create a structured configuration that encodes: voice attributes and channel-specific registers, audience-specific voice adaptations (backcountry vs. casual), messaging hierarchy by product line and audience segment, approved and forbidden terminology, style standards, and compliance rules. Output as an interactive HTML tool where I can browse the configuration, see how rules apply to different product/audience/channel combinations, and preview how the voice shifts by context.

**What was built:** Two files — a structured JSON rules engine (`brand/brand_config.json`) encoding all brand voice, terminology, messaging, and compliance rules, and an interactive HTML viewer (`tools/brand_config_viewer.html`) with product/audience/channel selectors that show combined rulesets and copy previews.

**Key decisions:**

- JSON config as machine-readable source of truth, separate from the HTML viewer, so future tools can import the rules programmatically
- Selector panel UI (product x audience x channel) over matrix view — matches the real workflow of "I'm writing for X, show me the rules"
- Static hand-crafted copy examples (22 total) over generated templates — the voice is too nuanced for templates to capture
- Single self-contained HTML file with embedded config — no server, no dependencies, any team member can open it
- Messaging pillar ranking computed by averaging product-line and audience emphasis indices, with audience emphasis as tiebreaker
- URL hash state for bookmarking specific combinations
- Terminology split into three tiers: approved (with usage notes), forbidden (with reasons), conditional (with correct/incorrect examples)

**Files created or modified:**

- `brand/brand_config.json` (created)
- `tools/brand_config_viewer.html` (created)
- `docs/process_log.md` (modified)
- `tutorials/02_brand_config_layer.md` (created)

---

## 2026-04-09 — Expanded Brand Definition

**Prompt:** Extend the Yowie brand definition from a single-product company to a four-product-line outdoor gear brand. (Full prompt included product specs, pricing, audience details, voice guidelines, and marketing objectives for backpacks, tents, sleeping bags, and coolers.)

**What was built:** Comprehensive brand definition document covering Yowie's expansion from a single backpack product to four product lines with eight total products.

**Key decisions:**

- Structured the document in six sections: brand overview, product catalog, audience profiles, messaging pillars, brand voice guidelines, and marketing objective
- Organized audience profiles by buyer type (backcountry hikers, thru-hikers, casual outdoor, cold-weather specialists) rather than by product line, since audiences overlap across products
- Messaging pillars written to work across all four lines with notes on how emphasis shifts by segment
- Voice guidelines include a comparison table showing how the same voice adapts for backcountry vs. casual buyers without changing core attributes
- Marketing objective structured as a three-phase launch strategy acknowledging the 6-person team constraint

**Files created or modified:**

- `brand/brand_definition.md` (created)
- `docs/process_log.md` (created)
- `tutorials/01_brand_definition.md` (created)
