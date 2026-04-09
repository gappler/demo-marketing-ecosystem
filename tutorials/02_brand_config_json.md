---
title: "Tutorial 02: Build the Brand Config JSON"
version: 1.0
date: 2026-04-09
task: Task 1 — Build JSON config: meta, voice, terminology
---

# Tutorial 02: Build the Brand Config JSON

This tutorial documents how to create a machine-readable brand configuration file from a prose brand definition document. It covers deriving structured data from narrative source material, organizing voice rules into a hierarchy, and establishing a terminology system with approved, forbidden, and conditional word lists.

---

## What This Builds

A single JSON file (`brand/brand_config.json`) that captures:

- Project metadata (version, date, source document)
- Core voice attributes with descriptions and rules
- Channel-specific registers with examples
- Audience adaptation guidance keyed by audience type
- A three-tier terminology system (approved, forbidden, conditional)

This file becomes the machine-readable source of truth for brand rules, intended to be read by downstream tools (copy validators, HTML viewers, AI prompts).

---

## Prompts Used

### Initiation prompt (paraphrased)

> Create `brand/brand_config.json` with `meta`, `voice`, and `terminology` sections. Read `brand/brand_definition.md` sections 1 and 5 for all voice attributes, channel registers, audience adaptations, and terminology lists. The JSON must contain specific keys and structure as defined in the task spec.

### Exact structural requirements provided

The task specified the following top-level structure:

```json
{
  "meta": { ... },
  "voice": {
    "core_attributes": { ... },
    "channels": { ... },
    "audience_adaptations": { ... }
  },
  "terminology": {
    "approved": [ ... ],
    "forbidden": [ ... ],
    "conditional": [ ... ]
  }
}
```

Each section had a defined schema (keys, value types, content sources).

---

## Step-by-Step Process

### Step 1: Read the source document

Read all of `brand/brand_definition.md`, focusing on:

- **Section 1 (Brand Overview):** Identity, design philosophy, materials/sustainability — used for context and informed the conditional terminology entries.
- **Section 3 (Audience Profiles):** Used to determine which audience slugs map to each adaptation mode.
- **Section 5 (Brand Voice Guidelines):** Primary source for all voice content — attributes, channel table, adaptation guidance, and word lists.

### Step 2: Derive core voice attributes

The source document names four attributes in prose form. Each attribute needed a `description` (condensed from the prose) and a `rules` array (actionable directives extracted from the attribute's paragraph).

**Source text (plainspoken):**
> "Short sentences. Common words. No jargon unless it's jargon the audience actually uses (fill power, denier, waterproof rating). If a ten-dollar word exists where a two-dollar word works, use the two-dollar word."

**Decision:** Preserve the original wording closely in the description. Convert the implicit directives into explicit imperative rules.

Repeat this extraction process for `confident`, `restrained`, and `dry`.

### Step 3: Map channels from the table

The source document contains a markdown table with four rows:

| Channel | Register | Example |
|---------|----------|---------|
| Product page | Technical, direct | "500D Cordura body. Rated to 45 lbs. 4 lbs 2 oz." |
| Email | Conversational, brief | "New tent. Low profile. Handles wind. Details below." |
| Social media | Casual, dry | "The Dirtbag. Named after its target market." |
| Customer service | Helpful, no-nonsense | "That shouldn't happen. Send a photo and we'll replace it." |

**Decision:** Use the exact example strings from the table — these are canonical and should not be paraphrased. Key channel names as snake_case slugs (`product_page`, `email`, `social_media`, `customer_service`). Add a `rules` array to each channel derived from the brand's broader voice principles applied to that context.

### Step 4: Build audience adaptations

The source document describes backcountry vs. casual adaptation under "Voice Adaptation: Backcountry vs. Casual."

**Decision:** Map to audience slugs from section 3:
- `backcountry` applies to `backcountry_hikers`, `thru_hikers`, and `cold_weather` (all share high technical literacy)
- `casual` applies to `casual_car_camping`

Each adaptation needed:
- `applies_to`: array of audience slugs
- `approach`: a short description of the communication mode
- `assumed_knowledge`: array derived from the explicit bullet list in the source document
- `guidelines`: array of actionable directives

### Step 5: Build terminology lists

The source document provides two word lists under "Words Yowie Uses" and "Words Yowie Doesn't Use."

**Approved terms:** 15 terms listed in the source. Each gets a `note` field — a brief explanation of how/when to use it, derived from how the terms appear in product copy across the brand definition.

**Forbidden terms:** 12 terms listed. Each gets a `reason` field explaining why it violates brand voice (e.g., "hyperbolic," "lifestyle-coded," "vague cliché").

**Conditional terms:** Three terms from the forbidden list have conditional rules (sustainable, eco-friendly, lightweight — these appear in the source with qualifications). Each gets:
- `rule`: when and how it may be used
- `correct`: an example of acceptable usage
- `incorrect`: an example of the violation

Note: `sustainable` appeared on the forbidden list but with a parenthetical — "as a leading claim — it's stated as fact when relevant." This signals conditional status.

### Step 6: Validate JSON

Run Python's built-in JSON parser to confirm syntax:

```bash
python3 -c "import json; json.load(open('brand/brand_config.json')); print('Valid JSON')"
```

Expected output: `Valid JSON`

---

## Key Decisions and Rationale

**Why JSON over YAML or TOML?**
JSON is universally parseable without a library in most languages, embeddable in HTML, and directly usable in JavaScript front-end viewers. The later HTML viewer task reads this file directly.

**Why preserve the source document's exact example strings?**
The channel examples are editorial decisions made by the brand — they are not samples to be improved, they are the canonical reference. Changing them would introduce interpretation drift.

**Why add `note` and `reason` fields beyond what was specified?**
The minimum spec was `term` only. Adding `note` (approved) and `reason` (forbidden) makes the file usable as a standalone reference — a copy reviewer can read why a term is forbidden without consulting the source document.

**Why include `correct` and `incorrect` examples in conditional terms?**
Conditional rules are the hardest to apply without examples. The source document itself uses this pattern implicitly — "stated as fact when relevant" is too vague to enforce. Concrete examples prevent misapplication.

---

## Final Output

**File path:** `brand/brand_config.json`

**Structure summary:**
- `meta`: 3 keys
- `voice.core_attributes`: 4 attributes × (1 description + 4 rules each)
- `voice.channels`: 4 channels × (register + 4 rules + 1 example each)
- `voice.audience_adaptations`: 2 modes × (applies_to + approach + assumed_knowledge + guidelines)
- `terminology.approved`: 15 terms with notes
- `terminology.forbidden`: 12 terms with reasons
- `terminology.conditional`: 3 terms with rule + correct + incorrect examples

---

## Replication Notes

To replicate this task from scratch:

1. Read the source brand definition document completely before writing any JSON.
2. For voice attributes, extract the prose description first, then convert implicit directives into explicit rules.
3. Copy channel examples verbatim — do not paraphrase.
4. Map audience slugs from the audience profiles section, not from the voice section alone.
5. For terminology, distinguish conditional terms from outright forbidden terms by looking for parenthetical qualifications in the source.
6. Always validate with a JSON parser before committing.
