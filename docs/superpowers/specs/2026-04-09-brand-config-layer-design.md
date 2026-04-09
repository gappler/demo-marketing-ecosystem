---
title: Brand Configuration Layer — Design Spec
version: 1.0
date: 2026-04-09
status: draft
---

# Brand Configuration Layer

## Overview

A structured rules engine that encodes Yowie's brand voice, terminology, messaging hierarchy, and compliance rules. Two deliverables:

1. **`brand/brand_config.json`** — Machine-readable config file encoding all brand rules. Source of truth for other ecosystem tools.
2. **`tools/brand_config_viewer.html`** — Self-contained HTML viewer that embeds the JSON config and lets the team browse rules by product/audience/channel combination.

## Architecture

### Config File: `brand/brand_config.json`

Six top-level keys:

#### `meta`
Version, date, and source reference to `brand/brand_definition.md`.

#### `voice`
Three sub-sections:

- **`core_attributes`** — Four attributes (plainspoken, confident, restrained, dry), each with a description and an array of concrete rules.
- **`channels`** — Four channels (product_page, email, social_media, customer_service), each with a register label and channel-specific rules.
- **`audience_adaptations`** — Two adaptation modes (backcountry, casual). Each lists assumed knowledge, the communication approach, and specific guidelines. The backcountry adaptation applies to backcountry hikers, thru-hikers, and cold-weather specialists. The casual adaptation applies to casual/car camping.

#### `terminology`
Three categories:

- **`approved`** — Array of words/phrases Yowie uses, each with a brief usage note.
- **`forbidden`** — Array of words/phrases Yowie never uses, each with a reason.
- **`conditional`** — Array of words with specific usage rules (e.g., "sustainable" — state as fact, never as a leading claim).

#### `messaging`
- **`pillars`** — Four messaging pillars, each with an id, a headline, a description, and segment-specific resonance notes (what backcountry buyers hear vs. what casual buyers hear).
- **`product_emphasis`** — For each product line (backpacks, tents, sleeping_bags, coolers), an ordered array of pillar IDs from primary to secondary.
- **`audience_emphasis`** — For each audience segment, an ordered array of pillar IDs.

#### `products`
All 8 products keyed by slug (e.g., `basecamp_45l`). Each includes: name, line, price, description, key features array, and the product's relevant audience segments.

#### `audiences`
All 4 audience segments keyed by slug. Each includes: name, relevant product lines, the voice adaptation mode (backcountry or casual), and a brief profile summary.

#### `examples`
Pre-written copy examples keyed by `product_slug|audience_slug|channel` triplets. Each entry has:

- `copy` — The example text.
- `rules_demonstrated` — Array of rule IDs/descriptions this example illustrates.

Approximately 20-25 examples covering the most important combinations:

- All 8 products on product_page for their primary audience (8 examples)
- Cooler products across all 4 channels for casual audience (4 examples, 2 overlap with above)
- Key backcountry products across email and social_media (6 examples)
- A few cross-audience examples showing the same product voiced for different audiences (4 examples)

Not every combination gets an example. The viewer handles missing examples gracefully.

### HTML Viewer: `tools/brand_config_viewer.html`

A single self-contained HTML file. No external dependencies, no build step, no server required. The JSON config is embedded in a `<script>` tag. Open the file in any browser.

#### Layout

Three vertical zones:

**1. Selector Bar (top)**

- Three rows of toggle buttons: Product (8 buttons), Audience (4 buttons), Channel (4 buttons).
- Product row is full width. Audience and Channel rows sit side by side below.
- Active selection highlighted with gold accent. One selection per group.
- Default on load: Basecamp 45L / Backcountry Hikers / Product Page.
- Clicking any button immediately updates the rules panel and copy preview.

**2. Rules Panel (middle)**

A 2x2 card grid. Each card has a gold label header and content that updates based on the current selection:

- **Voice** — Shows the four core attributes (always present), the channel-specific register, and the audience adaptation rules for the selected audience. The adaptation rules are the dynamic part — they change between backcountry and casual modes.
- **Messaging** — Shows all four pillars. Primary pillars for this product/audience combo are visually prominent (full opacity, filled dot). Secondary pillars are dimmed (reduced opacity, hollow dot). Ordering is determined by intersecting the product-line emphasis with the audience emphasis.
- **Terminology** — Approved terms (green check), forbidden terms (red x, strikethrough), conditional terms (amber warning with usage rule). This list is global — it doesn't change by selection. It's always visible as a reference.
- **Style & Compliance** — Formatting rules, branding rules, sustainability claim rules. Global rules that don't change by selection but serve as a persistent checklist.

**3. Copy Preview (bottom)**

- If an example exists for the current product/audience/channel triplet: shows the example copy in an indented blockquote with a gold left border, followed by annotations listing the rules it demonstrates.
- If no example exists: shows a muted placeholder ("No example for this combination") with a summary of which rules apply based on the current selection.

#### Visual Design

- Dark theme consistent with the mockup (dark navy background, muted text, gold accents for active states and labels, teal for positive indicators, red for forbidden items).
- Clean sans-serif typography (system fonts — no external font loading).
- Responsive enough to work on laptop screens. No mobile optimization needed — this is an internal team tool.

#### Interaction

- Toggle buttons are the only interactive elements. Click to select, panel updates instantly via JavaScript DOM manipulation.
- No animations, no transitions, no loading states. Instant updates from embedded data.
- URL hash state: the current selection is encoded in the URL hash (`#product=slab_35qt&audience=casual&channel=social_media`) so bookmarking and sharing specific combinations works.

## Files Created

| File | Purpose |
|------|---------|
| `brand/brand_config.json` | Machine-readable brand rules config |
| `tools/brand_config_viewer.html` | Interactive HTML viewer (embeds config) |

## Scope Boundaries

**In scope:**
- Encoding all rules from `brand/brand_definition.md` into structured JSON
- Building the interactive viewer with selector panel, rules display, and copy preview
- Writing ~20-25 hand-crafted copy examples for key combinations
- URL hash state for bookmarking

**Out of scope:**
- Config editing UI (edit the JSON directly)
- Sync mechanism between JSON file and HTML embed (manual copy for now)
- Mobile-responsive design
- Copy generation or AI-assisted writing
- Integration with other ecosystem tools (future work)
