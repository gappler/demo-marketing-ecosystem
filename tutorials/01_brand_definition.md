---
title: "Tutorial: Building a Multi-Product Brand Definition"
version: 1.0
date: 2026-04-09
tool: Brand Definition Document
output: brand/brand_definition.md
---

# Tutorial 01: Building a Multi-Product Brand Definition

## What This Covers

How to use Claude Code to expand a single-product brand identity into a comprehensive multi-product-line brand definition document.

## The Prompt

The prompt that built this document provided:

1. **Existing brand identity** — Yowie's core values (unpretentious, quality-obsessed, minimal branding, built for experienced outdoor adults)
2. **Design philosophy** — "Overbuild where it matters, strip weight where it doesn't"
3. **Four product lines with full specs** — backpacks, tents, sleeping bags, coolers — including model names, prices, and target audiences
4. **Voice guidelines** — plainspoken, confident, restrained, dry
5. **Company context** — $2.8M revenue, 6-person team, 4,200 email subscribers, 12,000 social followers
6. **Requested sections** — brand overview, product catalog, audience profiles, messaging pillars, voice guidelines, marketing objective

The key to a good brand definition prompt: be specific about products, prices, and audience. Be explicit about what the voice sounds like. Provide business context (team size, revenue, channels) so the marketing strategy section is grounded in reality.

## Key Decisions and Rationale

### Document structure

Six sections in order of abstraction: identity first, then products, then audience, then messaging, then voice, then strategy. This mirrors how a reader needs to build understanding — you can't evaluate messaging pillars without knowing the products, and you can't evaluate strategy without knowing the audience.

### Audience profiles organized by buyer type, not product line

Buyers for tents overlap with buyers for sleeping bags and backpacks. Organizing by product line would create redundancy. Organizing by buyer type (backcountry hikers, thru-hikers, casual outdoor, cold-weather specialists) captures the overlaps and distinctions more clearly.

### Voice adaptation section

The same brand voice applies to all products, but the backcountry audience and the casual/cooler audience have different levels of technical literacy. The document shows how to adapt assumed knowledge without changing tone — backcountry copy uses specs directly, casual copy translates specs into outcomes.

### Three-phase marketing strategy

Phase 1 targets existing audience (they already trust the brand). Phase 2 acquires new customers through the cooler line (lowest barrier to entry, broadest appeal). Phase 3 cross-pollinates cooler buyers into the broader catalog. This sequence respects a 6-person team's bandwidth.

## How to Replicate

1. **Start with identity.** Write down the 3-5 core brand values. Be specific — "quality-obsessed" is better than "premium."

2. **Define every product.** Include: name, price, one-sentence description, 6-10 key features with real specs (weights, dimensions, materials). If you don't have specs, estimate them — you can refine later.

3. **Describe each audience segment.** Demographics, psychographics, buying behavior. Note where segments overlap. Different products can share audiences.

4. **Write messaging pillars.** 3-5 core messages that work across all product lines. Test each one: does it apply to the backpack AND the cooler? If not, revise.

5. **Document the voice.** List words the brand uses and words it doesn't. Show examples of how copy sounds on different channels. If the brand serves different audiences, show how the voice adapts without changing character.

6. **Ground the strategy in constraints.** Team size, budget, existing channels, and realistic growth targets. A 6-person team can't do what a 60-person team can. Strategy should reflect that.

## Output

Final file: `brand/brand_definition.md`
