# Research & Brief Builder Implementation Plan

> **For agentic workers:** REQUIRED SUB-SKILL: Use superpowers:subagent-driven-development (recommended) or superpowers:executing-plans to implement this plan task-by-task. Steps use checkbox (`- [ ]`) syntax for tracking.

**Goal:** Build an interactive HTML tool that demonstrates Yowie's upstream research-to-brief pipeline — competitive analysis, audience research, and reasoning chains that produce structured campaign briefs compatible with the Content Engine.

**Architecture:** Single self-contained HTML file (`tools/brief_builder.html`) with an embedded `BUILDER_DATA` JSON object containing pipeline metadata, 3 pre-generated research briefs with competitive landscapes, audience analyses, reasoning chains, and output briefs. The UI has a pipeline diagram at top, sidebar brief selector, and scrollable main content with research/reasoning/brief/review sections.

**Tech Stack:** Vanilla HTML/CSS/JS, no dependencies. JSON data embedded inline.

---

## File Structure

- **Create:** `tools/brief_builder.html` — the complete interactive tool (HTML + CSS + JS + embedded JSON data)

---

### Task 1: Build the embedded JSON data block with pipeline metadata and all 3 briefs

**Files:**
- Create: `tools/brief_builder.html` (initial file — JSON data block + HTML skeleton only)

This task creates the file and populates all content data. The JSON structure has two sections: `pipeline` (stage metadata) and `briefs` (3 research brief objects).

- [ ] **Step 1: Create the HTML file with the embedded BUILDER_DATA JSON**

Create `tools/brief_builder.html` with the full JSON data block inside a `<script>` tag. The JSON contains:

**`pipeline`** object — metadata for the 5-stage diagram:
```json
{
  "stages": [
    {
      "id": "raw_inputs",
      "label": "Raw Inputs",
      "subtitle": "Product, market, season",
      "color": "teal",
      "scroll_to": "section-research"
    },
    {
      "id": "ai_research",
      "label": "AI Research",
      "subtitle": "Competitive, audience",
      "color": "teal",
      "scroll_to": "section-research"
    },
    {
      "id": "draft_brief",
      "label": "Draft Brief",
      "subtitle": "Structured output",
      "color": "teal",
      "scroll_to": "section-brief"
    },
    {
      "id": "human_review",
      "label": "Human Review",
      "subtitle": "Approve / revise",
      "color": "gold",
      "scroll_to": "section-review",
      "checklist": [
        "Does the primary message match the positioning angle?",
        "Are the proof points verifiable and specific?",
        "Is the channel mix right for this audience's media habits?",
        "Does the CTA match the campaign objective?",
        "Any tone overrides needed?"
      ]
    },
    {
      "id": "content_engine",
      "label": "Content Engine",
      "subtitle": "Generate content",
      "color": "teal",
      "scroll_to": null
    }
  ],
  "tagline": "AI produces drafts. Humans make decisions."
}
```

**`briefs`** array — 3 objects. Each brief has these fields:
- `id` (string)
- `name` (string)
- `description` (string)
- `product_id` (string) — matches brand_config.json
- `campaign_objective` (string)
- `research` object:
  - `competitive_landscape` (array of objects with: name, price_range, positioning, strengths (array), weaknesses (array))
  - `audience_analysis` (object with: profile (string), pain_points (array), buying_triggers (array), media_habits (array))
  - `seasonal_context` (string)
  - `positioning_angle` (string)
- `reasoning` object:
  - `message_rationale` (string)
  - `channel_rationale` (string)
  - `proof_point_rationale` (string)
  - `pillar_rationale` (string)
- `output_brief` object (Content Engine format):
  - `product_id`, `product_line`, `audience_segments`, `channels`, `campaign_stage`, `primary_message`, `secondary_messages`, `pricing`, `proof_points`, `tone_override`, `cta`, `personalization_variables`

Here is ALL the pre-generated content for each brief:

---

**Brief 1: `slab_acquisition`**

```json
{
  "id": "slab_acquisition",
  "name": "Slab 35QT Cooler Acquisition",
  "description": "New customer acquisition in the cooler market — competitive positioning against premium brands",
  "product_id": "slab_35qt",
  "campaign_objective": "acquisition",
  "research": {
    "competitive_landscape": [
      {
        "name": "YETI Tundra 35",
        "price_range": "$300–$350",
        "positioning": "Lifestyle brand with premium positioning. The cooler-as-status-symbol. Heavy investment in sponsorships, influencer marketing, and aspirational outdoor imagery.",
        "strengths": ["Dominant brand recognition", "Perceived quality leadership", "Strong retail presence", "Resale value holds"],
        "weaknesses": ["Price premium driven by brand, not materials", "Celebrity/influencer marketing alienates practical buyers", "Identical rotomolded construction to competitors at lower price points", "Logo is the product for many buyers"]
      },
      {
        "name": "Stanley (Adventure Quencher / lifestyle line)",
        "price_range": "$35–$60 (drinkware) / $150–$250 (hard coolers)",
        "positioning": "Viral social media momentum. Fashion-adjacent positioning driven by color drops and influencer partnerships. Originally a workwear brand, now lifestyle.",
        "strengths": ["Massive social media presence", "Color/limited edition strategy drives urgency", "Strong with younger demographics", "Retail distribution everywhere"],
        "weaknesses": ["Trend-dependent — viral momentum is fragile", "Build quality inconsistent across product line", "Cooler line is secondary to drinkware", "Brand identity increasingly disconnected from outdoor performance"]
      },
      {
        "name": "RTIC 35 QT",
        "price_range": "$170–$200",
        "positioning": "The 'same as YETI for less' value play. Openly positions as an alternative to premium brands. Direct-to-consumer pricing.",
        "strengths": ["Strong value proposition", "Similar construction to YETI at lower price", "Direct-to-consumer keeps costs down", "Loyal price-conscious following"],
        "weaknesses": ["Perceived as knockoff/copycat brand", "Lawsuit history with YETI reinforces copycat image", "Less retail presence — mostly online", "Brand carries no independent identity beyond 'cheaper YETI'"]
      },
      {
        "name": "Igloo BMX 25",
        "price_range": "$40–$60",
        "positioning": "Budget/mass market. The cooler most people buy at Walmart or Target. Not built to last, but built to a price point.",
        "strengths": ["Extremely low price", "Available everywhere", "Good enough for casual use", "No purchase anxiety — disposable price"],
        "weaknesses": ["Breaks within 1-3 seasons", "Latches, hinges, and drain plugs fail", "Insulation degrades quickly", "Becomes a recurring cost — replace every 2 years"]
      }
    ],
    "audience_analysis": {
      "profile": "Casual outdoor buyers, 30-55, household income $60K-$120K. They camp, fish, tailgate, and host backyard events. They've used cheap coolers and gotten tired of replacing them. They're willing to pay for quality but resent paying for a logo. Shorter research cycles than backcountry buyers — more influenced by recommendations, reviews, and price comparisons.",
      "pain_points": [
        "Cheap coolers break after 1-2 seasons — latches crack, lids warp, insulation fails",
        "Premium coolers feel overpriced — 'am I paying for the cooler or the sticker?'",
        "Hard to tell which mid-range coolers are actually good vs. which are just marketed well",
        "Ice retention claims are often exaggerated — '5-day ice retention' in lab conditions, not real life"
      ],
      "buying_triggers": [
        "Current cooler just broke or is showing signs of failure",
        "Friend or family member recommends a specific brand",
        "Price comparison reveals a quality option at a lower price than expected",
        "Seeing a factual, no-hype product comparison (specs, not lifestyle)"
      ],
      "media_habits": [
        "Facebook groups for camping, fishing, and outdoor cooking",
        "YouTube cooler comparison and review videos",
        "Amazon reviews — reads 1-star and 3-star reviews for real feedback",
        "Reddit threads (r/CampingGear, r/Coolers) for unfiltered opinions"
      ]
    },
    "seasonal_context": "Cooler purchases peak April through July as camping and outdoor season begins. Secondary peak in September for football tailgating season. Marketing should launch in early spring to capture pre-season research and purchase decisions.",
    "positioning_angle": "Yowie slots into the gap between RTIC's 'cheaper alternative' positioning and YETI's build quality. Same rotomolded construction and insulation class as YETI. No lifestyle tax, no celebrity endorsements, no limited-edition color drops. But unlike RTIC, Yowie doesn't position as a knockoff — it positions as a company that skipped the marketing budget and put the money into the cooler. The $285 price point is below YETI ($300-350) and above RTIC ($200), justified by materials and construction, not brand."
  },
  "reasoning": {
    "message_rationale": "Research shows the casual buyer's core tension: they want YETI quality but resent YETI's price premium. RTIC addresses this but carries a 'knockoff' stigma. The primary message 'Same quality as the $400 coolers. No logo tax.' directly targets this tension — it validates the buyer's skepticism of premium pricing while positioning Yowie as a quality-first alternative, not a copycat.",
    "channel_rationale": "Audience media habits center on social media (Facebook groups, YouTube), review platforms (Amazon, Reddit), and peer recommendations. Social media is the discovery channel — short, punchy, price-forward content. Email is the nurture channel for people who've shown interest. Landing page is the close — the full argument with specs, comparisons, and price. This matches the acquisition funnel: awareness (social) → consideration (email) → conversion (landing page).",
    "proof_point_rationale": "The four proof points selected are all verifiable and address specific audience pain points: '5-day ice retention' counters the skepticism about inflated claims (it's a measured result). 'Rotomolded — one piece, no seams' addresses the durability failures they've experienced with cheap coolers. 'Bear-resistant certified' is a third-party certification, not a marketing claim. '$285' is the price — stated as a fact, not a discount pitch.",
    "pillar_rationale": "'Pay for the gear, not the logo' is the primary pillar because the competitive analysis reveals that brand tax is the #1 objection for this audience. They've seen YETI's marketing machine. They've seen RTIC position itself as 'cheaper YETI.' Yowie's angle is neither — it's 'we skipped the marketing and put the money into the cooler.' This pillar directly addresses the audience's price-vs-quality tension and differentiates from both premium and knockoff positioning."
  },
  "output_brief": {
    "product_id": "slab_35qt",
    "product_line": "coolers",
    "audience_segments": ["casual_car_camping"],
    "channels": ["social_media", "email", "landing_page"],
    "campaign_stage": "acquisition",
    "primary_message": "Same quality as the $400 coolers. No logo tax.",
    "secondary_messages": ["You can stand on it, sit on it, drag it across a parking lot", "Rotomolded — one piece, no seams, nothing to crack or leak"],
    "pricing": "$285",
    "proof_points": ["5-day ice retention in 90°F heat", "Rotomolded polyethylene — one piece, no seams", "Bear-resistant certified (IGBC)", "$285"],
    "tone_override": null,
    "cta": "Compare for yourself",
    "personalization_variables": []
  }
}
```

---

**Brief 2: `lowline_cold_weather`**

```json
{
  "id": "lowline_cold_weather",
  "name": "Lowline 2P Cold-Weather Launch",
  "description": "Product launch targeting cold-weather specialists — the most spec-literate audience",
  "product_id": "lowline_2p",
  "campaign_objective": "launch",
  "research": {
    "competitive_landscape": [
      {
        "name": "MSR Access Series",
        "price_range": "$500–$700",
        "positioning": "Ski-touring and backcountry skiing focus. Four-season capable with snow-load engineering. Premium price justified by specialized use case.",
        "strengths": ["Strong reputation in ski-touring community", "Four-season ratings", "Snow-load engineering for winter conditions", "MSR brand trust in stove/water filter buyers extends to tents"],
        "weaknesses": ["Price premium for four-season capability many three-season buyers don't need", "Heavier than three-season alternatives", "Ski-touring focus means some features are irrelevant for general cold-weather camping", "Limited two-person options in the lineup"]
      },
      {
        "name": "Hilleberg Anjan 2",
        "price_range": "$700–$850",
        "positioning": "Expedition-grade Scandinavian engineering. The 'buy once, use forever' tent. Positioned as the luxury choice for serious conditions.",
        "strengths": ["Legendary build quality and durability", "Excellent wind performance", "Strong following among expedition and cold-weather communities", "Kerlon fabric is proprietary and well-regarded"],
        "weaknesses": ["Price is 50-80% higher than competitors", "Heavy for what it is — durability comes at a weight cost", "Brand premium similar to YETI in the cooler market", "Overkill for three-season cold-weather use"]
      },
      {
        "name": "Big Agnes Copper Spur HV UL2",
        "price_range": "$400–$450",
        "positioning": "Ultralight priority. Designed for weight-conscious hikers who want minimal pack weight. Popular on thru-hiking trails.",
        "strengths": ["Very light (under 3 lbs)", "Large interior volume for its weight", "Popular and well-reviewed in ultralight community", "Good availability and retail presence"],
        "weaknesses": ["Wind performance is mediocre — tall profile catches gusts", "Fabric durability concerns at low denier counts", "Not rated for sustained harsh weather", "Ultralight compromises show in conditions above treeline"]
      },
      {
        "name": "REI Co-op Half Dome 2 Plus",
        "price_range": "$250–$350",
        "positioning": "Generalist. Good-enough tent for a broad audience. REI house brand with a value proposition.",
        "strengths": ["Affordable entry point to quality tents", "Good feature set for the price", "Easy to purchase (REI stores everywhere)", "Solid reviews from casual to moderate users"],
        "weaknesses": ["Not designed for harsh conditions — struggles in sustained wind", "Generic design doesn't optimize for any specific use case", "Heavier than competitors at similar price points", "Materials are adequate, not premium"]
      }
    ],
    "audience_analysis": {
      "profile": "Cold-weather specialists, 30-50. Overlaps with backcountry hiker profile but with specific conditions focus. Camp in snow, at altitude, and in shoulder seasons. Most spec-literate audience segment. Higher gear spend per year. Trust independent lab testing over marketing. Will pay premium if specs justify it, but resent paying for brand.",
      "pain_points": [
        "Most tents are designed for fair weather and marketed as 'all-season' — they fail in real wind",
        "Ultralight tents sacrifice weather performance for weight savings — dangerous above treeline",
        "Premium four-season tents are overkill and overpriced for three-season cold-weather use",
        "Hard to find tents designed specifically for wind resistance without paying expedition prices"
      ],
      "buying_triggers": [
        "Tent failure in the field — a collapsed tent in wind is a safety issue, not an inconvenience",
        "Specific trip planning for exposed or above-treeline camping",
        "Independent review or lab test data showing wind performance",
        "Spec comparison that shows a clear engineering advantage at a fair price"
      ],
      "media_habits": [
        "Gear review sites with independent testing (OutdoorGearLab, Switchback Travel)",
        "Forum discussions on specific conditions (Backpacking Light, r/WildernessBackpacking)",
        "Trip reports mentioning specific gear performance in specific conditions",
        "Manufacturer spec sheets — this audience reads the actual data, not the marketing copy"
      ]
    },
    "seasonal_context": "Cold-weather tent purchases cluster in two windows: August-September (pre-season planning for fall/winter trips) and January-February (post-holiday gear purchases and spring trip planning). Product launch should target late August to capture the first wave of pre-season researchers.",
    "positioning_angle": "The Lowline 2P fills a gap no competitor addresses directly: a three-season tent purpose-built for wind resistance at a fair price. MSR and Hilleberg over-build for four-season and charge accordingly ($500-850). Big Agnes under-builds for weight savings and suffers in wind. REI offers a generalist tent that doesn't optimize for anything. The Lowline's 40-inch peak height isn't a compromise — it's the design. Low profile means lower wind load. This is an engineering choice that competitors either don't make (they optimize for headroom) or charge expedition prices for. At $475, the Lowline delivers purpose-built wind performance without the expedition price tag."
  },
  "reasoning": {
    "message_rationale": "Cold-weather specialists have experienced tent failures in wind. Their pain point is safety-critical, not just inconvenient. The primary message 'Built low and tight because ridgeline wind doesn't care about your headroom' directly explains the engineering decision in language this audience respects — it states the design rationale as a fact, not a selling point. The low profile is the feature, not a limitation.",
    "channel_rationale": "This audience does deep research before purchasing. Product page is the primary destination — they'll read every spec, every material detail, every dimension. Email reaches existing subscribers who already trust Yowie's approach. Social media is secondary but effective for single-fact posts that link to the full spec page. This audience doesn't impulse-buy from social — they research from social.",
    "proof_point_rationale": "Four proof points selected for verifiability and relevance to wind performance: '3000mm waterproof coating' is a measurable, comparable spec. 'DAC Featherlite poles' names the specific pole system — this audience knows DAC and respects the choice. '4 lbs 8 oz' states the weight honestly (it's not ultralight, and this audience respects the honesty). 'Two vestibules' addresses the practical need for gear storage in wet/snowy conditions.",
    "pillar_rationale": "'Built for the job' leads because the entire positioning angle is about purpose-built design. The 40-inch profile exists because of the job (wind resistance), not despite it. This audience evaluates gear by asking 'what is this designed to do?' — the 'built for the job' pillar answers that question directly. 'Quality you verify' is the secondary pillar — the specs are verifiable, and this audience will verify them."
  },
  "output_brief": {
    "product_id": "lowline_2p",
    "product_line": "tents",
    "audience_segments": ["cold_weather"],
    "channels": ["product_page", "email", "social_media"],
    "campaign_stage": "launch",
    "primary_message": "Built low and tight because ridgeline wind doesn't care about your headroom",
    "secondary_messages": ["Freestanding pitch — stakes optional on hard ground", "Recycled polyester fly fabric"],
    "pricing": "$475",
    "proof_points": ["3000mm waterproof coating", "DAC Featherlite poles", "4 lbs 8 oz packed", "Two vestibules"],
    "tone_override": null,
    "cta": "See full specs",
    "personalization_variables": []
  }
}
```

---

**Brief 3: `foldout_cross_sell`**

```json
{
  "id": "foldout_cross_sell",
  "name": "Foldout Soft Cooler Cross-Sell",
  "description": "Cross-pollination campaign — introduce the Foldout to existing Slab 35QT buyers",
  "product_id": "foldout",
  "campaign_objective": "cross-sell",
  "research": {
    "competitive_landscape": [
      {
        "name": "YETI Hopper Flip 18",
        "price_range": "$250–$350",
        "positioning": "Premium soft cooler. Same brand premium and lifestyle positioning as the YETI hard cooler line. The soft cooler for people who already own a YETI Tundra.",
        "strengths": ["YETI brand recognition carries over from hard coolers", "DryHide shell is genuinely durable", "Magnetic closure is well-engineered", "Strong resale market"],
        "weaknesses": ["$250-350 for a soft cooler is extreme — nearly the price of Yowie's hard cooler", "Same lifestyle tax as the hard cooler line", "Ice retention is good but not dramatically better than $100-150 competitors", "Heavy for a soft cooler (5+ lbs)"]
      },
      {
        "name": "Hydro Flask Day Escape",
        "price_range": "$125–$175",
        "positioning": "Lifestyle-adjacent. Appeals to the same demographics as Hydro Flask bottles — younger, style-conscious, outdoor-casual. Color options and clean design.",
        "strengths": ["Attractive design and color options", "Hydro Flask brand recognition from drinkware", "Reasonable price point", "Good retail availability"],
        "weaknesses": ["Style over substance — insulation is adequate, not exceptional", "Zipper closure less reliable than magnetic over time", "Brand is about aesthetics, not durability", "Limited size options"]
      },
      {
        "name": "Coleman Soft Coolers",
        "price_range": "$25–$40",
        "positioning": "Disposable. The soft cooler you buy at Walmart because you need one today. Not built to last, priced to replace.",
        "strengths": ["Extremely cheap", "Available everywhere immediately", "No purchase anxiety", "Good enough for a single trip or event"],
        "weaknesses": ["Zippers fail within months", "Liners leak after minimal use", "Insulation is negligible — ice lasts hours, not a full day", "Shoulder straps break, seams separate", "Becomes a recurring cost — replace every season"]
      },
      {
        "name": "Engel HD30",
        "price_range": "$110–$140",
        "positioning": "Marine and fishing focus. Built for boats, docks, and saltwater environments. Practical over pretty.",
        "strengths": ["Genuinely tough — built for marine use", "Welded seams, no leak points", "Good ice retention for the category (24+ hours)", "Respected in fishing and marine communities"],
        "weaknesses": ["Aesthetic is purely functional — looks like a marine cooler", "Limited to fishing/marine retail channels", "Not positioned for general outdoor use", "Brand unknown outside marine/fishing"]
      }
    ],
    "audience_analysis": {
      "profile": "Existing Slab 35QT buyers. They already own and trust a Yowie product. They bought the Slab for camping, tailgating, or fishing. They chose Yowie over YETI and RTIC — meaning they value quality without brand tax. They're receptive to a second Yowie product if it solves a different problem than the Slab.",
      "pain_points": [
        "The Slab is great for camping but overkill for day trips, grocery runs, and picnics",
        "They probably own a cheap soft cooler that's already leaking or has a broken zipper",
        "Carrying a 22-lb hard cooler to the beach or a park is impractical",
        "They want the same build quality they got from the Slab in a smaller, lighter format"
      ],
      "buying_triggers": [
        "Their cheap soft cooler just failed — and they remember why they bought the Slab",
        "Email from Yowie introducing a complementary product they hadn't considered",
        "Realizing the price ($145) is less than half what they paid for the Slab — low-friction purchase",
        "Seeing that the Foldout uses the same quality-first philosophy they already trust"
      ],
      "media_habits": [
        "Yowie email list — they're already subscribers from the Slab purchase",
        "May follow Yowie on social media",
        "Less research-intensive than the original Slab purchase — trust is already established",
        "Word-of-mouth to friends who saw their Slab and asked about the brand"
      ]
    },
    "seasonal_context": "Cross-sell timing should follow 2-4 weeks after Slab purchase for new buyers, or align with spring/summer season for existing buyers. The Foldout's use cases (day trips, groceries, beach, park) peak in warm months. Email cross-sell sequences should launch in April-May.",
    "positioning_angle": "This is not a competitive positioning campaign — the buyer already chose Yowie. This is a complementary product introduction. The angle is: 'You bought the Slab for camping. The Foldout is the Yowie cooler for everything else.' Position the Foldout as the everyday counterpart to the Slab — not a replacement, not a lesser product, but the right tool for a different job. The $145 price point is less than half the Slab, making it a low-friction add-on purchase. Lean into the Phase 3 strategy: cooler buyer → broader Yowie catalog."
  },
  "reasoning": {
    "message_rationale": "The primary message 'You bought the Slab for camping. This is the cooler for everything else.' directly references the customer's existing purchase and positions the Foldout as a complement, not a competitor. This resonates because the buyer already trusts Yowie's quality-first philosophy — they don't need to be re-sold on the brand. They need to see that the Foldout applies the same philosophy to a different use case.",
    "channel_rationale": "Email is the primary channel because these are existing customers on the Yowie email list. The cross-sell email can reference their Slab purchase directly (personalization). Social media is secondary — it reinforces the message for customers who follow Yowie but may not open every email. No landing page needed — these buyers already know how to navigate the Yowie site. The product page is their destination.",
    "proof_point_rationale": "Proof points are selected for the 'different job, same quality' positioning: 'Welded leak-proof liner' addresses the #1 failure mode of cheap soft coolers (leaking). '24-hour ice retention' sets honest expectations — not competing with the Slab's 5-day retention, but honest about what a soft cooler does. 'Folds flat to 3 inches' is the key differentiator from hard coolers — it solves the storage problem. '$145' is the price stated as a fact — at less than half the Slab, it's a low-friction purchase.",
    "pillar_rationale": "'Built for the job' leads because the entire cross-sell angle is about the right tool for the right job. The Slab is built for camping. The Foldout is built for everything else. This pillar also connects to the buyer's existing understanding of Yowie — they bought the Slab because it was built for a specific job. The Foldout extends that same principle to a different use case."
  },
  "output_brief": {
    "product_id": "foldout",
    "product_line": "coolers",
    "audience_segments": ["casual_car_camping"],
    "channels": ["email", "social_media"],
    "campaign_stage": "retention",
    "primary_message": "You bought the Slab for camping. This is the cooler for everything else.",
    "secondary_messages": ["Folds flat to 3 inches — stores anywhere", "Same build philosophy as the Slab, built for a different job"],
    "pricing": "$145",
    "proof_points": ["Welded leak-proof liner", "24-hour ice retention", "Folds flat to 3 inches", "$145"],
    "tone_override": null,
    "cta": "Add the Foldout",
    "personalization_variables": ["slab_purchase_date", "customer_first_name"]
  }
}
```

---

The complete HTML file skeleton should look like:

```html
<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <title>Yowie Research & Brief Builder</title>
  <style>/* styles added in Task 2 */</style>
</head>
<body>
  <!-- markup added in Task 3 -->
  <script>
    const BUILDER_DATA = { pipeline: {...}, briefs: [...] };
    // JS added in Task 4
  </script>
</body>
</html>
```

- [ ] **Step 2: Verify the file was created and the JSON is valid**

Run: `cd "/Users/gregappler/Claude Code Projects/demo-marketing-ecosystem" && node -e "const fs = require('fs'); const html = fs.readFileSync('tools/brief_builder.html','utf8'); const match = html.match(/const BUILDER_DATA = ({[\s\S]*?});/); try { const data = JSON.parse(match[1]); console.log('Pipeline stages:', data.pipeline.stages.length); console.log('Briefs:', data.briefs.length); data.briefs.forEach(b => console.log(' ', b.id, '- competitors:', b.research.competitive_landscape.length, '- output_brief channels:', b.output_brief.channels.length)); console.log('VALID'); } catch(e) { console.error('INVALID:', e.message); }"`

Expected:
```
Pipeline stages: 5
Briefs: 3
  slab_acquisition - competitors: 4 - output_brief channels: 3
  lowline_cold_weather - competitors: 4 - output_brief channels: 3
  foldout_cross_sell - competitors: 4 - output_brief channels: 2
VALID
```

- [ ] **Step 3: Commit**

```bash
cd "/Users/gregappler/Claude Code Projects/demo-marketing-ecosystem"
git add tools/brief_builder.html
git commit -m "Add brief builder with embedded research data and pre-generated briefs

Co-Authored-By: Claude Opus 4.6 (1M context) <noreply@anthropic.com>"
```

---

### Task 2: Build the CSS styles

**Files:**
- Modify: `tools/brief_builder.html` (replace `<style>/* styles added in Task 2 */</style>` with complete CSS)

- [ ] **Step 1: Add all CSS styles**

Replace the style placeholder with complete CSS. The styles match the existing tools' dark theme.

**CSS variables** (identical to content_engine.html):
```css
:root {
  --bg-primary: #0a0a1a;
  --bg-secondary: #12122a;
  --bg-selector: #16213e;
  --text-primary: #ccd6f6;
  --text-secondary: #8892b0;
  --accent-gold: #e2c87d;
  --accent-teal: #64ffda;
  --accent-red: #ff6b6b;
  --border: rgba(255,255,255,0.08);
}
```

**Reset & body**: Same as content_engine.html — box-sizing border-box, system font stack, bg-primary, text-primary, line-height 1.6.

**Pipeline diagram:**
```css
.pipeline {
  background: var(--bg-secondary);
  border-bottom: 1px solid var(--border);
  padding: 16px 24px 12px;
  position: sticky;
  top: 0;
  z-index: 100;
}
.pipeline-stages {
  display: flex;
  align-items: center;
  justify-content: center;
  gap: 0;
  max-width: 960px;
  margin: 0 auto;
}
.pipeline-stage {
  flex: 1;
  text-align: center;
  cursor: pointer;
  padding: 10px 12px;
  border-radius: 8px;
  border: 1px solid transparent;
  transition: all 0.2s ease;
}
.pipeline-stage.teal {
  background: rgba(100,255,218,0.06);
  border-color: rgba(100,255,218,0.2);
}
.pipeline-stage.teal:hover {
  background: rgba(100,255,218,0.12);
  border-color: rgba(100,255,218,0.4);
}
.pipeline-stage.gold {
  background: rgba(226,200,125,0.08);
  border-color: rgba(226,200,125,0.35);
  border-width: 2px;
}
.pipeline-stage.gold:hover {
  background: rgba(226,200,125,0.15);
  border-color: rgba(226,200,125,0.5);
}
.pipeline-stage.active {
  /* active state when scrolled to that section */
}
.pipeline-stage.teal.active {
  background: rgba(100,255,218,0.15);
  border-color: rgba(100,255,218,0.5);
}
.pipeline-stage.gold.active {
  background: rgba(226,200,125,0.18);
  border-color: rgba(226,200,125,0.6);
}
.stage-label {
  font-size: 0.65rem;
  text-transform: uppercase;
  letter-spacing: 0.1em;
  font-weight: 600;
}
.pipeline-stage.teal .stage-label { color: var(--accent-teal); }
.pipeline-stage.gold .stage-label { color: var(--accent-gold); }
.stage-subtitle {
  font-size: 0.7rem;
  color: var(--text-secondary);
  margin-top: 2px;
}
.pipeline-arrow {
  color: var(--text-secondary);
  font-size: 1.2rem;
  margin: 0 -4px;
  flex-shrink: 0;
}
.pipeline-tagline {
  text-align: center;
  font-size: 0.7rem;
  color: var(--text-secondary);
  font-style: italic;
  margin-top: 8px;
}
```

**Layout — sidebar + main (below pipeline):**
```css
.layout {
  display: flex;
  min-height: calc(100vh - 100px);
}
.sidebar {
  width: 280px;
  flex-shrink: 0;
  background: var(--bg-secondary);
  border-right: 1px solid var(--border);
  padding: 24px;
  position: fixed;
  top: 100px; /* below pipeline */
  left: 0;
  bottom: 0;
  overflow-y: auto;
}
.main {
  margin-left: 280px;
  flex: 1;
  padding: 32px 40px 64px;
  max-width: 900px;
}
```

**Sidebar elements** — same pattern as content_engine.html:
- `.sidebar-header h1`: 1.2rem, accent-gold, uppercase, letter-spacing
- `.sidebar-header p`: 0.8rem, text-secondary
- `.section-label`: 0.7rem, uppercase, letter-spacing, text-secondary, font-weight 600, margin-bottom 12px
- `.brief-card`: bg-selector background, border, rounded 8px, padding 16px, cursor pointer, margin-bottom 12px, border-left 3px solid transparent, transition
- `.brief-card.active`: border-left-color accent-gold, brighter background
- `.brief-card h3`: 0.9rem, text-primary, font-weight 500
- `.brief-meta`: 0.75rem, text-secondary, flex, align-items center, gap 8px
- `.objective-badge`: inline pill, 0.65rem, uppercase, letter-spacing, rounded 12px, padding 2px 10px, font-weight 600
- `.objective-acquisition`: rgba(255,107,107,0.15) background, accent-red color
- `.objective-launch`: rgba(100,255,218,0.15) background, accent-teal color
- `.objective-cross-sell`: rgba(179,136,255,0.15) background, #b388ff color
- `.sidebar-footer`: position absolute, bottom 24px, left 24px, right 24px, 0.7rem, text-secondary

**Main content sections:**
- `.content-section`: margin-bottom 40px, scroll-margin-top 120px (accounts for sticky pipeline)
- `.section-header`: font-size 1.1rem, color accent-gold, text-transform uppercase, letter-spacing 0.05em, font-weight 600, margin-bottom 20px, padding-bottom 10px, border-bottom 1px solid var(--border)
- `.section-highlight`: animation flash-highlight 1.5s ease-out (keyframe: 0% bg rgba(100,255,218,0.1), 100% bg transparent)

**Competitive grid:**
```css
.competitor-grid {
  display: grid;
  grid-template-columns: repeat(2, 1fr);
  gap: 16px;
  margin-bottom: 24px;
}
@media (max-width: 900px) {
  .competitor-grid { grid-template-columns: 1fr; }
}
.competitor-card {
  background: var(--bg-secondary);
  border: 1px solid var(--border);
  border-radius: 8px;
  padding: 20px;
}
.competitor-name {
  font-size: 0.9rem;
  color: var(--text-primary);
  font-weight: 600;
  margin-bottom: 4px;
}
.competitor-price {
  font-size: 0.8rem;
  color: var(--accent-gold);
  margin-bottom: 8px;
}
.competitor-positioning {
  font-size: 0.8rem;
  color: var(--text-secondary);
  margin-bottom: 12px;
  line-height: 1.5;
}
.competitor-detail-label {
  font-size: 0.7rem;
  text-transform: uppercase;
  letter-spacing: 0.05em;
  font-weight: 600;
  margin-bottom: 6px;
  margin-top: 10px;
}
.competitor-detail-label.strengths { color: var(--accent-teal); }
.competitor-detail-label.weaknesses { color: var(--accent-red); }
.competitor-list {
  list-style: none;
  padding: 0;
}
.competitor-list li {
  font-size: 0.78rem;
  color: var(--text-secondary);
  padding: 2px 0;
  line-height: 1.4;
}
.competitor-list li::before {
  margin-right: 6px;
}
.competitor-list.strengths li::before { content: "+"; color: var(--accent-teal); }
.competitor-list.weaknesses li::before { content: "−"; color: var(--accent-red); }
```

**Audience analysis:**
```css
.audience-card {
  background: var(--bg-secondary);
  border: 1px solid var(--border);
  border-radius: 8px;
  padding: 20px;
  margin-bottom: 16px;
}
.audience-profile {
  font-size: 0.85rem;
  color: var(--text-secondary);
  line-height: 1.6;
  margin-bottom: 16px;
}
.audience-grid {
  display: grid;
  grid-template-columns: repeat(2, 1fr);
  gap: 16px;
}
@media (max-width: 900px) {
  .audience-grid { grid-template-columns: 1fr; }
}
.audience-list-title {
  font-size: 0.7rem;
  text-transform: uppercase;
  letter-spacing: 0.05em;
  color: var(--accent-gold);
  font-weight: 600;
  margin-bottom: 8px;
}
.audience-list {
  list-style: none;
  padding: 0;
}
.audience-list li {
  font-size: 0.78rem;
  color: var(--text-secondary);
  padding: 3px 0;
  line-height: 1.4;
}
.audience-list li::before {
  content: "→ ";
  color: var(--accent-gold);
}
```

**Positioning angle callout:**
```css
.positioning-callout {
  background: rgba(100,255,218,0.06);
  border: 1px solid rgba(100,255,218,0.2);
  border-left: 3px solid var(--accent-teal);
  border-radius: 0 8px 8px 0;
  padding: 20px;
  margin-top: 20px;
}
.positioning-label {
  font-size: 0.7rem;
  text-transform: uppercase;
  letter-spacing: 0.1em;
  color: var(--accent-teal);
  font-weight: 600;
  margin-bottom: 8px;
}
.positioning-text {
  font-size: 0.85rem;
  color: var(--text-primary);
  line-height: 1.7;
}
```

**Seasonal context:**
```css
.seasonal-note {
  font-size: 0.82rem;
  color: var(--text-secondary);
  font-style: italic;
  padding: 12px 16px;
  background: var(--bg-secondary);
  border: 1px dashed var(--border);
  border-radius: 8px;
  margin-top: 16px;
  line-height: 1.5;
}
```

**Reasoning cards:**
```css
.reasoning-card {
  background: var(--bg-secondary);
  border: 1px solid var(--border);
  border-radius: 8px;
  margin-bottom: 12px;
  overflow: hidden;
}
.reasoning-header {
  display: flex;
  align-items: center;
  justify-content: space-between;
  padding: 14px 20px;
  cursor: pointer;
  user-select: none;
}
.reasoning-header:hover {
  background: rgba(255,255,255,0.02);
}
.reasoning-title {
  font-size: 0.82rem;
  color: var(--text-primary);
  font-weight: 500;
}
.reasoning-chevron {
  color: var(--text-secondary);
  font-size: 0.75rem;
  transition: transform 0.2s;
}
.reasoning-body {
  display: none;
  padding: 0 20px 16px;
}
.reasoning-card.expanded .reasoning-body {
  display: block;
}
.reasoning-text {
  font-size: 0.82rem;
  color: var(--text-secondary);
  line-height: 1.6;
}
```

**Draft brief display:**
```css
.brief-output {
  background: var(--bg-secondary);
  border: 1px solid var(--border);
  border-radius: 8px;
  padding: 24px;
  position: relative;
}
.brief-badge {
  position: absolute;
  top: 16px;
  right: 16px;
  font-size: 0.65rem;
  text-transform: uppercase;
  letter-spacing: 0.05em;
  padding: 3px 10px;
  border-radius: 12px;
  border: 1px solid rgba(100,255,218,0.3);
  color: var(--accent-teal);
  font-weight: 600;
}
.brief-fields {
  display: grid;
  grid-template-columns: 1fr 1fr;
  gap: 14px 24px;
}
.brief-field {
  display: flex;
  flex-direction: column;
}
.brief-field-label {
  font-size: 0.7rem;
  text-transform: uppercase;
  letter-spacing: 0.05em;
  color: var(--text-secondary);
  font-weight: 600;
  margin-bottom: 4px;
}
.brief-field-value {
  font-size: 0.82rem;
  color: var(--text-primary);
  line-height: 1.4;
}
.brief-field-value.mono {
  font-family: 'SF Mono', 'Fira Code', 'Consolas', monospace;
  font-size: 0.8rem;
}
.brief-field.full-width {
  grid-column: 1 / -1;
}
```

**Human review section:**
```css
.review-card {
  background: rgba(226,200,125,0.06);
  border: 2px solid rgba(226,200,125,0.25);
  border-radius: 8px;
  padding: 24px;
}
.review-checklist {
  list-style: none;
  padding: 0;
}
.review-checklist li {
  font-size: 0.82rem;
  color: var(--text-secondary);
  padding: 6px 0;
  display: flex;
  align-items: flex-start;
  gap: 10px;
  line-height: 1.4;
}
.review-checkbox {
  width: 16px;
  height: 16px;
  border: 1px solid rgba(226,200,125,0.4);
  border-radius: 3px;
  flex-shrink: 0;
  margin-top: 2px;
}
.review-note {
  font-size: 0.78rem;
  color: var(--text-secondary);
  font-style: italic;
  margin-top: 16px;
  padding-top: 12px;
  border-top: 1px solid var(--border);
  line-height: 1.5;
}
```

**Highlight animation:**
```css
@keyframes flash-highlight {
  0% { background-color: rgba(100,255,218,0.08); }
  100% { background-color: transparent; }
}
.section-highlight {
  animation: flash-highlight 1.5s ease-out;
}
```

- [ ] **Step 2: Verify styles render**

Run: `open "/Users/gregappler/Claude Code Projects/demo-marketing-ecosystem/tools/brief_builder.html"`

- [ ] **Step 3: Commit**

```bash
cd "/Users/gregappler/Claude Code Projects/demo-marketing-ecosystem"
git add tools/brief_builder.html
git commit -m "Add brief builder CSS styles matching dark theme

Co-Authored-By: Claude Opus 4.6 (1M context) <noreply@anthropic.com>"
```

---

### Task 3: Build the HTML markup

**Files:**
- Modify: `tools/brief_builder.html` (replace `<!-- markup added in Task 3 -->` with complete HTML)

- [ ] **Step 1: Add all HTML markup**

Replace the markup placeholder with the complete HTML structure inside `<body>`, BEFORE the `<script>` tag:

```html
<!-- Pipeline Diagram -->
<div class="pipeline">
  <div class="pipeline-stages" id="pipeline-stages">
    <!-- JS renders pipeline stages here -->
  </div>
  <div class="pipeline-tagline" id="pipeline-tagline"></div>
</div>

<div class="layout">
  <!-- Sidebar -->
  <aside class="sidebar">
    <div class="sidebar-header">
      <h1>Brief Builder</h1>
      <p>Research to brief pipeline</p>
    </div>

    <div class="sidebar-section">
      <div class="section-label">Research Briefs</div>
      <div id="brief-list">
        <!-- JS renders brief cards here -->
      </div>
    </div>

    <div class="sidebar-footer">
      <p>Research and briefs generated by Claude Code from the Brand Configuration Layer.</p>
    </div>
  </aside>

  <!-- Main content -->
  <main class="main">
    <div id="section-research" class="content-section">
      <div class="section-header">Research & Analysis</div>
      <div id="research-content">
        <!-- JS renders research content here -->
      </div>
    </div>

    <div id="section-reasoning" class="content-section">
      <div class="section-header">Reasoning</div>
      <div id="reasoning-content">
        <!-- JS renders reasoning cards here -->
      </div>
    </div>

    <div id="section-brief" class="content-section">
      <div class="section-header">Draft Brief</div>
      <div id="brief-content">
        <!-- JS renders brief output here -->
      </div>
    </div>

    <div id="section-review" class="content-section">
      <div class="section-header">Human Review</div>
      <div id="review-content">
        <!-- JS renders review checklist here -->
      </div>
    </div>
  </main>
</div>
```

- [ ] **Step 2: Verify markup structure**

Open in browser — pipeline area at top, sidebar with header text, main area with 4 section headers visible.

Run: `open "/Users/gregappler/Claude Code Projects/demo-marketing-ecosystem/tools/brief_builder.html"`

- [ ] **Step 3: Commit**

```bash
cd "/Users/gregappler/Claude Code Projects/demo-marketing-ecosystem"
git add tools/brief_builder.html
git commit -m "Add brief builder HTML markup structure

Co-Authored-By: Claude Opus 4.6 (1M context) <noreply@anthropic.com>"
```

---

### Task 4: Build the JavaScript rendering logic

**Files:**
- Modify: `tools/brief_builder.html` (replace `// JS added in Task 4` with complete JavaScript)

- [ ] **Step 1: Add state management, initialization, and helper functions**

```javascript
var state = {
  activeBrief: BUILDER_DATA.briefs[0].id
};

function init() {
  renderPipeline();
  renderBriefList();
  renderContent();
}

document.addEventListener('DOMContentLoaded', init);

function formatChannelName(key) {
  var names = {
    product_page: 'Product Page',
    email: 'Email',
    social_media: 'Social Media',
    landing_page: 'Landing Page'
  };
  return names[key] || key;
}

function formatAudienceName(key) {
  var names = {
    backcountry_hikers: 'Backcountry Hikers',
    thru_hikers: 'Thru-Hikers',
    casual_car_camping: 'Casual / Car Camping',
    cold_weather: 'Cold-Weather Specialists'
  };
  return names[key] || key;
}
```

- [ ] **Step 2: Add pipeline rendering**

```javascript
function renderPipeline() {
  var container = document.getElementById('pipeline-stages');
  var html = '';
  BUILDER_DATA.pipeline.stages.forEach(function(stage, i) {
    if (i > 0) {
      html += '<div class="pipeline-arrow">→</div>';
    }
    var icon = stage.color === 'gold' ? '⚑ ' : '';
    html += '<div class="pipeline-stage ' + stage.color + '" data-stage="' + stage.id + '" onclick="scrollToStage(\'' + stage.id + '\')">';
    html += '<div class="stage-label">' + icon + stage.label + '</div>';
    html += '<div class="stage-subtitle">' + stage.subtitle + '</div>';
    html += '</div>';
  });
  container.innerHTML = html;
  document.getElementById('pipeline-tagline').textContent = BUILDER_DATA.pipeline.tagline;
}

function scrollToStage(stageId) {
  var stage = BUILDER_DATA.pipeline.stages.find(function(s) { return s.id === stageId; });
  if (!stage || !stage.scroll_to) return;
  var target = document.getElementById(stage.scroll_to);
  if (!target) return;
  target.scrollIntoView({ behavior: 'smooth', block: 'start' });
  target.classList.remove('section-highlight');
  void target.offsetWidth; // force reflow to restart animation
  target.classList.add('section-highlight');
  // Update active state on pipeline
  document.querySelectorAll('.pipeline-stage').forEach(function(el) {
    el.classList.remove('active');
  });
  document.querySelector('[data-stage="' + stageId + '"]').classList.add('active');
}
```

- [ ] **Step 3: Add sidebar rendering**

```javascript
function renderBriefList() {
  var container = document.getElementById('brief-list');
  container.innerHTML = BUILDER_DATA.briefs.map(function(brief) {
    return '<div class="brief-card ' + (brief.id === state.activeBrief ? 'active' : '') + '" onclick="selectBrief(\'' + brief.id + '\')">' +
      '<h3>' + brief.name + '</h3>' +
      '<div class="brief-meta">' +
        brief.product_id + ' <span class="objective-badge objective-' + brief.campaign_objective + '">' + brief.campaign_objective + '</span>' +
      '</div>' +
    '</div>';
  }).join('');
}

function selectBrief(briefId) {
  state.activeBrief = briefId;
  renderBriefList();
  renderContent();
  // Scroll to top of main content
  document.getElementById('section-research').scrollIntoView({ behavior: 'smooth', block: 'start' });
}
```

- [ ] **Step 4: Add research section rendering**

```javascript
function renderResearch(brief) {
  var html = '';

  // Competitive landscape
  html += '<h3 style="font-size:0.9rem; color:var(--text-primary); margin-bottom:16px;">Competitive Landscape</h3>';
  html += '<div class="competitor-grid">';
  brief.research.competitive_landscape.forEach(function(comp) {
    html += '<div class="competitor-card">';
    html += '<div class="competitor-name">' + comp.name + '</div>';
    html += '<div class="competitor-price">' + comp.price_range + '</div>';
    html += '<div class="competitor-positioning">' + comp.positioning + '</div>';
    html += '<div class="competitor-detail-label strengths">Strengths</div>';
    html += '<ul class="competitor-list strengths">';
    comp.strengths.forEach(function(s) { html += '<li>' + s + '</li>'; });
    html += '</ul>';
    html += '<div class="competitor-detail-label weaknesses">Weaknesses</div>';
    html += '<ul class="competitor-list weaknesses">';
    comp.weaknesses.forEach(function(w) { html += '<li>' + w + '</li>'; });
    html += '</ul>';
    html += '</div>';
  });
  html += '</div>';

  // Audience analysis
  html += '<h3 style="font-size:0.9rem; color:var(--text-primary); margin:24px 0 16px;">Audience Analysis</h3>';
  html += '<div class="audience-card">';
  html += '<div class="audience-profile">' + brief.research.audience_analysis.profile + '</div>';
  html += '<div class="audience-grid">';

  var audienceSections = [
    { title: 'Pain Points', items: brief.research.audience_analysis.pain_points },
    { title: 'Buying Triggers', items: brief.research.audience_analysis.buying_triggers },
    { title: 'Media Habits', items: brief.research.audience_analysis.media_habits }
  ];
  audienceSections.forEach(function(section) {
    html += '<div>';
    html += '<div class="audience-list-title">' + section.title + '</div>';
    html += '<ul class="audience-list">';
    section.items.forEach(function(item) { html += '<li>' + item + '</li>'; });
    html += '</ul>';
    html += '</div>';
  });

  html += '</div></div>';

  // Seasonal context
  html += '<div class="seasonal-note">' + brief.research.seasonal_context + '</div>';

  // Positioning angle
  html += '<div class="positioning-callout">';
  html += '<div class="positioning-label">Yowie Positioning Angle</div>';
  html += '<div class="positioning-text">' + brief.research.positioning_angle + '</div>';
  html += '</div>';

  return html;
}
```

- [ ] **Step 5: Add reasoning section rendering**

```javascript
function renderReasoning(brief) {
  var cards = [
    { id: 'message', title: 'Message Rationale', text: brief.reasoning.message_rationale },
    { id: 'channel', title: 'Channel Rationale', text: brief.reasoning.channel_rationale },
    { id: 'proof', title: 'Proof Point Rationale', text: brief.reasoning.proof_point_rationale },
    { id: 'pillar', title: 'Pillar Rationale', text: brief.reasoning.pillar_rationale }
  ];

  return cards.map(function(card) {
    return '<div class="reasoning-card" id="reasoning-' + card.id + '">' +
      '<div class="reasoning-header" onclick="toggleReasoning(\'reasoning-' + card.id + '\')">' +
        '<span class="reasoning-title">' + card.title + '</span>' +
        '<span class="reasoning-chevron">▸</span>' +
      '</div>' +
      '<div class="reasoning-body">' +
        '<div class="reasoning-text">' + card.text + '</div>' +
      '</div>' +
    '</div>';
  }).join('');
}

function toggleReasoning(id) {
  var card = document.getElementById(id);
  card.classList.toggle('expanded');
  var chevron = card.querySelector('.reasoning-chevron');
  chevron.textContent = card.classList.contains('expanded') ? '▾' : '▸';
}
```

- [ ] **Step 6: Add brief output rendering**

```javascript
function renderBriefOutput(brief) {
  var ob = brief.output_brief;
  var fields = [
    { label: 'Product', value: ob.product_id + ' (' + ob.product_line + ')', fullWidth: false },
    { label: 'Price', value: ob.pricing, fullWidth: false },
    { label: 'Audiences', value: ob.audience_segments.map(function(a) { return formatAudienceName(a); }).join(', '), fullWidth: false },
    { label: 'Channels', value: ob.channels.map(function(c) { return formatChannelName(c); }).join(', '), fullWidth: false },
    { label: 'Campaign Stage', value: ob.campaign_stage, fullWidth: false },
    { label: 'CTA', value: ob.cta, fullWidth: false },
    { label: 'Primary Message', value: ob.primary_message, fullWidth: true, mono: true },
    { label: 'Secondary Messages', value: ob.secondary_messages.join(' · '), fullWidth: true },
    { label: 'Proof Points', value: ob.proof_points.join(' · '), fullWidth: true }
  ];

  if (ob.personalization_variables && ob.personalization_variables.length > 0) {
    fields.push({ label: 'Personalization', value: ob.personalization_variables.join(', '), fullWidth: true });
  }

  if (ob.tone_override) {
    fields.push({ label: 'Tone Override', value: ob.tone_override, fullWidth: false });
  }

  var html = '<div class="brief-output">';
  html += '<span class="brief-badge">Content Engine Compatible</span>';
  html += '<div class="brief-fields">';
  fields.forEach(function(field) {
    var cls = 'brief-field' + (field.fullWidth ? ' full-width' : '');
    var valCls = 'brief-field-value' + (field.mono ? ' mono' : '');
    html += '<div class="' + cls + '">';
    html += '<span class="brief-field-label">' + field.label + '</span>';
    html += '<span class="' + valCls + '">' + field.value + '</span>';
    html += '</div>';
  });
  html += '</div></div>';

  return html;
}
```

- [ ] **Step 7: Add review section rendering and main renderContent function**

```javascript
function renderReview() {
  var reviewStage = BUILDER_DATA.pipeline.stages.find(function(s) { return s.id === 'human_review'; });
  var html = '<div class="review-card">';
  html += '<ul class="review-checklist">';
  reviewStage.checklist.forEach(function(item) {
    html += '<li><span class="review-checkbox"></span>' + item + '</li>';
  });
  html += '</ul>';
  html += '<div class="review-note">In production, this is where a marketing lead reviews the AI-generated brief before approving it for content generation.</div>';
  html += '</div>';
  return html;
}

function renderContent() {
  var brief = BUILDER_DATA.briefs.find(function(b) { return b.id === state.activeBrief; });
  document.getElementById('research-content').innerHTML = renderResearch(brief);
  document.getElementById('reasoning-content').innerHTML = renderReasoning(brief);
  document.getElementById('brief-content').innerHTML = renderBriefOutput(brief);
  document.getElementById('review-content').innerHTML = renderReview();
}
```

- [ ] **Step 8: Verify full functionality**

Open in browser and test:
1. Pipeline diagram shows 5 stages with arrows, human review in gold
2. Clicking pipeline stages scrolls to sections with highlight animation
3. All 3 brief cards appear in sidebar, first is active
4. Clicking a brief switches all content
5. Competitive grid shows 4 competitors with strengths/weaknesses
6. Audience analysis shows profile, pain points, triggers, media habits
7. Positioning angle is in a teal-bordered callout
8. Reasoning cards expand/collapse
9. Draft brief shows all fields with "Content Engine Compatible" badge
10. Human review shows checklist with checkboxes and note

Run: `open "/Users/gregappler/Claude Code Projects/demo-marketing-ecosystem/tools/brief_builder.html"`

- [ ] **Step 9: Commit**

```bash
cd "/Users/gregappler/Claude Code Projects/demo-marketing-ecosystem"
git add tools/brief_builder.html
git commit -m "Add brief builder JavaScript rendering and interaction logic

Co-Authored-By: Claude Opus 4.6 (1M context) <noreply@anthropic.com>"
```

---

### Task 5: Write process log and tutorial

**Files:**
- Modify: `docs/process_log.md`
- Create: `tutorials/04_brief_builder.md`

- [ ] **Step 1: Add process log entry**

Prepend a new entry to `docs/process_log.md` (newest first, after any YAML header but before existing entries):

```markdown
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
```

- [ ] **Step 2: Create tutorial**

Create `tutorials/04_brief_builder.md`:

```markdown
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
```

- [ ] **Step 3: Commit**

```bash
cd "/Users/gregappler/Claude Code Projects/demo-marketing-ecosystem"
git add docs/process_log.md tutorials/04_brief_builder.md
git commit -m "Add process log and tutorial for brief builder

Co-Authored-By: Claude Opus 4.6 (1M context) <noreply@anthropic.com>"
```
