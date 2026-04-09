---
title: Building the Research & Brief Builder
version: 1.0
date: 2026-04-09
tool: Research & Brief Builder
output: tools/brief_builder.html
---

# Tutorial 04: Building the Research & Brief Builder

## What This Tool Does

The Research & Brief Builder is the upstream intelligence feed for Yowie's content pipeline. It takes raw inputs — product data, competitive landscape, audience research, seasonal context — and produces structured campaign briefs that feed directly into the Content Engine. The tool makes the human decision point in the workflow explicit.

## Step 1: Define the Pipeline

The first design decision was making the workflow visible. A 5-stage horizontal pipeline diagram sits at the top of the tool:

1. **Raw Inputs** — product data, market research, seasonal context
2. **AI Research & Analysis** — competitive landscape, audience analysis, positioning
3. **Draft Brief** — structured campaign brief
4. **Human Review** — the decision gate (visually distinct in gold)
5. **Content Engine** — approved brief feeds content generation

The human review stage is styled differently from AI stages — gold vs. teal — to make the decision point impossible to miss.

### Prompt Used

"Build the Research & Brief Builder for Yowie. This component is the upstream intelligence feed — it takes raw inputs (product data, audience research, competitive landscape, seasonal context) and produces structured campaign briefs that feed directly into the Content Engine."

## Step 2: Choose the Research Briefs

Three briefs were chosen to demonstrate different campaign objectives:

1. **Slab 35QT Acquisition** — entering the cooler market against YETI, Stanley, RTIC, Igloo
2. **Lowline 2P Cold-Weather Launch** — launching a tent to the most spec-literate audience
3. **Foldout Cross-Sell** — cross-pollinating existing Slab buyers into a complementary product

Each brief covers a different phase of Yowie's marketing strategy (Phase 2 acquisition, Phase 1 launch, Phase 3 cross-pollination).

## Step 3: Research the Competitive Landscape

For each brief, 4 real competitors were analyzed:

- **Name and price range** — positions each competitor in the market
- **Positioning** — how they present themselves (lifestyle, value, performance, budget)
- **Strengths** — what they do well that Yowie must acknowledge
- **Weaknesses** — gaps Yowie can exploit

### Key Research Insight: The Cooler Market

The Slab brief reveals a gap between YETI (quality + lifestyle tax), RTIC (value + knockoff stigma), and Igloo (cheap + disposable). Yowie slots into the quality-without-lifestyle-tax position that no major competitor owns.

## Step 4: Analyze the Audience

Each brief includes a detailed audience analysis:

- **Profile** — who they are, what they care about
- **Pain points** — specific frustrations that drive purchase decisions
- **Buying triggers** — what actually makes them buy
- **Media habits** — where they research and discover products

The audience analysis directly informs channel selection in the output brief.

## Step 5: Build the Reasoning Chain

The reasoning section makes the research-to-brief logic chain visible:

- **Message rationale** — "Research showed X tension → so the primary message addresses it this way"
- **Channel rationale** — "Audience media habits are X → so we use these channels in this order"
- **Proof point rationale** — "Audience pain points are X → so we selected these verifiable claims"
- **Pillar rationale** — "Competitive landscape shows X → so this messaging pillar leads"

This is the part most marketing tools hide. Making it visible builds trust in the AI-generated brief.

## Step 6: Output the Brief

The output brief matches the Content Engine's input schema exactly:

- product_id, product_line, audience_segments, channels
- campaign_stage, primary_message, secondary_messages
- pricing, proof_points, tone_override, cta
- personalization_variables (used in the cross-sell brief)

A "Content Engine Compatible" badge indicates the brief can feed directly into the next tool in the pipeline.

## Step 7: The Human Review Gate

The human review section makes the decision point concrete with a checklist:

- Does the primary message match the positioning angle?
- Are the proof points verifiable and specific?
- Is the channel mix right for this audience's media habits?
- Does the CTA match the campaign objective?
- Any tone overrides needed?

This reinforces the tool's core principle: AI produces drafts, humans make decisions.

## Output

Final tool: `tools/brief_builder.html`

Open in any browser — no server required. Select a brief, explore the research, expand the reasoning chains, review the output brief, and see the human decision checklist.
