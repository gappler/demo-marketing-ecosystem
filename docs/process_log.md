---
title: Process Log
version: 1.0
---

# Process Log

## 2026-04-09 — Build Brand Config JSON: meta, voice, terminology

**Prompt:** Create `brand/brand_config.json` with `meta`, `voice`, and `terminology` sections. The JSON must contain: meta (version, date, source), voice.core_attributes (four attributes: plainspoken, confident, restrained, dry — each with description and rules array), voice.channels (four channels: product_page, email, social_media, customer_service — each with register, rules, and example), voice.audience_adaptations (two modes: backcountry and casual — each with applies_to, approach, assumed_knowledge, and guidelines), terminology.approved (array with term and note), terminology.forbidden (array with term and reason), terminology.conditional (array with term, rule, correct, and incorrect).

**What was built:** `brand/brand_config.json` — a machine-readable JSON file capturing Yowie's brand voice, channel-specific communication rules, audience adaptation guidance, and approved/forbidden/conditional terminology, all derived from `brand/brand_definition.md`.

**Key decisions:**

- Derived all voice attribute descriptions and rules directly from the Brand Voice Guidelines section (section 5) of `brand_definition.md`, preserving the original language where possible
- Used exact example strings from the voice-by-channel table in the source document
- Mapped backcountry audience adaptation to three audience slugs (`backcountry_hikers`, `thru_hikers`, `cold_weather`) to reflect section 3 audience profiles
- Expanded terminology entries with `note` and `reason` fields that add context from the broader brand definition, not just the word lists
- Added conditional terminology rules with concrete correct/incorrect examples drawn from product copy in section 2

**Files created or modified:**

- `brand/brand_config.json` (created)
- `docs/process_log.md` (modified)
- `tutorials/02_brand_config_json.md` (created)

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
