---
title: "Tutorial: Building a Brand Configuration Layer"
version: 1.0
date: 2026-04-09
tool: Brand Config Viewer
output: tools/brand_config_viewer.html
---

# Tutorial 02: Building a Brand Configuration Layer

## What This Covers

How to encode a brand definition document into a structured rules engine (JSON) and build an interactive HTML viewer for browsing rules by product/audience/channel combination.

## Prerequisites

A completed brand definition document (`brand/brand_definition.md`) with defined voice attributes, product catalog, audience profiles, messaging pillars, and terminology guidelines.

## The Prompt

The prompt asked for a "rules engine" — not a document, but a structured configuration that governs content production. Key requirements:

1. Voice attributes with channel-specific registers
2. Audience-specific voice adaptations
3. Messaging hierarchy by product line and audience
4. Approved/forbidden/conditional terminology
5. Style and compliance rules
6. Interactive HTML tool for browsing rule combinations

## Step-by-Step Process

### Step 1: Structure the JSON config

Build `brand/brand_config.json` with seven top-level keys, added iteratively:

1. **meta** — version, date, source reference
2. **voice** — core attributes (plainspoken, confident, restrained, dry), channel registers (product page, email, social, customer service), and audience adaptations (backcountry vs. casual)
3. **terminology** — three tiers: approved (with usage notes), forbidden (with reasons), conditional (with correct/incorrect examples)
4. **messaging** — four pillars with audience-specific resonance notes, plus emphasis rankings per product line and audience
5. **products** — all 8 products with specs, pricing, and audience mappings
6. **audiences** — all 4 segments with voice adaptation mode and profile
7. **examples** — 22 hand-crafted copy examples keyed by product|audience|channel

Build each section from the brand definition document. Validate JSON after each addition.

### Step 2: Write copy examples

Pick the most important product/audience/channel combinations:

- All 8 products on their primary audience's product page (8 examples)
- Cooler products across all 4 channels for casual audience (4 examples, 2 overlap)
- Key backcountry products on email and social media (6 examples)
- Cross-audience examples showing the same product for different audiences (4 examples)

Write each example in the brand voice. Annotate which rules it demonstrates. This is the hardest step — templates can't capture the dry humor and confident tone.

### Step 3: Build the HTML viewer

Create a single self-contained HTML file (`tools/brand_config_viewer.html`):

1. Embed the JSON config in a `<script>` tag
2. Build a selector bar with toggle buttons for all products, audiences, and channels
3. Render four rules cards in a 2x2 grid: Voice, Messaging, Terminology, Style & Compliance
4. Add a copy preview section that shows examples when available
5. Implement URL hash state for bookmarking

The viewer uses vanilla HTML/CSS/JavaScript — no dependencies, no build step. Any team member can open the file in a browser.

### Step 4: Implement pillar ranking

The messaging card computes pillar priority by averaging each pillar's index in the product-line emphasis array and the audience emphasis array. Lower average = more primary. Ties break in favor of the audience emphasis ordering (the human context is the tiebreaker).

## Key Decisions and Rationale

### JSON as the config format

The brand rules need to be both human-browsable and machine-readable. JSON serves both: it's structured enough for programmatic consumption by future tools, and the HTML viewer renders it for human use. YAML was considered but adds a build step for browser consumption.

### Selector panel interaction model

Three selector groups (product, audience, channel) with toggle buttons. This mirrors the real workflow: "I'm writing about the Slab for casual buyers on social media — what rules apply?" A matrix view was considered but optimizes for pattern-spotting over practical use.

### Hand-crafted copy examples

22 pre-written examples covering key combinations. Templates were rejected because Yowie's voice — especially the dry humor and confident tone — can't be captured by slot-filling. The examples serve as reference copy, not generated output.

### Single-file HTML

The viewer embeds the JSON config directly. No server, no build step, no dependencies. Any team member can open the file in a browser. Trade-off: updating the config requires copying the new JSON into the HTML file. Acceptable for a 6-person team.

### Three-tier terminology

Splitting terminology into approved/forbidden/conditional (instead of just approved/forbidden) captures nuance. Words like "sustainable" aren't banned — they have specific usage rules. The conditional tier with correct/incorrect examples prevents misapplication.

## How to Test

1. Open `tools/brand_config_viewer.html` in a browser
2. Default state: Basecamp 45L / Backcountry Hikers / Product Page — verify technical voice register and spec-forward copy
3. Switch to Slab 35QT / Casual / Social Media — verify casual adaptation, "pay for the gear" pillar prominence, and dry humor copy
4. Switch to Basecamp 4P / Backcountry Hikers / Product Page, then switch audience to Casual — verify the same product's voice shifts from spec-forward to outcome-focused
5. Try a combination without an example (e.g., Ridgeline 22L / Cold-Weather / Customer Service) — verify the fallback shows applicable rules

## Output

- Config: `brand/brand_config.json`
- Viewer: `tools/brand_config_viewer.html`
