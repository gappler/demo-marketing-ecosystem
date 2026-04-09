---
title: Process Log
version: 1.0
---

# Process Log

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
