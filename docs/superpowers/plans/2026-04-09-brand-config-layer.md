# Brand Configuration Layer Implementation Plan

> **For agentic workers:** REQUIRED SUB-SKILL: Use superpowers:subagent-driven-development (recommended) or superpowers:executing-plans to implement this plan task-by-task. Steps use checkbox (`- [ ]`) syntax for tracking.

**Goal:** Build a structured JSON rules engine encoding Yowie's brand voice, terminology, messaging, and compliance rules, plus an interactive HTML viewer for browsing rules by product/audience/channel combination.

**Architecture:** A standalone JSON config file (`brand/brand_config.json`) serves as the machine-readable source of truth. A self-contained HTML file (`tools/brand_config_viewer.html`) embeds that JSON and renders an interactive selector-panel UI. No dependencies, no build step, no server.

**Tech Stack:** JSON, vanilla HTML/CSS/JavaScript (single-file app)

---

## File Map

| File | Responsibility |
|------|---------------|
| `brand/brand_config.json` | All brand rules as structured data: voice, terminology, messaging, products, audiences, copy examples |
| `tools/brand_config_viewer.html` | Self-contained HTML viewer — embeds config JSON, renders selector bar + rules panel + copy preview |

---

### Task 1: Build the JSON Config — meta, voice, terminology

**Files:**
- Create: `brand/brand_config.json`

**Reference:** Read `brand/brand_definition.md` sections 1 (Brand Overview) and 5 (Brand Voice Guidelines) for all voice attributes, channel registers, audience adaptations, and terminology lists.

- [ ] **Step 1: Create `brand/brand_config.json` with `meta`, `voice`, and `terminology` sections**

```json
{
  "meta": {
    "version": "1.0",
    "date": "2026-04-09",
    "source": "brand/brand_definition.md"
  },
  "voice": {
    "core_attributes": {
      "plainspoken": {
        "description": "Short sentences. Common words. No jargon unless the audience actually uses it.",
        "rules": [
          "Use two-dollar words over ten-dollar words",
          "Short sentences preferred",
          "Technical jargon only when the audience uses it (denier, fill power, waterproof rating)",
          "No corporate-speak"
        ]
      },
      "confident": {
        "description": "Yowie knows what it built and why. No hedging.",
        "rules": [
          "No hedging or qualifiers",
          "Never use 'we believe' or 'we strive'",
          "State what the product does directly",
          "State trade-offs honestly — don't hide them"
        ]
      },
      "restrained": {
        "description": "No exclamation points. No superlatives without evidence. Undersell and overdeliver.",
        "rules": [
          "No exclamation points",
          "No superlatives without evidence",
          "Never use 'best'",
          "Use 'built for' instead of 'best for'",
          "Undersell and overdeliver"
        ]
      },
      "dry": {
        "description": "A sense of humor that doesn't try hard. Humor lives in product names and occasional asides, not in the copy itself.",
        "rules": [
          "Humor in product names and occasional asides only",
          "Never force humor into copy",
          "The Dirtbag is named that because it's funny and true — that's the model"
        ]
      }
    },
    "channels": {
      "product_page": {
        "register": "Technical, direct",
        "rules": [
          "Lead with specs and materials",
          "State measurements, weights, ratings as facts",
          "No narrative framing — just what the product is and does"
        ],
        "example": "500D Cordura body. Rated to 45 lbs. 4 lbs 2 oz."
      },
      "email": {
        "register": "Conversational, brief",
        "rules": [
          "Short sentences, short paragraphs",
          "Get to the point in the first line",
          "Link to product page for details — don't repeat the spec sheet"
        ],
        "example": "New tent. Low profile. Handles wind. Details below."
      },
      "social_media": {
        "register": "Casual, dry",
        "rules": [
          "One-liner or two-liner format",
          "Dry humor welcome here more than any other channel",
          "No hashtag stuffing — one or two relevant tags max",
          "Field photos only — no studio shots, no lifestyle staging"
        ],
        "example": "The Dirtbag. Named after its target market."
      },
      "customer_service": {
        "register": "Helpful, no-nonsense",
        "rules": [
          "Acknowledge the problem immediately",
          "No scripts, no canned responses that sound canned",
          "Offer a solution in the first reply",
          "Replacement over repair when possible"
        ],
        "example": "That shouldn't happen. Send a photo and we'll replace it."
      }
    },
    "audience_adaptations": {
      "backcountry": {
        "applies_to": ["backcountry_hikers", "thru_hikers", "cold_weather"],
        "approach": "Use specs directly — no translation needed",
        "assumed_knowledge": [
          "What denier ratings mean",
          "Why fill power matters",
          "The difference between comfort rating and lower limit",
          "What DAC poles are and why they matter",
          "Load rating and framesheet construction"
        ],
        "guidelines": [
          "Speak peer-to-peer — these buyers know as much as the designer",
          "Technical detail is a feature, not a barrier",
          "Honest trade-off descriptions build trust"
        ]
      },
      "casual": {
        "applies_to": ["casual_car_camping"],
        "approach": "Translate specs into outcomes",
        "assumed_knowledge": [
          "General sense of quality vs. cheap",
          "Basic outdoor experience — camping, fishing, tailgating",
          "Price-value comparison with known brands"
        ],
        "guidelines": [
          "Explain without condescending",
          "Focus on what the product does, not how",
          "Avoid gear-nerd jargon — translate to outcomes",
          "The 'no brand tax' message resonates here"
        ]
      }
    }
  },
  "terminology": {
    "approved": [
      { "term": "built", "note": "Core verb — implies intentional construction" },
      { "term": "engineered", "note": "For products with specific design decisions to highlight" },
      { "term": "designed", "note": "General purpose — use when 'engineered' feels too heavy" },
      { "term": "rated", "note": "For specs with measurable standards (load, temperature)" },
      { "term": "tested", "note": "When referencing real-world or lab performance data" },
      { "term": "measured", "note": "For precise specs — weights, dimensions, capacities" },
      { "term": "durable", "note": "Backed by material specs — not a vague claim" },
      { "term": "reliable", "note": "For products with field-proven track records" },
      { "term": "functional", "note": "Every feature serves a purpose" },
      { "term": "handles", "note": "For performance descriptions — 'handles wind', 'handles load'" },
      { "term": "manages", "note": "Similar to 'handles' — for load, moisture, temperature" },
      { "term": "performs", "note": "Use with specific conditions — 'performs in sustained wind'" },
      { "term": "honest", "note": "For brand-level messaging, not product copy" },
      { "term": "straightforward", "note": "Describes the buying experience and communication" },
      { "term": "direct", "note": "Describes communication style" }
    ],
    "forbidden": [
      { "term": "revolutionary", "reason": "Hyperbolic — nothing is revolutionary about a cooler" },
      { "term": "game-changing", "reason": "Startup jargon, not outdoor gear language" },
      { "term": "innovative", "reason": "Overused and vague — describe the actual innovation instead" },
      { "term": "passionate", "reason": "Performative — Yowie shows passion through product quality, not adjectives" },
      { "term": "inspired", "reason": "Lifestyle-brand language" },
      { "term": "curated", "reason": "Lifestyle-brand language" },
      { "term": "adventure", "reason": "Aspirational framing — Yowie doesn't sell experiences" },
      { "term": "journey", "reason": "Aspirational framing" },
      { "term": "epic", "reason": "Hyperbolic and overused" },
      { "term": "best", "reason": "Superlative without evidence — use 'built for' instead" },
      { "term": "ultimate", "reason": "Superlative without evidence" },
      { "term": "perfect", "reason": "Nothing is perfect — honesty over hype" }
    ],
    "conditional": [
      {
        "term": "sustainable",
        "rule": "State as fact when relevant ('recycled nylon shell'). Never use as a leading claim or marketing hook. Yowie doesn't wave a flag about it.",
        "correct": "Recycled nylon shell fabric.",
        "incorrect": "Our sustainable approach to outdoor gear."
      },
      {
        "term": "eco-friendly",
        "rule": "Same as 'sustainable' — describe the specific material or practice, don't use as a brand descriptor.",
        "correct": "Minimal packaging — no plastic clamshells.",
        "incorrect": "Yowie's eco-friendly product line."
      },
      {
        "term": "lightweight",
        "rule": "Only for products where weight savings is a design goal (Ridgeline 22L, Dirtbag 30°F). Never for products where durability is the priority (Basecamp 45L, Slab 35QT).",
        "correct": "1 lb 6 oz. Light enough for summit pushes.",
        "incorrect": "Our lightweight Basecamp pack."
      }
    ]
  }
}
```

- [ ] **Step 2: Validate JSON syntax**

Run: `cd "/Users/gregappler/Claude Code Projects/demo-marketing-ecosystem" && python3 -c "import json; json.load(open('brand/brand_config.json')); print('Valid JSON')"`
Expected: `Valid JSON`

- [ ] **Step 3: Commit**

```bash
git add brand/brand_config.json
git commit -m "Add brand config JSON: meta, voice, and terminology sections"
```

---

### Task 2: Add messaging, products, and audiences to JSON config

**Files:**
- Modify: `brand/brand_config.json`

**Reference:** Read `brand/brand_definition.md` sections 2 (Product Catalog), 3 (Audience Profiles), and 4 (Messaging Pillars).

- [ ] **Step 1: Add `messaging` section to config**

Add after `terminology`, before the closing `}`. The four pillars with IDs, and emphasis mappings per product line and audience:

```json
"messaging": {
  "pillars": {
    "built_for_job": {
      "headline": "Built for the job, not the brand.",
      "description": "Every product is designed around its specific use case. No features for the spec sheet. No materials for the marketing copy.",
      "resonance": {
        "backcountry": "We respect the job you're doing and built gear that matches it.",
        "casual": "This does what you need. Nothing extra."
      }
    },
    "quality_you_verify": {
      "headline": "Quality you verify, not quality we claim.",
      "description": "Real specs, real materials data, real-world performance. The products hold up to scrutiny because they're built to hold up to scrutiny.",
      "resonance": {
        "backcountry": "Dig into the specs. The more you research, the more you'll find to like.",
        "casual": "Check the reviews. Compare the build. We're not hiding anything."
      }
    },
    "pay_for_gear": {
      "headline": "Pay for the gear, not the logo.",
      "description": "Minimal branding. No aspirational marketing. No sponsorship deals. The savings get reinvested into materials and construction.",
      "resonance": {
        "backcountry": "We respect your intelligence — you're paying for materials, not marketing.",
        "casual": "Same quality as the big names. No brand tax."
      }
    },
    "responsible_materials": {
      "headline": "Responsible materials, zero grandstanding.",
      "description": "Recycled fabrics, natural insulation, minimal packaging — because these choices make sense, not because they make good Instagram posts.",
      "resonance": {
        "backcountry": "Good materials happen to be responsible materials. That's the whole story.",
        "casual": "We use recycled and natural materials where it makes sense. No lecture."
      }
    }
  },
  "product_emphasis": {
    "backpacks": ["built_for_job", "quality_you_verify", "pay_for_gear", "responsible_materials"],
    "tents": ["built_for_job", "quality_you_verify", "responsible_materials", "pay_for_gear"],
    "sleeping_bags": ["quality_you_verify", "built_for_job", "responsible_materials", "pay_for_gear"],
    "coolers": ["pay_for_gear", "built_for_job", "quality_you_verify", "responsible_materials"]
  },
  "audience_emphasis": {
    "backcountry_hikers": ["built_for_job", "quality_you_verify", "pay_for_gear", "responsible_materials"],
    "thru_hikers": ["quality_you_verify", "built_for_job", "pay_for_gear", "responsible_materials"],
    "casual_car_camping": ["pay_for_gear", "built_for_job", "quality_you_verify", "responsible_materials"],
    "cold_weather": ["quality_you_verify", "built_for_job", "responsible_materials", "pay_for_gear"]
  }
}
```

- [ ] **Step 2: Add `products` section**

Add after `messaging`. All 8 products keyed by slug:

```json
"products": {
  "basecamp_45l": {
    "name": "Basecamp 45L",
    "line": "backpacks",
    "price": 385,
    "description": "Multi-day backcountry hauler built for loads up to 45 pounds over rough, trailless terrain.",
    "key_features": ["500D Cordura body", "Framesheet rated to 45 lbs", "Rolltop closure", "Recycled nylon shell", "4 lbs 2 oz"],
    "audiences": ["backcountry_hikers", "thru_hikers"]
  },
  "ridgeline_22l": {
    "name": "Ridgeline 22L",
    "line": "backpacks",
    "price": 185,
    "description": "Stripped-down daypack for summit pushes, day hikes, and fast-and-light side trips.",
    "key_features": ["210D ripstop nylon body", "500D Cordura base", "Removable hip belt", "Recycled nylon shell", "1 lb 6 oz"],
    "audiences": ["backcountry_hikers", "thru_hikers"]
  },
  "lowline_2p": {
    "name": "Lowline 2P",
    "line": "tents",
    "price": 475,
    "description": "Two-person, three-season tent built low and tight for wind resistance.",
    "key_features": ["3000mm waterproof coating", "DAC Featherlite poles", "40\" peak height", "Two vestibules", "Recycled polyester fly", "4 lbs 8 oz"],
    "audiences": ["backcountry_hikers", "cold_weather"]
  },
  "basecamp_4p": {
    "name": "Basecamp 4P",
    "line": "tents",
    "price": 695,
    "description": "Four-person basecamp tent for multi-day trips in variable conditions.",
    "key_features": ["5000mm waterproof coating", "DAC Pressfit poles", "Near-vertical walls", "Full-coverage fly", "Recycled polyester fly", "Footprint included", "9 lbs 4 oz"],
    "audiences": ["backcountry_hikers", "casual_car_camping", "cold_weather"]
  },
  "dirtbag_30f": {
    "name": "Dirtbag 30°F",
    "line": "sleeping_bags",
    "price": 295,
    "description": "Three-season bag for campers who sleep out from spring through fall.",
    "key_features": ["650-fill duck down", "20D recycled polyester shell", "Comfort rating: 30°F", "Lower limit: 19°F", "Mummy cut", "2 lbs 4 oz"],
    "audiences": ["backcountry_hikers", "thru_hikers"]
  },
  "burrow_0f": {
    "name": "Burrow 0°F",
    "line": "sleeping_bags",
    "price": 445,
    "description": "Winter bag for cold-weather specialists. Comfort-rated to 0°F.",
    "key_features": ["800-fill goose down", "20D recycled polyester shell with DWR", "Comfort rating: 0°F", "Lower limit: -15°F", "Trapezoidal baffles", "3 lbs 6 oz"],
    "audiences": ["backcountry_hikers", "cold_weather"]
  },
  "slab_35qt": {
    "name": "Slab 35QT",
    "line": "coolers",
    "price": 285,
    "description": "35-quart hard cooler that keeps ice for days, not hours.",
    "key_features": ["Rotomolded polyethylene", "3\" commercial-grade insulation", "5+ day ice retention", "Bear-resistant certified", "Marine-grade hardware", "22 lbs"],
    "audiences": ["casual_car_camping"]
  },
  "foldout": {
    "name": "Foldout Soft Cooler",
    "line": "coolers",
    "price": 145,
    "description": "Collapsible soft cooler for day trips, grocery runs, and anywhere a hard cooler is overkill.",
    "key_features": ["840D TPU-coated polyester", "Welded leak-proof liner", "24-hour ice retention", "Magnetic closure", "Folds flat to 3\"", "3 lbs 2 oz"],
    "audiences": ["casual_car_camping"]
  }
}
```

- [ ] **Step 3: Add `audiences` section**

Add after `products`:

```json
"audiences": {
  "backcountry_hikers": {
    "name": "Backcountry Hikers",
    "product_lines": ["backpacks", "tents", "sleeping_bags"],
    "voice_adaptation": "backcountry",
    "profile": "Experienced hikers with 5+ years backcountry travel. Distrust marketing-heavy brands. Research extensively. Loyalty goes to products, not companies."
  },
  "thru_hikers": {
    "name": "Thru-Hikers",
    "product_lines": ["backpacks", "sleeping_bags"],
    "voice_adaptation": "backcountry",
    "profile": "Weight-conscious but not weight-obsessed. Need gear that performs over 2,000+ miles. Community-driven purchase decisions."
  },
  "casual_car_camping": {
    "name": "Casual / Car Camping",
    "product_lines": ["coolers", "tents"],
    "voice_adaptation": "casual",
    "profile": "Quality-aware but not gear-obsessed. Tired of replacing cheap gear. See premium brands as overpriced. Shorter research cycles, more influenced by recommendations."
  },
  "cold_weather": {
    "name": "Cold-Weather Specialists",
    "product_lines": ["sleeping_bags", "tents"],
    "voice_adaptation": "backcountry",
    "profile": "Conditions-driven buyers. Most spec-literate segment. Trust independent lab testing. Will pay premium if specs justify it."
  }
}
```

- [ ] **Step 4: Validate JSON syntax**

Run: `cd "/Users/gregappler/Claude Code Projects/demo-marketing-ecosystem" && python3 -c "import json; data = json.load(open('brand/brand_config.json')); print(f'Valid JSON — {len(data)} top-level keys: {list(data.keys())}')"`
Expected: `Valid JSON — 6 top-level keys: ['meta', 'voice', 'terminology', 'messaging', 'products', 'audiences']`

- [ ] **Step 5: Commit**

```bash
git add brand/brand_config.json
git commit -m "Add messaging, products, and audiences to brand config"
```

---

### Task 3: Add copy examples to JSON config

**Files:**
- Modify: `brand/brand_config.json`

**Context:** Write ~20-25 hand-crafted copy examples in Yowie's voice. These are keyed by `product_slug|audience_slug|channel` triplets. Each example must follow the voice rules: plainspoken, confident, restrained, dry. Backcountry examples use specs directly. Casual examples translate specs into outcomes.

- [ ] **Step 1: Add `examples` section to config**

Add after `audiences`. This is the full set of examples — approximately 22 entries covering the combinations specified in the design spec:

```json
"examples": {
  "basecamp_45l|backcountry_hikers|product_page": {
    "copy": "500D Cordura body with reinforced bottom panel. Framesheet and aluminum stays rated to 45 lbs. Rolltop closure. Four attachment points. 4 lbs 2 oz. This is not an ultralight pack. It's a pack that carries real weight over real terrain without falling apart.",
    "rules_demonstrated": ["Technical register — specs stated as facts", "Confident — no hedging", "Restrained — no superlatives", "Honest trade-off acknowledgment (weight vs. durability)"]
  },
  "basecamp_45l|backcountry_hikers|email": {
    "copy": "The Basecamp 45L. Same pack, updated hip belt geometry. 500D Cordura. Rated to 45 lbs. If your current pack is showing wear, this is the replacement that won't.",
    "rules_demonstrated": ["Conversational, brief register", "Specs included but not exhaustive", "Confident closing statement"]
  },
  "basecamp_45l|backcountry_hikers|social_media": {
    "copy": "45 liters. 45-pound load rating. 500D Cordura. The math is simple.",
    "rules_demonstrated": ["Casual, dry register", "Dry humor — 'the math is simple'", "Spec-forward even on social"]
  },
  "basecamp_45l|thru_hikers|product_page": {
    "copy": "4 lbs 2 oz is not light. But 2,000 miles from now, the zippers still work and the fabric isn't shredding. The Basecamp trades ounces for longevity. If you've replaced a pack mid-trail, you know why.",
    "rules_demonstrated": ["Backcountry adaptation — speaks peer-to-peer", "Honest trade-off (weight vs. durability)", "Confident — no apologizing for the weight"]
  },
  "ridgeline_22l|backcountry_hikers|product_page": {
    "copy": "210D ripstop body, 500D Cordura base. 1 lb 6 oz. Light where it can be, reinforced where it contacts rock. Summit pushes, day hikes, side trips from basecamp.",
    "rules_demonstrated": ["Technical register", "Design philosophy stated directly — light where possible, tough where necessary"]
  },
  "ridgeline_22l|thru_hikers|social_media": {
    "copy": "Leave the Basecamp at camp. Grab the Ridgeline. 1 lb 6 oz, Cordura base. Go get the summit and come back.",
    "rules_demonstrated": ["Casual register", "Cross-product reference", "Action-oriented, no fluff"]
  },
  "lowline_2p|backcountry_hikers|product_page": {
    "copy": "40-inch peak height. Wide footprint. Two vestibules. Built low and tight because ridgeline wind doesn't care about your headroom. 3000mm waterproof coating, DAC Featherlite poles, sealed seams. 4 lbs 8 oz packed.",
    "rules_demonstrated": ["Technical register", "Design rationale stated plainly — why it's low", "Specs as facts"]
  },
  "lowline_2p|cold_weather|email": {
    "copy": "The Lowline 2P. Low-profile, wind-resistant, two vestibules for gear. 3000mm coating, sealed seams, DAC poles. If you're camping above treeline, this is the tent that stays standing.",
    "rules_demonstrated": ["Conversational register", "Cold-weather audience — assumes knowledge of conditions", "Confident closing"]
  },
  "basecamp_4p|backcountry_hikers|product_page": {
    "copy": "9 lbs 4 oz. Not a backpacking tent. A basecamp tent for groups who set up and stay put. 5000mm waterproof coating, DAC Pressfit poles oversized for wind load. Near-vertical walls because four adults and their gear need room. Two doors. Two vestibules. Footprint included.",
    "rules_demonstrated": ["Technical register", "Honest about what it isn't (not a backpacking tent)", "Design rationale for weight and wall geometry"]
  },
  "basecamp_4p|casual_car_camping|product_page": {
    "copy": "Room for four adults and their gear. Walls that stand up straight so you're not hunched over. Two doors so nobody climbs over anybody. Handles heavy rain and sustained wind. 5000mm waterproof rating — that's serious waterproofing. Footprint included.",
    "rules_demonstrated": ["Casual adaptation — translates specs to outcomes", "Explains waterproof rating without assuming knowledge", "Same confident voice, different assumed knowledge"]
  },
  "dirtbag_30f|backcountry_hikers|product_page": {
    "copy": "650-fill duck down. 20D recycled polyester shell. Comfort-rated to 30°F, lower limit 19°F. Mummy cut with draft collar. 2 lbs 4 oz. Named for the people who use it — the ones who'd rather be dirty and outside than clean and home.",
    "rules_demonstrated": ["Technical register", "Dry humor — the name explanation", "Specs stated as facts with no embellishment"]
  },
  "dirtbag_30f|backcountry_hikers|social_media": {
    "copy": "The Dirtbag. Named after its target market. 650-fill down, 30°F comfort rating, 2 lbs 4 oz. You know who you are.",
    "rules_demonstrated": ["Casual, dry register", "Dry humor — self-aware naming", "Backcountry audience — specs without explanation"]
  },
  "dirtbag_30f|thru_hikers|email": {
    "copy": "Three-season bag. 650-fill down. 2 lbs 4 oz packed. Comfort-rated to 30°F. Spring through fall without carrying winter weight. The Dirtbag — it packs small and lasts longer than the cheap one you're about to replace.",
    "rules_demonstrated": ["Conversational register", "Thru-hiker focus — weight and packability", "Confident, honest comparison to competitors"]
  },
  "burrow_0f|cold_weather|product_page": {
    "copy": "800-fill goose down. Trapezoidal baffles. Comfort-rated to 0°F, lower limit -15°F. DWR-coated 20D recycled shell. Full-length insulated draft tube. 3 lbs 6 oz. This bag is for people who camp when the temperature drops below the point where most people stop camping.",
    "rules_demonstrated": ["Technical register", "Cold-weather audience — maximum spec detail", "Confident positioning — defines the buyer"]
  },
  "burrow_0f|backcountry_hikers|social_media": {
    "copy": "0°F comfort rating. 800-fill goose down. Trapezoidal baffles. Winter doesn't care if you're cold. The Burrow does.",
    "rules_demonstrated": ["Casual, dry register", "Dry tone — 'winter doesn't care'", "Spec-forward"]
  },
  "burrow_0f|cold_weather|email": {
    "copy": "The Burrow 0°F. 800-fill goose down. Comfort-rated to 0°F, lower limit -15°F. Trapezoidal baffles, insulated draft tube, DWR-coated shell. If you're sleeping outside below freezing, this is the bag.",
    "rules_demonstrated": ["Conversational, brief register", "Cold-weather audience — full spec detail in email", "Confident closer"]
  },
  "slab_35qt|casual_car_camping|product_page": {
    "copy": "Keeps ice for five days in 90-degree heat. Rotomolded — one piece, no seams, nothing to crack or leak. 3 inches of commercial-grade insulation. You can stand on it. You can drag it across gravel. Bear-resistant certified. 35 quarts, 24 cans plus ice. Competes with coolers that cost $100 more because those coolers are paying for a logo.",
    "rules_demonstrated": ["Casual adaptation — specs translated to outcomes", "Confident competitive positioning", "'Pay for the gear' pillar in action", "Plain English throughout"]
  },
  "slab_35qt|casual_car_camping|email": {
    "copy": "The Slab 35QT. Five-day ice retention. Rotomolded, bear-resistant, built to get dragged around. Same quality as the $400 coolers. Less money. No logo tax.",
    "rules_demonstrated": ["Conversational, brief register", "Casual audience — outcome-focused", "'Pay for the gear' messaging"]
  },
  "slab_35qt|casual_car_camping|social_media": {
    "copy": "Five days of ice. $285. No celebrity endorsement included.",
    "rules_demonstrated": ["Casual, dry register", "Dry humor — the celebrity jab", "'Pay for the gear' pillar", "Maximum brevity"]
  },
  "slab_35qt|casual_car_camping|customer_service": {
    "copy": "That latch shouldn't be sticking after two months. Send us a photo and your order number — we'll ship a replacement cooler, not just the part. You'll have it in 3-4 business days.",
    "rules_demonstrated": ["Helpful, no-nonsense register", "Immediate acknowledgment", "Solution in first reply", "Replacement over repair"]
  },
  "foldout|casual_car_camping|product_page": {
    "copy": "Folds flat to 3 inches. Stands up when loaded. Welded liner — no leaks, not now, not after a year of use. Closed-cell foam keeps ice for 24 hours. Magnetic closure, shoulder strap, zip pocket for your keys. 3 lbs 2 oz. For day trips, groceries, and anywhere a hard cooler is more than you need.",
    "rules_demonstrated": ["Casual adaptation — outcomes and use cases", "Durability claim with time frame", "Plain language throughout"]
  },
  "foldout|casual_car_camping|social_media": {
    "copy": "A soft cooler that isn't disposable. 24-hour ice retention. Folds flat. $145.",
    "rules_demonstrated": ["Casual, dry register", "Competitive dig without naming competitors", "Price as a feature"]
  }
}
```

Total: 22 examples covering all 8 products on product_page, both coolers across all 4 channels, key backcountry products on email and social_media, and cross-audience examples (Basecamp 4P for backcountry vs. casual, Basecamp 45L for backcountry vs. thru-hikers).

- [ ] **Step 2: Validate JSON syntax and count examples**

Run: `cd "/Users/gregappler/Claude Code Projects/demo-marketing-ecosystem" && python3 -c "import json; data = json.load(open('brand/brand_config.json')); print(f'Valid JSON — {len(data[\"examples\"])} examples'); print('Keys:', list(data.keys()))"`
Expected: `Valid JSON — 22 examples` and all 7 top-level keys listed (meta, voice, terminology, messaging, products, audiences, examples).

- [ ] **Step 3: Commit**

```bash
git add brand/brand_config.json
git commit -m "Add 22 hand-crafted copy examples to brand config"
```

---

### Task 4: Build HTML viewer — structure, styles, embedded config

**Files:**
- Create: `tools/brand_config_viewer.html`

**Context:** Single self-contained HTML file. Dark theme, system fonts, gold accents. The JSON config from `brand/brand_config.json` is embedded in a `<script>` tag. This task creates the file with HTML structure, all CSS, and the embedded config data. Task 5 adds the JavaScript interactivity.

- [ ] **Step 1: Create `tools/` directory**

Run: `mkdir -p "/Users/gregappler/Claude Code Projects/demo-marketing-ecosystem/tools"`

- [ ] **Step 2: Create `tools/brand_config_viewer.html` with HTML structure and CSS**

Write the complete HTML file with:

1. `<!DOCTYPE html>` with meta charset and viewport
2. `<title>Yowie Brand Configuration</title>`
3. A `<style>` block with all CSS:
   - CSS custom properties for the color palette:
     - `--bg-primary: #0a0a1a` (page background)
     - `--bg-secondary: #12122a` (card backgrounds)
     - `--bg-selector: #16213e` (selector bar)
     - `--text-primary: #ccd6f6` (main text)
     - `--text-secondary: #8892b0` (muted text)
     - `--accent-gold: #e2c87d` (active states, labels)
     - `--accent-teal: #64ffda` (positive indicators)
     - `--accent-red: #ff6b6b` (forbidden indicators)
     - `--border: rgba(255,255,255,0.08)` (card borders)
   - System font stack: `-apple-system, BlinkMacSystemFont, 'Segoe UI', sans-serif`
   - Selector bar: horizontal layout, button groups with labels
   - Toggle buttons: default state (dark, muted text), active state (gold background, dark text)
   - 2x2 card grid using CSS Grid: `grid-template-columns: 1fr 1fr; gap: 16px`
   - Card styling: dark background, subtle border, gold label header
   - Copy preview: blockquote with gold left border, annotation text below
   - Responsive: cards stack to single column below 768px width (laptops in split view)
4. The HTML body structure:
   - Header: "Yowie Brand Configuration" title
   - Selector bar: three groups (Product, Audience, Channel) with button elements. Each button has a `data-group` and `data-value` attribute.
   - Rules panel: four card `<div>`s with IDs (`voice-card`, `messaging-card`, `terminology-card`, `compliance-card`)
   - Copy preview: a `<div>` with ID `copy-preview`
5. A `<script>` tag containing `const CONFIG = ` followed by the entire contents of `brand/brand_config.json`
6. A second `<script>` tag (empty for now — JavaScript added in Task 5)

The HTML for the selector buttons. All 8 products, 4 audiences, 4 channels as `<button>` elements:

**Product buttons** (data-group="product"):
- `data-value="basecamp_45l"` → "Basecamp 45L"
- `data-value="ridgeline_22l"` → "Ridgeline 22L"
- `data-value="lowline_2p"` → "Lowline 2P"
- `data-value="basecamp_4p"` → "Basecamp 4P"
- `data-value="dirtbag_30f"` → "Dirtbag 30°F"
- `data-value="burrow_0f"` → "Burrow 0°F"
- `data-value="slab_35qt"` → "Slab 35QT"
- `data-value="foldout"` → "Foldout"

**Audience buttons** (data-group="audience"):
- `data-value="backcountry_hikers"` → "Backcountry Hikers"
- `data-value="thru_hikers"` → "Thru-Hikers"
- `data-value="casual_car_camping"` → "Casual / Car Camping"
- `data-value="cold_weather"` → "Cold-Weather Specialists"

**Channel buttons** (data-group="channel"):
- `data-value="product_page"` → "Product Page"
- `data-value="email"` → "Email"
- `data-value="social_media"` → "Social Media"
- `data-value="customer_service"` → "Customer Service"

Default active: `basecamp_45l`, `backcountry_hikers`, `product_page`.

- [ ] **Step 3: Verify the file opens in a browser**

Run: `open "/Users/gregappler/Claude Code Projects/demo-marketing-ecosystem/tools/brand_config_viewer.html"`

Visually verify: dark background, selector bar with gold-highlighted defaults, four empty cards, empty copy preview area. No JavaScript errors in console.

- [ ] **Step 4: Commit**

```bash
git add tools/brand_config_viewer.html
git commit -m "Add HTML viewer structure, styles, and embedded config"
```

---

### Task 5: Add JavaScript interactivity to HTML viewer

**Files:**
- Modify: `tools/brand_config_viewer.html`

**Context:** Add the JavaScript that powers the interactive behavior. The config data is already embedded as `CONFIG`. This task adds: selector button handling, rules panel rendering, copy preview rendering, and URL hash state.

- [ ] **Step 1: Add JavaScript in the second `<script>` tag**

The JavaScript has these parts:

**State management:**

```javascript
const state = {
  product: 'basecamp_45l',
  audience: 'backcountry_hikers',
  channel: 'product_page'
};
```

**Button click handler:**

```javascript
function selectOption(group, value) {
  state[group] = value;
  // Update button active states for this group
  document.querySelectorAll(`[data-group="${group}"]`).forEach(btn => {
    btn.classList.toggle('active', btn.dataset.value === value);
  });
  updateDisplay();
  updateHash();
}
```

Attach click listeners to all buttons:
```javascript
document.querySelectorAll('button[data-group]').forEach(btn => {
  btn.addEventListener('click', () => selectOption(btn.dataset.group, btn.dataset.value));
});
```

**`updateDisplay()` function — renders all four cards and the copy preview:**

Voice card (`#voice-card`):
1. Show channel register: `CONFIG.voice.channels[state.channel].register`
2. Show channel rules: `CONFIG.voice.channels[state.channel].rules`
3. Determine adaptation mode: `CONFIG.audiences[state.audience].voice_adaptation`
4. Show adaptation approach and assumed knowledge from `CONFIG.voice.audience_adaptations[adaptationMode]`
5. List core attributes (always the same four, as a compact line)

Messaging card (`#messaging-card`):
1. Get the product's line: `CONFIG.products[state.product].line`
2. Get product emphasis order: `CONFIG.messaging.product_emphasis[productLine]`
3. Get audience emphasis order: `CONFIG.messaging.audience_emphasis[state.audience]`
4. Compute combined rank: for each pillar, average its index in both arrays. Lower average = more primary.
5. Sort pillars by combined rank.
6. Render all four pillars. Top 2 are "primary" (full opacity, filled dot `●`). Bottom 2 are "secondary" (dimmed, hollow dot `○`).
7. Show the pillar's resonance note for the current adaptation mode.

Terminology card (`#terminology-card`):
- Render approved terms with green checkmark
- Render forbidden terms with red x and strikethrough
- Render conditional terms with amber warning and usage rule
- This card content is static — same for all selections

Compliance card (`#compliance-card`):
- Render the style and compliance rules (static content):
  - No exclamation points
  - No superlatives without evidence
  - No lifestyle imagery or aspirational framing
  - Minimal logo placement
  - Eco claims stated as fact, not marketing
  - No influencer partnerships
  - No studio shots — field photos only
  - Minimal packaging claims only when factually describing packaging

Copy preview (`#copy-preview`):
1. Build the example key: `${state.product}|${state.audience}|${state.channel}`
2. If `CONFIG.examples[key]` exists: render the copy in a blockquote, render `rules_demonstrated` as a list below
3. If not: show "No example for this combination" in muted text, followed by a summary of which rules currently apply (channel register + adaptation mode)

**URL hash state:**

```javascript
function updateHash() {
  window.location.hash = `product=${state.product}&audience=${state.audience}&channel=${state.channel}`;
}

function loadFromHash() {
  const hash = window.location.hash.slice(1);
  if (!hash) return;
  const params = new URLSearchParams(hash);
  if (params.get('product') && CONFIG.products[params.get('product')]) {
    state.product = params.get('product');
  }
  if (params.get('audience') && CONFIG.audiences[params.get('audience')]) {
    state.audience = params.get('audience');
  }
  if (params.get('channel') && CONFIG.voice.channels[params.get('channel')]) {
    state.channel = params.get('channel');
  }
  // Update button active states to match loaded state
  ['product', 'audience', 'channel'].forEach(group => {
    document.querySelectorAll(`[data-group="${group}"]`).forEach(btn => {
      btn.classList.toggle('active', btn.dataset.value === state[group]);
    });
  });
}
```

**Initialization:**

```javascript
loadFromHash();
updateDisplay();
```

- [ ] **Step 2: Test in browser — default state**

Run: `open "/Users/gregappler/Claude Code Projects/demo-marketing-ecosystem/tools/brand_config_viewer.html"`

Verify:
- Basecamp 45L / Backcountry Hikers / Product Page selected (gold buttons)
- Voice card shows "Technical, direct" register with backcountry adaptation rules
- Messaging card shows 4 pillars with "Built for the job" and "Quality you verify" as primary
- Terminology card shows approved (green), forbidden (red), conditional (amber) terms
- Copy preview shows the Basecamp 45L product page example

- [ ] **Step 3: Test — switch to casual audience and cooler product**

Click "Slab 35QT" then "Casual / Car Camping" then "Social Media".

Verify:
- Voice card switches to casual adaptation: "Translate specs into outcomes"
- Messaging card re-ranks pillars: "Pay for the gear" becomes primary
- Copy preview shows: "Five days of ice. $285. No celebrity endorsement included."
- URL hash updates to `#product=slab_35qt&audience=casual_car_camping&channel=social_media`

- [ ] **Step 4: Test — hash state loading**

Copy the URL with the hash from step 3. Open a new browser tab and paste it.

Verify: page loads with Slab 35QT / Casual / Social Media pre-selected and correct content displayed.

- [ ] **Step 5: Test — missing example**

Click "Ridgeline 22L" then "Cold-Weather Specialists" then "Customer Service".

Verify: copy preview shows "No example for this combination" with a summary of applicable rules.

- [ ] **Step 6: Commit**

```bash
git add tools/brand_config_viewer.html
git commit -m "Add JavaScript interactivity to brand config viewer"
```

---

### Task 6: Process documentation and final commit

**Files:**
- Modify: `docs/process_log.md`
- Create: `tutorials/02_brand_config_layer.md`

- [ ] **Step 1: Add process log entry**

Prepend a new entry to `docs/process_log.md` (after the YAML header, before the existing entry):

```markdown
## 2026-04-09 — Brand Configuration Layer

**Prompt:** Build the Brand Configuration Layer for Yowie. This is not a document — it's the rules engine that governs every piece of content the ecosystem produces. Read the brand definition and create a structured configuration that encodes: voice attributes and channel-specific registers, audience-specific voice adaptations (backcountry vs. casual), messaging hierarchy by product line and audience segment, approved and forbidden terminology, style standards, and compliance rules. Output as an interactive HTML tool where I can browse the configuration, see how rules apply to different product/audience/channel combinations, and preview how the voice shifts by context.

**What was built:** Two files — a structured JSON rules engine (`brand/brand_config.json`) encoding all brand voice, terminology, messaging, and compliance rules, and an interactive HTML viewer (`tools/brand_config_viewer.html`) with product/audience/channel selectors that show combined rulesets and copy previews.

**Key decisions:**
- JSON config as machine-readable source of truth, separate from the HTML viewer, so future tools can import the rules programmatically
- Selector panel UI (product × audience × channel) over matrix view — matches the real workflow of "I'm writing for X, show me the rules"
- Static hand-crafted copy examples (~22) over generated templates — the voice is too nuanced for templates to capture
- Single self-contained HTML file with embedded config — no server, no dependencies, any team member can open it
- Messaging pillar ranking computed by averaging product-line and audience emphasis indices — gives nuanced primary/secondary distinction
- URL hash state for bookmarking specific combinations

**Files created or modified:**
- `brand/brand_config.json` (created)
- `tools/brand_config_viewer.html` (created)
- `docs/process_log.md` (modified)
- `tutorials/02_brand_config_layer.md` (created)
```

- [ ] **Step 2: Create tutorial**

Write `tutorials/02_brand_config_layer.md`:

```markdown
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

## Key Decisions and Rationale

### JSON as the config format

The brand rules need to be both human-browsable and machine-readable. JSON serves both: it's structured enough for programmatic consumption by future tools, and the HTML viewer renders it for human use. YAML was considered but adds a build step for browser consumption.

### Selector panel interaction model

Three selector groups (product, audience, channel) with toggle buttons. This mirrors the real workflow: "I'm writing about the Slab for casual buyers on social media — what rules apply?" The alternative was a matrix view, but that optimizes for pattern-spotting over practical use.

### Hand-crafted copy examples

22 pre-written examples covering key product/audience/channel combinations. Templates were rejected because Yowie's voice — especially the dry humor and confident tone — can't be captured by slot-filling. The examples serve as reference copy, not generated output.

### Messaging pillar ranking

Pillar priority for a given combination is computed by averaging the pillar's rank in the product-line emphasis array and the audience emphasis array. This produces nuanced primary/secondary distinctions without manually mapping every combination.

### Single-file HTML

The viewer embeds the JSON config directly. No server, no build step, no dependencies. Any team member can open the file in a browser. Trade-off: updating the config requires copying the new JSON into the HTML file. Acceptable for a 6-person team.

## How to Replicate

1. **Start with the brand definition.** You need defined voice attributes, product specs, audience profiles, messaging pillars, and terminology rules.

2. **Structure the JSON config.** Seven top-level keys: meta, voice, terminology, messaging, products, audiences, examples. Build iteratively — voice and terminology first, then products and audiences, then messaging mappings, then examples last.

3. **Write copy examples.** Pick the most important product/audience/channel combinations. Write 2-3 sentences per example in the brand voice. Annotate which rules each example demonstrates. Aim for 20-25 examples.

4. **Build the HTML viewer.** Embed the JSON in a script tag. Three selector groups with toggle buttons. Four rules cards in a 2x2 grid. Copy preview at the bottom. URL hash state for bookmarking.

5. **Test cross-audience adaptation.** The most valuable test: pick a product that serves both backcountry and casual audiences (like the Basecamp 4P tent). Switch between audiences and verify the voice register, messaging emphasis, and copy preview all shift appropriately.

## Output

- Config: `brand/brand_config.json`
- Viewer: `tools/brand_config_viewer.html`
```

- [ ] **Step 3: Commit**

```bash
git add docs/process_log.md tutorials/02_brand_config_layer.md
git commit -m "Add process log and tutorial for brand config layer"
```
