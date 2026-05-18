# Caffeine Interactive Deck — Claude Code Handover

## What this is

An interactive HTML presentation for DFINITY/Caffeine's "Hello Self-Writing Cloud" talk. Not a PowerPoint — every slide is a standalone interactive experience that *demonstrates* ICP's value rather than just describing it. Single HTML file, zero dependencies, hostable on GitHub Pages.

**Origin:** Dominic Williams' Indus AI Week deck (Feb 2026, Pakistan). The static PDF version is at `/uploads/Business_breakfast_Islamabad__1_-compressed.pdf`. This project turns it into something people can touch.

**Live URL target:** `christopherharrison.xyz/caffeine-deck` (GitHub Pages, not yet deployed)

---

## Current state

### Files

| File | Status | Description |
|------|--------|-------------|
| `caffeine-poc-deck.html` | **Active — work on this** | Main deck, 4 interactive slides built, ~1215 lines |
| `web2-3d-model.html` | **Complete** | Standalone 3D Traditional Stack vs ICP architecture comparison (Three.js). Will be embedded as slide or iframe. ~1219 lines |
| `caffeine-deck.html` | **Deprecated** | First attempt — static HTML version of the PDF with transitions. Scrapped because it was just a transcription, not interactive. Ignore this file. |

### Built slides (in caffeine-poc-deck.html)

**Slide 1 — Attack Simulator** (`#slide-atk`)
- Split-screen: Traditional Stack (left, red) vs ICP (right, blue)
- 12 server nodes per side as a grid
- 3 attack buttons: Ransomware, DDoS Flood, Insider Attack
- Traditional side: nodes die with cascade animation, status panel degrades (uptime drops, data compromised, services fail), attack log streams red entries
- ICP side: nodes pulse and bounce attacks, consensus stays active, all canisters healthy, log streams blue "rejected" entries
- Reset button restores everything
- Key functions: `launchAttack(type)`, `resetAttack()`, `killTradNode()`, `bounceIcpNode()`

**Slide 2 — Wish Machine** (`#slide-wish`)
- Text input: type an app idea, press Enter
- 5-stage build pipeline: Analyze → Design → Generate → Verify → Deploy
- Each stage activates sequentially with streaming text output (line by line)
- Code rain particle effect during the Generate stage
- Ends with a live ICP canister URL and deploy time
- Recognizes keywords: coffee/café, CRM/customer, inventory/stock, booking/schedule — each returns tailored output
- Key functions: `runBuild(config)`, `getWishConfig(text)`, `spawnCodeDrop()`

**Slide 3 — Timeline Slider** (`#slide-tl`)
- Draggable slider from 1950 to 2026+
- 7 eras: Mainframe → Client-Server → Cloud/DevOps → Serverless → Vibe Coding → AI + Traditional Cloud → Self-Writing on ICP
- Visual workspace shows team figures (colored by role: dev/design/ops/AI) that shrink from 8 people to 1 AI agent
- Stats update live: team size, build time, cost, security rating
- App visualization fills in as technology improves
- Click year marks to jump to specific eras
- Key functions: `setPos(pos)`, `renderEra(era)`, `getEra(pos)`

**Slide 4 — Sovereign World Map** (`#slide-map`)
- 20 clickable countries as positioned blocks on a proportional layout
- Each country has: foreign cloud dependency %, provider breakdown (AWS/Azure/Google/Alibaba/Huawei with brand colors), risk assessment text
- Sidebar shows animated dependency meter, provider breakdown, risk text
- "Deploy ICP Sovereign Cloud" button: turns selected country blue, drops dependency to 0%, shows deployment readout (nodes needed, subnet creation, CLOUD Act immunity)
- Pakistan at 85% dependency — designed for the Indus AI Week audience
- Key functions: `selectCountry(id)`, `toggleSovereignty()`, `buildMap()`, `applyDepColor()`

---

## Slides still to build

These were discussed and approved. Add them to the same `caffeine-poc-deck.html` file:

### Slide 5 — Subnet Builder (drag-and-drop cloud engine creator)
- User drags node machines onto a world map or rack visualization
- Pick locations (country flags or city labels)
- Configure: internet / cloud-on-cloud / sovereign mode
- Watch the subnet "spin up" with connection lines between nodes
- Show resulting cloud engine specs: replication factor, fault tolerance, latency estimate
- References the Caffeine Cloud Engines concept (3 modes: internet, cloud-on-cloud, sovereign)

### Slide 6 — Market Growth (interactive revenue projection)
- Animated chart: cloud computing revenue $1T (2025) → $2T (2030)
- Draggable handle to adjust self-writing market penetration %
- Revenue numbers recalculate live as you drag
- Split between SaaS/software layer ($1.2T by 2030) and compute layer ($800B)
- Show ICP's addressable market within the self-writing segment

### Slide 7 — 3D Model Embed
- Full-screen embed of `web2-3d-model.html` via iframe
- The 3D model shows Traditional Stack's 7-layer architecture with exploded view, failure cascade simulations, and ICP comparison mode
- Important: scroll/wheel events should pass through to the 3D model when this slide is active (don't let deck navigation capture them)
- Add an overlay label: "INTERACTIVE — CLICK LAYERS · TOGGLE ICP MODE · TRIGGER FAILURES"
- The iframe src should be `web2-3d-model.html` (same directory)

---

## Architecture & patterns

### Deck engine
- Pure vanilla JS, no frameworks
- Slides are `div.slide` elements with `.active` class for visibility
- Navigation: dot nav at bottom, arrow keys, touch swipe
- Each slide's JS is wrapped in an IIFE `(function(){ ... })();` to avoid global pollution
- Registration in the nav IIFE: update the `slides[]` array and `names[]` array when adding slides

### Adding a new slide — checklist
1. Add CSS in the `<style>` block (find the section headers like `/* ═══ SLIDE N: NAME ═══ */`)
2. Add HTML inside `<div class="deck">` before `</div><!-- /deck -->`
3. Give it `class="slide"` and a unique `id="slide-xxx"`
4. Add JS in a new IIFE before `</script>`
5. Update the nav registration:
   ```js
   var slides=[..., document.getElementById('slide-xxx')];
   var names=[..., 'SLIDE NAME'];
   ```
6. Update the slide label initial text count: `SLIDE 1 OF N`
7. Update the `slideLabel` update line count in `go()` function

### Design system
- Fonts: `Anybody` (display/numbers), `IBM Plex Mono` (labels/code/mono), `Manrope` (body)
- Colors: defined in `:root` — use CSS variables
  - ICP brand: `--blue:#29ABE2`, `--orange:#F15A24`, `--yellow:#FBB03B`, `--purple:#522785`
  - Semantic: `--green:#00e676`, `--red:#ff2d55`, `--lime:#c8ff00`, `--cyan:#00d4ff`
  - Background: `--bg:#050508`, surfaces near-black
- All backgrounds are dark (#050508 base). Text is white/light.
- Noise overlay: `.noise` div for subtle texture
- Interactive elements need clear hover/active states
- Animations: CSS transitions preferred, JS `setTimeout` chains for sequenced events

### 3D model details (web2-3d-model.html)
- Three.js r128 (loaded from CDN)
- Traditional Stack has 7 explodable layers: CDN, Load Balancer, API Gateway, App Server, Cache, Database, Storage
- ICP has 4 layers: Boundary Nodes, NNS Governance, Subnet & Canisters, Node Machines
- ICP colors are brand-accurate: Blue, Orange, Yellow, Green, Purple
- Features: clickable layers with info panel, failure cascade simulation, dual request trace (side-by-side), stats bar, corporate dependency overlays
- All user-facing text says "Traditional Stack" (not "Web2")
- Auto-rotation, starfield particles, draggable info/trace panels

---

## Key decisions & rules

1. **"Traditional Stack" not "Web2"** — all user-facing instances changed. Internal variable names kept as `web2` to avoid breaking things.
2. **Every slide must be interactive** — no static text slides. If a concept can't be demonstrated interactively, it doesn't belong in this deck.
3. **Single HTML file** — everything inline (CSS + JS + HTML). External resources: only Google Fonts CDN and Three.js CDN (for the 3D model file only).
4. **ICP tooling rule** — always use `icp` (icp-cli), never `dfx` (legacy). Applies to any code snippets or references in the deck content.
5. **No crypto jargon for general audiences** — "digital asset wallet" not "crypto wallet", "network custody" not "stores data on chain"
6. **Caffeine context** — Caffeine Labs Inc is the commercial self-writing cloud platform built on ICP. Caffeine Engine v3.0 launching Q2 2026. Motoko is the programming language developed by Caffeine's AI team.

---

## Hosting

Both files go in the same GitHub Pages directory:
```
caffeine-deck/
  ├── index.html          (renamed from caffeine-poc-deck.html)
  └── web2-3d-model.html  (loaded via iframe in slide 7)
```

Target URL: `christopherharrison.xyz/caffeine-deck`
Domain: GoDaddy, GitHub Pages with custom domain (A records + CNAME already configured for the `/trad-stack-vs-icp` path).

---

## What needs improvement (feedback from Christopher)

The existing 4 slides are "a great start, needs work." No specific issues called out yet — the priority was to keep building forward. When iterating on existing slides, test each one for:
- Does the interactive element actually demonstrate the concept, or is it decorative?
- Is the visual hierarchy clear — can someone understand the point in 3 seconds?
- Do animations feel purposeful or gratuitous?
- Mobile responsiveness (basic media query exists but needs testing)
