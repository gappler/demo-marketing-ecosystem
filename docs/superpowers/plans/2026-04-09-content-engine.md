# Content Engine Implementation Plan

> **For agentic workers:** REQUIRED SUB-SKILL: Use superpowers:subagent-driven-development (recommended) or superpowers:executing-plans to implement this plan task-by-task. Steps use checkbox (`- [ ]`) syntax for tracking.

**Goal:** Build an interactive HTML tool that demonstrates Yowie's content generation pipeline — campaign brief in, on-brand content out — using pre-generated AI copy embedded as JSON.

**Architecture:** Single self-contained HTML file (`tools/content_engine.html`) with an embedded `CONTENT_DATA` JSON object containing 4 campaign briefs and all pre-generated copy variants. The UI uses a sidebar brief selector + output mode switcher pattern matching the existing brand_config_viewer.html dark theme.

**Tech Stack:** Vanilla HTML/CSS/JS, no dependencies. JSON data embedded inline.

---

## File Structure

- **Create:** `tools/content_engine.html` — the complete interactive tool (HTML + CSS + JS + embedded JSON data)

This is a single-file build. The JSON data block, styles, markup, and rendering logic all live in one file.

---

### Task 1: Build the embedded JSON data block with all 4 briefs and pre-generated content

**Files:**
- Create: `tools/content_engine.html` (initial file — JSON data block + HTML skeleton only)

This is the largest task. It creates the file and populates all the content data that the UI will render. The JSON structure has two sections: `briefs` (the 4 campaign brief definitions) and `generated` (all pre-generated copy keyed by brief ID).

- [ ] **Step 1: Create the HTML file with the embedded CONTENT_DATA JSON**

Create `tools/content_engine.html` with the full JSON data block inside a `<script>` tag. The JSON contains:

**`briefs`** array — 4 objects with these fields:
- `id` (string): `lowline_launch`, `basecamp_4p_cross`, `slab_acquisition`, `burrow_seasonal`
- `name` (string): display name
- `description` (string): one-line description
- `product_id` (string): matches key in brand_config.json products
- `product_line` (string): backpacks/tents/sleeping_bags/coolers
- `audience_segments` (string[]): audience keys from brand_config.json
- `channels` (string[]): channel keys — `product_page`, `email`, `social_media`, `landing_page`
- `campaign_stage` (string): launch/education/acquisition/seasonal
- `primary_message` (string)
- `secondary_messages` (string[])
- `pricing` (string): e.g. "$475"
- `proof_points` (string[])
- `tone_override` (string|null)
- `cta` (string or object with audience-keyed CTAs)
- `personalization_variables` (string[])
- `default_mode` (string): `single`, `segments`, `channels`, `package`

**`generated`** object — keyed by brief ID. Each brief's generated content contains:

For **`lowline_launch`** (default mode: single):
- `single`: one featured piece — `backcountry_hikers` × `product_page`
- `segment_variants`: `product_page` channel with variants for `backcountry_hikers` and `cold_weather`, plus `what_changed` annotation
- `channel_variants`: `backcountry_hikers` audience across `email`, `social_media`, `product_page`, plus `what_changed` annotations
- `campaign_package`: brief summary + all 2 audiences × 3 channels = 6 content pieces + campaign metadata

For **`basecamp_4p_cross`** (default mode: segments):
- `single`: one featured piece — `backcountry_hikers` × `product_page`
- `segment_variants`: `product_page` channel with variants for `backcountry_hikers` and `casual_car_camping`, plus `what_changed` annotation
- `channel_variants`: `backcountry_hikers` audience across `product_page`, `email`, plus `what_changed` annotations
- `campaign_package`: all 2 audiences × 2 channels = 4 content pieces + campaign metadata

For **`slab_acquisition`** (default mode: channels):
- `single`: one featured piece — `casual_car_camping` × `social_media`
- `segment_variants`: `social_media` channel with variant for `casual_car_camping` (single audience — show note explaining only one segment)
- `channel_variants`: `casual_car_camping` across `social_media`, `email`, `landing_page`, plus `what_changed` annotations
- `campaign_package`: 1 audience × 3 channels = 3 content pieces + campaign metadata

For **`burrow_seasonal`** (default mode: package):
- `single`: one featured piece — `cold_weather` × `email`
- `segment_variants`: `email` channel with variants for `cold_weather` and `backcountry_hikers`, plus `what_changed` annotation
- `channel_variants`: `cold_weather` across `email`, `social_media`, `product_page`, plus `what_changed` annotations
- `campaign_package`: all 2 audiences × 3 channels = 6 content pieces + campaign metadata

Each content piece is an object with:
```json
{
  "audience": "backcountry_hikers",
  "channel": "product_page",
  "copy": "The actual generated copy text...",
  "rules_applied": {
    "voice": ["plainspoken", "confident", "restrained", "dry"],
    "channel_register": "Technical, direct",
    "audience_adaptation": "backcountry — assumed technical literacy, specs without translation"
  },
  "pillar": {
    "id": "built_for_job",
    "headline": "Built for the job, not the brand.",
    "resonance": "We respect the job you're doing and built gear that matches it."
  },
  "rationale": "1-2 sentence explanation of why this content was shaped this way."
}
```

Each `what_changed` annotation is:
```json
{
  "from": "backcountry_hikers",
  "to": "casual_car_camping",
  "summary": "Explanation of what changed and why."
}
```

Campaign package metadata:
```json
{
  "suggested_timing": "Description of send/post sequence",
  "content_hierarchy": "Which piece leads, which supports",
  "pillar_emphasis": "Which pillar drives this campaign"
}
```

**Now generate the actual copy.** For each content piece, read the brand_config.json to apply:
1. The correct voice rules (all 4 core attributes always apply)
2. The channel register and rules for the selected channel
3. The audience adaptation (backcountry or casual) with assumed knowledge and guidelines
4. The primary messaging pillar for this product × audience combination (from `messaging.product_emphasis` and `messaging.audience_emphasis` — use the highest-ranked pillar that appears in both lists)
5. The terminology rules (use approved words, avoid forbidden words, follow conditional rules)
6. The existing examples in brand_config.json as style references for the same product/audience/channel if available

**Copy generation guidelines:**
- Each piece should be 2-6 sentences depending on channel (social shortest, product page longest)
- Email copy: conversational, brief — 3-4 sentences max. Lead with product, name key benefit, point to details.
- Social copy: 1-2 sentences. One idea. Dry tone. Let a single fact carry.
- Product page copy: 4-6 sentences. Specs as facts, fragment sentences, technical register.
- Landing page copy (for Slab only): 4-6 sentences. Like product page but for casual audience — translate specs to outcomes.
- All copy must follow voice rules: no exclamation points, no superlatives without evidence, no forbidden words, short sentences, common words.
- Use the brief's primary_message as the core idea, proof_points as supporting details.
- Segment variants MUST show measurable differences: backcountry uses technical specs raw, casual translates to outcomes.
- Channel variants MUST show register shifts: social is shortest/driest, email is conversational, product page is spec-dense.

Here are the pre-generated copy pieces. Generate all content for all 4 briefs:

**Brief 1: Lowline 2P Launch**

Single (backcountry_hikers × product_page):
```
40-inch peak height. Two vestibules. DAC Featherlite poles. 3000mm waterproof coating on a recycled polyester fly. 4 lbs 8 oz packed. Built low and tight because the ridgeline wind that flattens tall tents treats this one like a rock. Sealed seams, bathtub floor, freestanding pitch.
```
Rules: Technical register, backcountry adaptation (specs without translation), pillar: built_for_job.
Rationale: "Product page for backcountry audience leads with specs stated as facts. Low-profile design rationale is stated plainly — function drives form. No lifestyle framing."

Segment variants (product_page):
- backcountry_hikers: same as single above
- cold_weather:
```
3000mm waterproof coating. DAC Featherlite poles. Sealed seams with bathtub floor. 40-inch peak height keeps the profile below wind shear. Two vestibules for gear storage in conditions where gear stays wet. 4 lbs 8 oz. Built for exposed camps where the weather is the reason you need a tent.
```
Rules: Technical register, backcountry adaptation (cold_weather uses backcountry voice), pillar: built_for_job.
Rationale: "Cold-weather audience gets the same technical density but with emphasis on weather performance and gear management in wet conditions. The closing line frames the tent as safety equipment, not recreation gear."
What changed: "Cold-weather variant shifts emphasis from structural specs to weather performance. '40-inch peak height' is reframed from a spec to a wind-shear advantage. Vestibules are positioned for wet-gear management, not just storage."

Channel variants (backcountry_hikers):
- product_page: same as single above
- email:
```
The Lowline 2P. Low profile, two vestibules, DAC Featherlite poles. 3000mm waterproof coating. 4 lbs 8 oz. Built for ridgelines and exposed camps where tall tents become kites. Full specs on the site.
```
- social_media:
```
40 inches tall. Handles ridgeline wind. The Lowline 2P — named for the thing that keeps it standing.
```
What changed (product_page → email): "Email register drops to conversational — fewer specs, key benefits only, points reader to full details. Same confidence, shorter format."
What changed (email → social_media): "Social register strips to one idea with dry humor. 'Named for the thing that keeps it standing' is a deadpan aside — the humor lives in the observation, not a punchline."

Campaign package metadata:
```json
{
  "suggested_timing": "Product page goes live first as the reference destination. Email to subscriber list same day. Social posts staggered over 3 days — one feature angle per post.",
  "content_hierarchy": "Product page is the anchor. Email drives traffic to it. Social builds awareness with single-fact posts.",
  "pillar_emphasis": "Built for the job — the low profile is a design decision driven by function, not aesthetics."
}
```
Campaign package pieces: all 6 pieces (2 audiences × 3 channels) — the 5 above plus:
- cold_weather × email:
```
The Lowline 2P. 3000mm waterproof, sealed seams, DAC Featherlite poles. 40-inch profile sits below wind shear. Two vestibules keep wet gear out of the sleeping area. Details and full specs on the site.
```
- cold_weather × social_media:
```
3000mm waterproof. 40-inch profile. Built for the camps where weather is the point. The Lowline 2P.
```

**Brief 2: Basecamp 4P Cross-Segment**

Single (backcountry_hikers × product_page):
```
9 lbs 4 oz. DAC Pressfit poles oversized for sustained wind load. 5000mm waterproof coating on recycled polyester fly. Near-vertical walls for livable volume. Two doors, two vestibules. Footprint included. This is not a backpacking tent. It is a basecamp for groups who set up, stay put, and run day missions from a fixed camp.
```
Rules: Technical register, backcountry adaptation, pillar: built_for_job.
Rationale: "Leads with weight as an honest statement, not a selling point. Explains what the tent is by first explaining what it isn't — backcountry audience respects that honesty."

Segment variants (product_page):
- backcountry_hikers: same as single above
- casual_car_camping:
```
Room for four adults and their gear without anyone hunching over. Two doors so nobody climbs over anybody at 2 AM. Handles heavy rain and sustained wind — 5000mm waterproof rating means serious waterproofing. Footprint included so you don't buy it separately. Sets up at camp and stays standing while you go do things.
```
Rules: Technical register adjusted for casual adaptation — specs translated to outcomes, pillar: built_for_job.
Rationale: "Casual audience gets the same product facts translated to lived outcomes. '5000mm waterproof' is explained. 'Near-vertical walls' becomes 'room without hunching.' Footprint inclusion is framed as savings."
What changed: "Casual variant translates every technical spec to an observable outcome. 'Near-vertical walls for livable volume' becomes 'room without hunching.' 'DAC Pressfit poles' disappears — casual buyers don't know or care about pole brands. 'Footprint included' gets a cost-saving frame."

Channel variants (backcountry_hikers):
- product_page: same as single above
- email:
```
The Basecamp 4P. Four-person shelter for fixed camps. 5000mm waterproof, DAC Pressfit poles, near-vertical walls. 9 lbs 4 oz with footprint. Not a backpacking tent — a tent that earns its weight by staying standing in conditions that matter.
```
What changed (product_page → email): "Email condenses to key facts and the core positioning statement. Register shifts from spec-dense fragments to brief conversational sentences."

Campaign package metadata:
```json
{
  "suggested_timing": "Product page updated first. Segment-specific emails sent to backcountry list and casual list on the same day with different copy. No social for this brief — product education works better in longer formats.",
  "content_hierarchy": "Two product pages (one per audience adaptation) are the anchors. Emails drive each segment to their respective page.",
  "pillar_emphasis": "Built for the job — the weight and wall geometry exist because of the use case, not despite it."
}
```
Campaign package pieces: all 4 (2 audiences × 2 channels) — the 3 above plus:
- casual_car_camping × email:
```
The Basecamp 4P. Room for four, two doors, handles weather you'd rather not be out in. Waterproof rating that actually means waterproof. Footprint included — no add-ons. Built for car camping trips where you want the tent to be the last thing you worry about.
```

**Brief 3: Slab 35QT Acquisition**

Single (casual_car_camping × social_media):
```
Five days of ice. $285. No celebrity endorsement included.
```
Rules: Casual/dry register, casual adaptation, pillar: pay_for_gear.
Rationale: "Social for acquisition leads with the two facts that matter to this audience: performance and price. The celebrity jab is peak Yowie dry humor — competitive positioning without naming competitors."

Segment variants (social_media — single audience note):
- casual_car_camping: same as single above
- Note: "This brief targets one audience segment (casual car camping). The Slab 35QT is a cooler-market acquisition product — the audience is unified. Segment variants would apply if this product also targeted backcountry buyers, but coolers serve a single audience in Yowie's catalog."

Channel variants (casual_car_camping):
- social_media: same as single above
- email:
```
The Slab 35QT. Rotomolded — one piece, no seams, nothing to crack. 3 inches of commercial-grade insulation. Five-day ice retention in 90-degree heat. Bear-resistant certified. $285. The coolers that cost $100 more are paying for a logo. This one pays for insulation.
```
- landing_page:
```
The Slab 35QT keeps ice for five days in 90-degree heat. That is not a marketing claim — it is a measured result. Rotomolded polyethylene, one piece, no seams to fail. 3 inches of commercial-grade insulation. Bear-resistant certified. Marine-grade stainless steel hardware. You can stand on it, sit on it, drag it across a parking lot. 35 quarts — 24 cans plus ice. $285. Compare that to the $400 coolers with the lifestyle branding. Same construction method. Same insulation class. Different price, because we skipped the logo.
```
What changed (social_media → email): "Email expands from one-liner to benefit stack. Same casual register, but now there's room to build the value argument. 'Pay for the gear' pillar gets explicit with the logo-vs-insulation comparison."
What changed (email → landing_page): "Landing page is the full pitch — every proof point deployed. Opens with the headline claim, immediately backs it with 'measured result.' Builds construction story, then drops the competitive comparison. This is the page social and email drive traffic to."

Campaign package metadata:
```json
{
  "suggested_timing": "Landing page goes live as destination. Social ads run first for awareness — short, punchy, price-forward. Email follows for subscribers who clicked but didn't buy. Sequence: social (day 1) → email (day 3) → retarget social (day 7).",
  "content_hierarchy": "Social is the hook. Email is the pitch. Landing page is the close. Each step adds detail — social is one line, email is a paragraph, landing page is the full argument.",
  "pillar_emphasis": "Pay for the gear, not the logo — the competitive positioning against premium cooler brands drives every piece."
}
```
Campaign package pieces: all 3 (1 audience × 3 channels) — the 3 above.

**Brief 4: Burrow 0°F Seasonal**

Single (cold_weather × email):
```
The Burrow 0°F. 800-fill goose down. Trapezoidal baffles. Comfort-rated to 0°F, lower limit -15°F. DWR-coated recycled shell. Winter doesn't care if you're cold. This bag does. Full specs and temperature data on the site.
```
Rules: Conversational/brief register, backcountry adaptation (cold_weather uses backcountry voice), pillar: quality_you_verify.
Rationale: "Email to cold-weather specialists leads with the specs they care about most — fill power, baffle construction, temperature ratings. The primary message line lands after the spec stack. Register is conversational but spec-dense because this audience reads specs like sentences."

Segment variants (email):
- cold_weather: same as single above
- backcountry_hikers:
```
The Burrow 0°F. 800-fill goose down, trapezoidal baffles, DWR-coated shell. Comfort-rated to 0°F. 3 lbs 6 oz. If your shoulder-season trips are pushing into real cold, this is the bag that handles it. Specs and field data on the site.
```
What changed: "Backcountry variant keeps full specs but shifts framing from 'winter camping' to 'shoulder-season trips pushing cold.' Weight is included (matters to backcountry audience), lower limit rating is dropped (less relevant for shoulder-season use). Closing is practical — 'handles it' — rather than the more pointed seasonal message."

Channel variants (cold_weather):
- email: same as single above
- social_media:
```
0°F comfort rating. 800-fill goose down. Trapezoidal baffles. Winter doesn't care if you're cold. The Burrow does.
```
- product_page:
```
800-fill responsibly sourced goose down. Trapezoidal baffles to eliminate cold spots. Comfort-rated to 0°F, lower limit -15°F. DWR-coated 20D recycled polyester shell sheds moisture before it reaches the down. Full-length insulated draft tube. 3 lbs 6 oz. Built for people who camp when the temperature drops below the point where most people stop camping.
```
What changed (email → social_media): "Social strips to the spec stack and the campaign line. No CTA, no explanation. The specs are the argument. The tagline is the closer."
What changed (email → product_page): "Product page adds specs that email omitted — DWR function explanation, draft tube, weight. Register shifts from conversational to technical fragments. The closing line defines the buyer rather than pitching to them."

Campaign package metadata:
```json
{
  "suggested_timing": "Product page updated with seasonal framing by early October. Email to cold-weather and backcountry segments in mid-October as temperatures drop. Social posts through October and November — one spec angle per post, campaign line as recurring anchor.",
  "content_hierarchy": "Product page is the reference. Email is the nudge — seasonal urgency without being pushy. Social repeats the campaign line across multiple posts with different spec leads.",
  "pillar_emphasis": "Quality you verify — the specs are the pitch. Temperature ratings, fill power, and baffle construction are verifiable claims that this audience will check."
}
```
Campaign package pieces: all 6 (2 audiences × 3 channels) — the 5 above plus:
- backcountry_hikers × social_media:
```
800-fill goose down. 0°F comfort rating. 3 lbs 6 oz. The Burrow — for shoulder-season camps that stop being shoulder season overnight.
```
- backcountry_hikers × product_page:
```
800-fill goose down. Trapezoidal baffles. Comfort-rated to 0°F, lower limit -15°F. DWR-coated 20D recycled polyester shell. 3 lbs 6 oz. The Burrow is a winter bag that earns its weight for shoulder-season trips where conditions shift. If you carry a 30°F bag and add layers to compensate, this replaces the bag and the layers.
```

The complete HTML file skeleton with this JSON data block should look like:

```html
<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <title>Yowie Content Engine</title>
  <style>/* styles added in Task 2 */</style>
</head>
<body>
  <!-- markup added in Task 3 -->
  <script>
    const CONTENT_DATA = { briefs: [...], generated: {...} };
    // JS added in Task 4
  </script>
</body>
</html>
```

- [ ] **Step 2: Verify the file was created and the JSON is valid**

Run: `cd "/Users/gregappler/Claude Code Projects/demo-marketing-ecosystem" && node -e "const fs = require('fs'); const html = fs.readFileSync('tools/content_engine.html','utf8'); const match = html.match(/const CONTENT_DATA = ({[\\s\\S]*?});/); const data = JSON.parse(match[1]); console.log('Briefs:', data.briefs.length); Object.keys(data.generated).forEach(k => console.log(k, ':', Object.keys(data.generated[k])))"`

Expected: 4 briefs, 4 generated keys each with single/segment_variants/channel_variants/campaign_package.

- [ ] **Step 3: Commit**

```bash
cd "/Users/gregappler/Claude Code Projects/demo-marketing-ecosystem"
git add tools/content_engine.html
git commit -m "Add content engine with embedded campaign data and pre-generated copy"
```

---

### Task 2: Build the CSS styles

**Files:**
- Modify: `tools/content_engine.html` (add styles in the `<style>` block)

The styles match the brand_config_viewer.html dark theme: dark navy background (#0a0a1a), gold accents (#e2c87d), teal highlights (#64ffda). The layout is a fixed left sidebar + scrollable main content area.

- [ ] **Step 1: Add all CSS styles**

Add the complete CSS inside the `<style>` tag. Key style sections:

**CSS variables** (match brand_config_viewer.html exactly):
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

**Layout** — sidebar + main:
```css
.layout {
  display: flex;
  min-height: 100vh;
}
.sidebar {
  width: 300px;
  flex-shrink: 0;
  background: var(--bg-secondary);
  border-right: 1px solid var(--border);
  padding: 24px;
  position: fixed;
  top: 0;
  left: 0;
  bottom: 0;
  overflow-y: auto;
}
.main {
  margin-left: 300px;
  flex: 1;
  padding: 32px 40px 64px;
  max-width: 900px;
}
```

**Sidebar elements:**
- `.sidebar-header`: brand name in gold, subtitle in secondary text
- `.brief-card`: background bg-selector, border, border-radius 8px, padding 16px, cursor pointer, margin-bottom 12px. Active state: gold left border, brighter background.
- `.brief-card h3`: 0.9rem, text-primary
- `.brief-card .brief-meta`: 0.75rem, text-secondary — product + stage
- `.stage-badge`: inline pill with 0.65rem text, uppercase, letter-spacing, border-radius 12px, padding 2px 10px. Colors: launch=teal, education=gold, acquisition=accent-red, seasonal=#b388ff (light purple)
- `.mode-selector`: group of 4 buttons below brief cards, separated by a divider line
- `.mode-btn`: same style as brand_config_viewer buttons — bg rgba(255,255,255,0.06), text-secondary, border, rounded. Active: gold background, dark text.

**Main content area:**
- `.brief-summary`: collapsible card showing brief inputs. Background bg-secondary, border, rounded. Grid layout for fields (label: value pairs). Collapsed state: just the title bar with expand chevron.
- `.brief-summary .field-label`: 0.7rem, uppercase, text-secondary, letter-spacing
- `.brief-summary .field-value`: 0.85rem, text-primary

**Content cards:**
- `.content-card`: background bg-secondary, border, border-radius 8px, padding 24px, margin-bottom 20px
- `.content-card .card-header`: flex row with channel badge (gold outline pill) and audience badge (teal outline pill)
- `.copy-block`: monospace font ('SF Mono', 'Fira Code', 'Consolas', monospace), 0.85rem, text-primary, background rgba(255,255,255,0.03), padding 20px, border-radius 6px, border-left 3px solid accent-gold, line-height 1.8
- `.meta-section`: collapsible, 0.8rem, text-secondary. Toggle arrow. Contains rules, pillar, rationale.
- `.meta-section .meta-label`: 0.7rem uppercase gold
- `.meta-section .meta-value`: 0.8rem text-secondary
- `.pillar-tag`: inline, 0.75rem, teal text, border 1px solid teal, rounded pill
- `.rationale`: italic, 0.8rem, text-secondary, border-left 2px solid accent-gold, padding-left 12px

**Variant annotations:**
- `.what-changed`: background rgba(226,200,125,0.08), border 1px dashed accent-gold at 30% opacity, rounded, padding 16px, margin 16px 0. Contains a label "What changed" in gold and the explanation text in text-secondary.

**Segment variant layout:**
- `.variant-pair`: CSS grid, 2 columns with gap 20px. On screens < 800px, stack to 1 column.

**Campaign package grid:**
- `.package-grid`: display grid, 1 column. Each card is full width and expandable.
- `.package-card-header`: clickable, shows audience × channel, expand/collapse chevron

**Attribution footer:**
- `.attribution`: text-center, 0.75rem, text-secondary, padding 24px, border-top

- [ ] **Step 2: Verify styles render**

Open the file in a browser — sidebar should appear on the left with dark theme. Main area should be blank (no markup yet).

Run: `open "/Users/gregappler/Claude Code Projects/demo-marketing-ecosystem/tools/content_engine.html"`

- [ ] **Step 3: Commit**

```bash
cd "/Users/gregappler/Claude Code Projects/demo-marketing-ecosystem"
git add tools/content_engine.html
git commit -m "Add content engine CSS styles matching brand config viewer theme"
```

---

### Task 3: Build the HTML markup

**Files:**
- Modify: `tools/content_engine.html` (add markup in `<body>`)

- [ ] **Step 1: Add all HTML markup**

Add the complete HTML structure inside `<body>`, before the `<script>` tag:

```html
<div class="layout">
  <!-- Sidebar -->
  <aside class="sidebar">
    <div class="sidebar-header">
      <h1>Yowie Content Engine</h1>
      <p>Campaign brief in. Branded content out.</p>
    </div>

    <div class="sidebar-section">
      <div class="section-label">Campaign Briefs</div>
      <div id="brief-list">
        <!-- JS renders brief cards here -->
      </div>
    </div>

    <div class="sidebar-divider"></div>

    <div class="sidebar-section">
      <div class="section-label">Output Mode</div>
      <div id="mode-selector">
        <button class="mode-btn" data-mode="single">Single</button>
        <button class="mode-btn" data-mode="segments">Segments</button>
        <button class="mode-btn" data-mode="channels">Channels</button>
        <button class="mode-btn" data-mode="package">Full Package</button>
      </div>
    </div>

    <div class="sidebar-footer">
      <p>Content generated by Claude Code from the Brand Configuration Layer and campaign brief.</p>
    </div>
  </aside>

  <!-- Main content -->
  <main class="main">
    <div id="brief-summary">
      <!-- JS renders brief summary here -->
    </div>
    <div id="content-output">
      <!-- JS renders content cards here -->
    </div>
  </main>
</div>
```

- [ ] **Step 2: Verify markup structure**

Open in browser — sidebar should show header text, section labels, mode buttons. Main area empty.

Run: `open "/Users/gregappler/Claude Code Projects/demo-marketing-ecosystem/tools/content_engine.html"`

- [ ] **Step 3: Commit**

```bash
cd "/Users/gregappler/Claude Code Projects/demo-marketing-ecosystem"
git add tools/content_engine.html
git commit -m "Add content engine HTML markup structure"
```

---

### Task 4: Build the JavaScript rendering logic

**Files:**
- Modify: `tools/content_engine.html` (add JS in the `<script>` block, after `CONTENT_DATA`)

- [ ] **Step 1: Add state management and initialization**

```javascript
const state = {
  activeBrief: CONTENT_DATA.briefs[0].id,
  activeMode: CONTENT_DATA.briefs[0].default_mode
};

function init() {
  renderBriefList();
  renderModeSelector();
  renderContent();
}

document.addEventListener('DOMContentLoaded', init);
```

- [ ] **Step 2: Add sidebar rendering functions**

```javascript
function renderBriefList() {
  const container = document.getElementById('brief-list');
  container.innerHTML = CONTENT_DATA.briefs.map(brief => `
    <div class="brief-card ${brief.id === state.activeBrief ? 'active' : ''}"
         data-brief="${brief.id}" onclick="selectBrief('${brief.id}')">
      <h3>${brief.name}</h3>
      <div class="brief-meta">
        ${brief.product_line} <span class="stage-badge stage-${brief.campaign_stage}">${brief.campaign_stage}</span>
      </div>
    </div>
  `).join('');
}

function renderModeSelector() {
  document.querySelectorAll('.mode-btn').forEach(btn => {
    btn.classList.toggle('active', btn.dataset.mode === state.activeMode);
  });
}

function selectBrief(briefId) {
  const brief = CONTENT_DATA.briefs.find(b => b.id === briefId);
  state.activeBrief = briefId;
  state.activeMode = brief.default_mode;
  renderBriefList();
  renderModeSelector();
  renderContent();
}

function selectMode(mode) {
  state.activeMode = mode;
  renderModeSelector();
  renderContent();
}

// Attach mode button listeners
document.getElementById('mode-selector').addEventListener('click', e => {
  if (e.target.classList.contains('mode-btn')) {
    selectMode(e.target.dataset.mode);
  }
});
```

- [ ] **Step 3: Add brief summary rendering**

```javascript
function renderBriefSummary(brief, alwaysExpanded) {
  const fields = [
    ['Product', `${brief.name || ''} (${brief.product_line})`],
    ['Audiences', brief.audience_segments.map(a => formatAudienceName(a)).join(', ')],
    ['Channels', brief.channels.map(c => formatChannelName(c)).join(', ')],
    ['Stage', brief.campaign_stage],
    ['Primary Message', brief.primary_message],
    ['Proof Points', brief.proof_points.join(' · ')],
    ['CTA', typeof brief.cta === 'string' ? brief.cta : Object.entries(brief.cta).map(([k,v]) => `${formatAudienceName(k)}: ${v}`).join(' | ')],
    ['Price', brief.pricing]
  ];
  if (brief.secondary_messages && brief.secondary_messages.length) {
    fields.splice(5, 0, ['Secondary', brief.secondary_messages.join(' · ')]);
  }

  const expandedClass = alwaysExpanded ? 'expanded always-expanded' : 'collapsed';
  return `
    <div class="brief-summary ${expandedClass}" onclick="toggleBriefSummary(this)">
      <div class="summary-header">
        <span class="summary-title">Campaign Brief: ${brief.name}</span>
        <span class="chevron">${alwaysExpanded ? '' : '▸'}</span>
      </div>
      <div class="summary-fields">
        ${fields.map(([label, value]) => `
          <div class="summary-field">
            <span class="field-label">${label}</span>
            <span class="field-value">${value}</span>
          </div>
        `).join('')}
      </div>
    </div>
  `;
}

function toggleBriefSummary(el) {
  if (el.classList.contains('always-expanded')) return;
  el.classList.toggle('expanded');
  el.classList.toggle('collapsed');
  const chevron = el.querySelector('.chevron');
  chevron.textContent = el.classList.contains('expanded') ? '▾' : '▸';
}
```

- [ ] **Step 4: Add content card rendering**

```javascript
function renderContentCard(piece) {
  const metaId = `meta-${piece.audience}-${piece.channel}-${Math.random().toString(36).slice(2, 8)}`;
  return `
    <div class="content-card">
      <div class="card-header">
        <span class="badge badge-channel">${formatChannelName(piece.channel)}</span>
        <span class="badge badge-audience">${formatAudienceName(piece.audience)}</span>
      </div>
      <div class="copy-block">${piece.copy}</div>
      <div class="meta-section" id="${metaId}">
        <div class="meta-toggle" onclick="toggleMeta('${metaId}')">
          <span class="meta-toggle-label">Rules & Rationale</span>
          <span class="meta-chevron">▸</span>
        </div>
        <div class="meta-content">
          <div class="meta-group">
            <span class="meta-label">Voice</span>
            <span class="meta-value">${piece.rules_applied.voice.join(', ')}</span>
          </div>
          <div class="meta-group">
            <span class="meta-label">Channel Register</span>
            <span class="meta-value">${piece.rules_applied.channel_register}</span>
          </div>
          <div class="meta-group">
            <span class="meta-label">Audience Adaptation</span>
            <span class="meta-value">${piece.rules_applied.audience_adaptation}</span>
          </div>
          <div class="meta-group">
            <span class="meta-label">Pillar</span>
            <span class="pillar-tag">${piece.pillar.headline}</span>
            <span class="meta-value pillar-resonance">${piece.pillar.resonance}</span>
          </div>
          <div class="rationale">${piece.rationale}</div>
        </div>
      </div>
    </div>
  `;
}

function toggleMeta(id) {
  const section = document.getElementById(id);
  section.classList.toggle('expanded');
  section.querySelector('.meta-chevron').textContent =
    section.classList.contains('expanded') ? '▾' : '▸';
}
```

- [ ] **Step 5: Add the main renderContent function with all 4 modes**

```javascript
function renderContent() {
  const brief = CONTENT_DATA.briefs.find(b => b.id === state.activeBrief);
  const gen = CONTENT_DATA.generated[state.activeBrief];
  const summaryEl = document.getElementById('brief-summary');
  const outputEl = document.getElementById('content-output');

  const alwaysExpanded = state.activeMode === 'package';
  summaryEl.innerHTML = renderBriefSummary(brief, alwaysExpanded);

  let html = '';

  switch (state.activeMode) {
    case 'single':
      html = renderContentCard(gen.single);
      break;

    case 'segments':
      if (gen.segment_variants.note) {
        html += `<div class="single-segment-note">${gen.segment_variants.note}</div>`;
        html += renderContentCard(gen.segment_variants.pieces[0]);
      } else {
        html += '<div class="variant-pair">';
        html += gen.segment_variants.pieces.map(p => renderContentCard(p)).join('');
        html += '</div>';
        if (gen.segment_variants.what_changed) {
          html += renderWhatChanged(gen.segment_variants.what_changed);
        }
      }
      break;

    case 'channels':
      gen.channel_variants.pieces.forEach((piece, i) => {
        html += renderContentCard(piece);
        if (i < gen.channel_variants.pieces.length - 1 && gen.channel_variants.what_changed && gen.channel_variants.what_changed[i]) {
          html += renderWhatChanged(gen.channel_variants.what_changed[i]);
        }
      });
      break;

    case 'package':
      if (gen.campaign_package.metadata) {
        html += renderPackageMetadata(gen.campaign_package.metadata);
      }
      gen.campaign_package.pieces.forEach(piece => {
        html += renderContentCard(piece);
      });
      break;
  }

  outputEl.innerHTML = html;
}
```

- [ ] **Step 6: Add helper rendering functions**

```javascript
function renderWhatChanged(change) {
  return `
    <div class="what-changed">
      <span class="what-changed-label">What changed</span>
      <p>${change.summary}</p>
    </div>
  `;
}

function renderPackageMetadata(meta) {
  return `
    <div class="package-metadata">
      <div class="meta-group">
        <span class="meta-label">Pillar Emphasis</span>
        <span class="meta-value">${meta.pillar_emphasis}</span>
      </div>
      <div class="meta-group">
        <span class="meta-label">Content Hierarchy</span>
        <span class="meta-value">${meta.content_hierarchy}</span>
      </div>
      <div class="meta-group">
        <span class="meta-label">Suggested Timing</span>
        <span class="meta-value">${meta.suggested_timing}</span>
      </div>
    </div>
  `;
}

function formatChannelName(key) {
  const names = {
    product_page: 'Product Page',
    email: 'Email',
    social_media: 'Social Media',
    landing_page: 'Landing Page'
  };
  return names[key] || key;
}

function formatAudienceName(key) {
  const names = {
    backcountry_hikers: 'Backcountry Hikers',
    thru_hikers: 'Thru-Hikers',
    casual_car_camping: 'Casual / Car Camping',
    cold_weather: 'Cold-Weather Specialists'
  };
  return names[key] || key;
}
```

- [ ] **Step 7: Verify full functionality**

Open in browser. Test:
1. All 4 brief cards appear in sidebar, first is active
2. Clicking a brief switches content and resets to default mode
3. All 4 mode buttons work
4. Brief summary expands/collapses (always expanded in package mode)
5. Content cards show copy, rules toggle works
6. Segment variant "what changed" annotations appear
7. Channel variant annotations appear between cards
8. Full package shows metadata + all cards

Run: `open "/Users/gregappler/Claude Code Projects/demo-marketing-ecosystem/tools/content_engine.html"`

- [ ] **Step 8: Commit**

```bash
cd "/Users/gregappler/Claude Code Projects/demo-marketing-ecosystem"
git add tools/content_engine.html
git commit -m "Add content engine JavaScript rendering and interaction logic"
```

---

### Task 5: Write process log and tutorial

**Files:**
- Modify: `docs/process_log.md`
- Create: `tutorials/03_content_engine.md`

- [ ] **Step 1: Add process log entry**

Prepend a new entry to `docs/process_log.md` (newest first):

```markdown
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
```

- [ ] **Step 2: Create tutorial**

Create `tutorials/03_content_engine.md`:

```markdown
---
title: Building the Content Engine
version: 1.0
date: 2026-04-09
tool: Content Engine
output: tools/content_engine.html
---

# Tutorial 03: Building the Content Engine

## What This Tool Does

The Content Engine takes structured campaign briefs and produces on-brand, segment-specific, channel-ready marketing content. It demonstrates how the Brand Configuration Layer drives content decisions — from voice rules to audience adaptation to channel register shifts.

## Step 1: Define the Brief Structure

The first design decision was the campaign brief schema. A brief captures everything needed to generate content:

- **Product line and product ID** — links to brand_config.json product data
- **Audience segments** — determines voice adaptation (backcountry vs. casual)
- **Channels** — determines register (technical, conversational, casual/dry)
- **Campaign stage** — launch, education, acquisition, seasonal
- **Primary message** — the core idea the content must communicate
- **Proof points** — specific facts and specs to support the message
- **CTA** — can be audience-specific (different CTAs for different segments)

### Prompt Used

"Build the Content Engine for Yowie. It reads the Brand Configuration Layer and takes a structured campaign brief as input. The brief includes: product line, audience segment, channel, content type, campaign stage, primary message, and optional fields (secondary messages, pricing, proof points, tone override, CTA, personalization variables). The engine produces on-brand, segment-specific, channel-ready content."

## Step 2: Choose the Content Generation Approach

The key decision: **hybrid approach**. Content is pre-generated by Claude at build time, not generated live. This means:

- The demo shows real AI-written copy, not template fill-ins
- No API keys or live generation needed
- The tool works as a static HTML file
- Output quality is curated and verified

### Why Pre-Generated

Live generation would require API keys and couldn't guarantee brand compliance. Pre-generating lets us verify every piece against the brand config rules before shipping.

## Step 3: Design the Campaign Briefs

Four briefs were chosen to cover maximum range:

1. **Lowline 2P Launch** — new product, backcountry + cold-weather audiences, 3 channels
2. **Basecamp 4P Cross-Segment** — same product pitched to backcountry vs. casual buyers
3. **Slab 35QT Acquisition** — cooler market, single audience, 3 channels including landing page
4. **Burrow 0°F Seasonal** — full campaign package, seasonal urgency

Each brief maps to a default output mode that best demonstrates its value.

## Step 4: Generate Content Using Brand Rules

For each brief × audience × channel combination, content was generated following:

1. **Voice rules** — all 4 core attributes (plainspoken, confident, restrained, dry) always apply
2. **Channel register** — product page (technical, direct), email (conversational, brief), social (casual, dry)
3. **Audience adaptation** — backcountry (assumed technical literacy, raw specs) vs. casual (translate specs to outcomes)
4. **Messaging pillar** — selected by cross-referencing product_emphasis and audience_emphasis rankings
5. **Terminology** — approved words used, forbidden words avoided, conditional words checked

### How Pillar Selection Works

The engine looks at two ranked lists in brand_config.json:
- `messaging.product_emphasis[product_line]` — pillar priority for this product type
- `messaging.audience_emphasis[audience]` — pillar priority for this audience

The highest-ranked pillar that appears in both lists becomes the primary pillar for that content piece.

## Step 5: Build the Interactive Tool

The tool uses the same single-file HTML pattern as the Brand Config Viewer:

- **Left sidebar** — brief selector cards + output mode buttons
- **Main area** — brief summary + rendered content cards
- **Four output modes** — single, segments, channels, full package

Each content card shows the copy plus collapsible metadata: rules applied, pillar used, audience adaptation, and a rationale explaining why the content was shaped that way.

### Key UI Decisions

- "What changed" annotations between variant cards explain adaptation logic
- Brief summary is collapsible (always expanded in full package mode)
- Monospace font for generated copy distinguishes it from UI text
- Dark theme matches brand_config_viewer.html

## Step 6: Verify Against Brand Rules

Each generated piece was checked against:
- No forbidden words (revolutionary, game-changing, adventure, etc.)
- No exclamation points
- No superlatives without evidence
- Approved terminology used correctly
- Channel register matches (social is shortest, product page is spec-dense)
- Audience adaptation is measurable (backcountry uses raw specs, casual translates)

## Output

Final tool: `tools/content_engine.html`

Open in any browser — no server required. Select a brief, switch output modes, explore how the same message adapts across segments and channels.
```

- [ ] **Step 3: Commit**

```bash
cd "/Users/gregappler/Claude Code Projects/demo-marketing-ecosystem"
git add docs/process_log.md tutorials/03_content_engine.md
git commit -m "Add process log and tutorial for content engine"
```
