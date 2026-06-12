<div align="center">

# 🚀 Prompt Craft v3

**Three-Tier Prompt Architecture for AI-Powered Web Generation**

> 用户一句需求 → 设计方向 → 技术栈 → 可运行代码 · One-sentence requirement to production-ready output

[![Version](https://img.shields.io/badge/version-3.0.0-blue)](https://github.com/xcsweb/prompt-craft)
[![License](https://img.shields.io/badge/license-MIT-green.svg)](LICENSE)
[![Made with AI](https://img.shields.io/badge/made%20with-AI%20%2B%20TRAE-purple)](#)

</div>

---

## 📖 What is Prompt Craft?

Prompt Craft is a structured prompt engineering system that turns vague user requirements into consistent, high-quality web applications. Instead of relying on magic one-liner prompts, it uses a **three-tier architecture** (T1 project rules → T2 expert personas → T3 task templates) to produce reliable, reusable output.

**The Problem It Solves**

- ❌ AI generates inconsistent styles each run
- ❌ Output lacks design systems / typography / color hierarchy
- ❌ No standard process for accessibility, mobile fallback, or performance
- ❌ Can't easily "switch direction" while staying in the same design language

**How It Solves Them**

```
                    User says "make me a website"
                             │
    ┌────────────────────────┼────────────────────────┐
    │                        │                        │
    ▼                        ▼                        ▼
T1 · Project Rules     T2 · Expert Personas    T3 · Task Templates
（tech-stack contracts）（who's coding）      （blueprint to follow）
                         ┌───────────────────────┐
                         │  Composed Prompt       │
                         │  = T1 + T2 + T3 + user │
                         └───────────┬─────────────┘
                                     ▼
                              Working Code
                                     ▼
                               15-point QA checklist
                                     ▼
                                  Deliver
```

---

## 🎯 Three-Tier Architecture

### T1 · Project Rules / 项目规则

Hard contracts that every output must follow. These are the "laws" that ensure consistent quality.

| Category | Files | Purpose |
|----------|-------|---------|
| Artistic websites | `project-rules/artistic-stack.md` | GSAP + Lenis + typography system + color budget |
| Next.js + shadcn | `project-rules/nextjs-shadcn.md` | Production-grade fullstack scaffold |
| Vue 3 + Vite + TS | `project-rules/vite-vue3-typescript.md` | Modular SPA architecture |
| Node.js Backend | `project-rules/node-express-prisma.md` | REST/GraphQL API scaffold |
| Shared Security | `project-rules/shared-security.md` | OWASP-informed hardening rules |

### T2 · Expert Personas / 专家角色

The "mind" behind the code. Determines *tone, priorities, and quality bar*.

| Persona | File | Best For |
|---------|------|----------|
| Creative Technologist | `expert-personas/creative-technologist.md` | Artistic / Awwwards-level websites |
| Senior Frontend Engineer | `expert-personas/senior-frontend-engineer.md` | Standard web applications |
| Senior Backend Engineer | `expert-personas/senior-backend-engineer.md` | APIs, databases, infrastructure |
| Senior Fullstack Engineer | `expert-personas/senior-fullstack-engineer.md` | End-to-end features |
| Senior AI Agent Developer | `expert-personas/senior-ai-agent-developer.md` | Agent frameworks / RAG / LLM apps |

### T3 · Task Templates / 任务模板

The "blueprints" — reusable prompt scaffolds for specific jobs.

| Template | File | Use Case |
|----------|------|----------|
| Build Artistic Website | `task-templates/build-artistic-website.md` | Brand sites, portfolios, landing pages |
| Build Dashboard | `task-templates/build-dashboard.md` | Data visualization, admin panels |
| Build Login Form | `task-templates/build-login-form.md` | Auth UI flows |
| Build CRUD API | `task-templates/build-crud-api.md` | REST/GraphQL endpoints |
| Debug Errors | `task-templates/debug-error.md` | Troubleshooting workflow |
| Write Tests | `task-templates/write-tests.md` | Test generation templates |
| Code Review | `task-templates/code-review.md` | Pull request review prompts |

### Reference Library / 参考资料库

The heart of the system — design patterns and signatures that keep output consistent.

| Reference | File | Content |
|-----------|------|---------|
| **Artistic Website DNA** | `references/artistic-website-dna.md` | ⭐ **3 design directions × 15 interaction signatures** |
| Awwwards-Level Website | `references/frontend-awwwards-website.md` | Full design → code guide for high-end sites |
| Frontend Dashboard v2 | `references/frontend-dashboard-v2.md` | Admin / dashboard patterns |
| Fullstack SaaS | `references/fullstack-saas.md` | SaaS product architecture |
| Real-time Data Viz | `references/dataviz-realtime.md` | Live data visualization patterns |
| AI Agent / RAG / Chatbot | `references/ai-agent.md`, `ai-rag.md`, `ai-chatbot.md` | AI application blueprints |
| Backend: API / Auth / DB / MQ / Scheduler | Multiple files | Backend pattern library |
| System Architecture Guide | `SYSTEM_PROMPT_ARCHITECTURE.md` | Prompt system design doc |

---

## 🎨 Design DNA Library

### 3 Design Directions / 三大设计方向

| Direction | Color System | Typography | Layout | Best For |
|-----------|-------------|-----------|--------|----------|
| **A · Editorial Magazine** | Cream `#F5F3EF` + ink-black + terracotta accent (3-color budget) | Serif display (Newsreader/Playfair) + sans body (Inter) · `clamp(3rem, 10vw, 9rem)` headings | Asymmetric two-column · massive whitespace 14-20vh · giant section numbers | Design studios · culture · luxury brands |
| **B · Cyberpunk Dark** | Deep black `#0A0A0F` + near-white text + magenta `#FF2E97` + cyan `#00F0FF` | Condensed geometric sans · ALL-CAPS wide letter-spacing labels | Canvas-first · WebGL fills screen · HUD-style UI floats on top | Game studios · interactive media · music · sci-fi launches |
| **C · Playful Exploration** | Sky/grass/orange + signature red | Rounded geometric sans · monospace numerals · "pinned" to 3D world | No traditional layout — page is a 3D map the user explores/drives | Personal portfolios · education · creative tools |

### 15 Interaction Signatures / 15 个交互签名

Pick 3–5 per project. This is the "menu" — signature elements that make output professional.

| # | Signature | Effect |
|---|-----------|--------|
| IS-01 | **Mask Reveal** | Text clipped by a mask, "opens" bottom→top on load |
| IS-02 | **SplitText Stagger** | Headings split per-character, staggered reveal |
| IS-03 | **Scroll-driven Line Grow** | Section underlines animate as you scroll |
| IS-04 | **Custom Cursor + Magnetic** | Hidden native cursor + ring that sticks to buttons on hover |
| IS-05 | **Hover Invert** | Cards flip from light→dark on hover |
| IS-06 | **Scroll-driven Shader** | Hero WebGL shader evolves with scroll position |
| IS-07 | **Horizontal Scroll** | Vertical scroll pins + slides content horizontally |
| IS-08 | **Sticky Reveal Narrative** | Section header sticks; content swaps underneath |
| IS-09 | **Shader Orb** | 3D fresnel-lit orb reacts to mouse |
| IS-10 | **Canvas 2D Particles** | 100–300 particles that converge toward cursor |
| IS-11 | **SVG Noise Paper** | 2–4% opacity `feTurbulence` simulating print texture |
| IS-12 | **Tilt 3D Card** | Perspective tilt on card hover |
| IS-13 | **Page Wipe** | Full-screen color wipe on route change |
| IS-14 | **Long-form Editor Scroll** | Optimized reading + paragraph motion |
| IS-15 | **Fixed Background Progress** | Static hero morphs subtly as user scrolls |

> **Full technical details** (GSAP timelines, CSS values, easing curves) available in [`references/artistic-website-dna.md`](references/artistic-website-dna.md).

---

## 🖼️ Examples Gallery

Six real, working websites generated using this system. Every file is a **single self-contained HTML** — open it in any browser, no build step.

| # | Name | Style | Lines | Live Preview | Source |
|---|------|-------|-------|--------------|--------|
| 01 | Basic Official Site | Corporate, clean | ~700 | Open `index.html` | [`examples/01-basic-official-site/`](examples/01-basic-official-site/) |
| 02 | Standard Official Site v2 | Refined corporate | ~570 | Open `index.html` | [`examples/02-standard-official-site-v2/`](examples/02-standard-official-site-v2/) |
| 03 | Advanced Animation Site | Motion-heavy, GSAP | ~570 | Open `index.html` | [`examples/03-advanced-animation-site/`](examples/03-advanced-animation-site/) |
| 04 | **HELIOS** | Editorial · Magazine | ~900 | Open `index.html` | [`examples/04-editorial-helios/`](examples/04-editorial-helios/) |
| 05 | **BRUTALIST** | Neo-Swiss · Black/Lime | ~1270 | Open `index.html` | [`examples/05-brutalist-swiss/`](examples/05-brutalist-swiss/) |
| 06 | Dashboard Data Viz | KPI / charts / dark theme | ~510 | Open `index.html` | [`examples/06-dashboard-data-viz/`](examples/06-dashboard-data-viz/) |

Each example demonstrates a different combination of direction + signatures. Compare HELIOS (Direction A · mask-reveal + magnetic cursor + hover-invert) vs BRUTALIST (Direction A variant · canvas spotlight + split-text + horizontal-scroll + block-hover-reveal) to see how the DNA library enables variation.

---

## 🚦 Quick Start

### Option 1 · Use with TRAE / Claude Code

1. Clone or copy `prompt-craft/` into your workspace
2. Load `SKILL.md` as your skill entry point
3. Say: **"Make a design studio website"** — the system composes T1 + T2 + T3 automatically
4. Output goes to a working directory. Defaults to single-file `index.html` (no build).

### Option 2 · Use as a Prompt Scaffold (any LLM)

1. Open [`SKILL.md`](SKILL.md) — it contains the full system summary in ~380 lines
2. For an artistic website, pair it with:
   - `expert-personas/creative-technologist.md` (mindset)
   - `project-rules/artistic-stack.md` (tech contract)
   - `task-templates/build-artistic-website.md` (prompt template)
   - `references/artistic-website-dna.md` (design menu — pick 3 directions + 3–5 signatures)
3. Feed all four to any LLM. The architecture ensures consistent, high-quality output.

### Option 3 · View the 6 Example Sites

Just open any `examples/*/index.html` in a browser — nothing to install.

---

## 📁 Project Structure

```
prompt-craft/
├── SKILL.md                              ← ⭐ System entry point (read this first)
├── README.md                             ← You're here
├── SYSTEM_PROMPT_ARCHITECTURE.md         ← Architecture design document
├── .preheat                              ← (TRAE internal)
│
├── project-rules/                        ← T1 · Hard contracts / 项目级硬约束
│   ├── artistic-stack.md
│   ├── nextjs-shadcn.md
│   ├── vite-vue3-typescript.md
│   ├── node-express-prisma.md
│   └── shared-security.md
│
├── expert-personas/                      ← T2 · AI mindset / 专家角色
│   ├── creative-technologist.md
│   ├── senior-frontend-engineer.md
│   ├── senior-backend-engineer.md
│   ├── senior-fullstack-engineer.md
│   └── senior-ai-agent-developer.md
│
├── task-templates/                       ← T3 · Blueprint prompts / 任务模板
│   ├── build-artistic-website.md
│   ├── build-dashboard.md
│   ├── build-login-form.md
│   ├── build-crud-api.md
│   ├── debug-error.md
│   ├── write-tests.md
│   └── code-review.md
│
├── plugins/                              ← Optional feature plugins / 插件
│   ├── plugin-auth.md
│   ├── plugin-docker.md
│   └── plugin-i18n.md
│
├── references/                           ← Design DNA & patterns / 参考资料库
│   ├── artistic-website-dna.md           ← ⭐ Core: 3 dirs × 15 signatures
│   ├── frontend-awwwards-website.md
│   ├── frontend-dashboard-v2.md
│   ├── fullstack-saas.md
│   ├── ai-agent.md, ai-rag.md, ai-chatbot.md
│   ├── backend-api.md, backend-auth.md, backend-database.md
│   ├── backend-microservices.md, backend-mq.md, backend-scheduler.md
│   └── ...
│
└── examples/                             ← 6 working websites / 6 个示例站点
    ├── 01-basic-official-site/
    ├── 02-standard-official-site-v2/
    ├── 03-advanced-animation-site/
    ├── 04-editorial-helios/
    ├── 05-brutalist-swiss/
    └── 06-dashboard-data-viz/
```

---

## ✅ 15-Point QA Checklist

Every generated website is auto-checked against:

- [ ] Color budget ≤ 4 colors
- [ ] Typography uses `clamp()` with viewport units
- [ ] GSAP ScrollTrigger + Lenis installed and active
- [ ] `prefers-reduced-motion` fallback implemented
- [ ] ≤ 768px mobile breakpoint + simplified animations
- [ ] ≥ 3 interaction signatures from the DNA library
- [ ] WebGL has Canvas 2D / static SVG fallback
- [ ] Font loading (`document.fonts.ready`) triggers animations (no FOUT jank)
- [ ] No external cookies / analytics without user request
- [ ] CSS design tokens in `:root` (`--bg`, `--text`, `--accent`, `--line`)
- [ ] Easing: `power3.out` / `power4.out` (no `linear` / `ease-in-out`)
- [ ] Keyboard focus rings preserved
- [ ] Code is complete, no `// TODO` / `// ...` placeholders
- [ ] Single-file prototype: one `index.html` / no build step
- [ ] Works offline (assets via CDN or inline — no private-domain dependencies)

---

## 📜 License

MIT — use, modify, ship. If this helped you ship something cool, a star ⭐ is appreciated.

---

<div align="center">

**The goal of Prompt Craft is simple:**
> Replace "I hope the AI gets it right this run" with "I know exactly what it will produce — and I can reason about *why*."

</div>
