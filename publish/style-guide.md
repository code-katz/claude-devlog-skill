# 🎨 Claude Tool Suite — Visual Style Guide

> **Applies to:** `claude-team-cli` · `claude-devlog-skill` · `claude-roadmap-skill`

---

## 1. Brand Identity

### 1.1 Suite Name

The three projects form a single suite referred to as the **Claude Tool Suite** in documentation and marketing materials. Each project retains its own name but shares this visual identity.

| Project | Short Label | Role |
|---|---|---|
| `claude-team-cli` | **Team CLI** | Multi-agent orchestration from the terminal |
| `claude-devlog-skill` | **Devlog Skill** | Structured development changelog skill |
| `claude-roadmap-skill` | **Roadmap Skill** | Project planning and roadmap skill |

---

## 2. Color Palette

### 2.1 Primary Colors

These are the foundation colors used across all projects.

| Name | Hex | Usage |
|---|---|---|
| **Charcoal** | `#141413` | Primary text, dark backgrounds |
| **Warm White** | `#faf9f5` | Light backgrounds, text on dark |
| **Mid Gray** | `#b0aea5` | Secondary text, borders, dividers |
| **Light Gray** | `#e8e6dc` | Subtle backgrounds, code blocks |

### 2.2 Accent Colors

Each project has a **designated accent** for visual distinction, drawn from a shared palette.

| Name | Hex | Assigned To | Usage |
|---|---|---|---|
| **Rust Orange** | `#d97757` | `claude-team-cli` | Primary accent, CTAs, highlights |
| **Slate Blue** | `#6a9bcc` | `claude-devlog-skill` | Primary accent, CTAs, highlights |
| **Sage Green** | `#788c5d` | `claude-roadmap-skill` | Primary accent, CTAs, highlights |

### 2.3 Extended Palette (for illustrations)

Use these softer tones in illustrations and diagrams alongside the primary accents.

| Name | Hex | Usage |
|---|---|---|
| **Soft Teal** | `#7ab5b0` | Secondary illustration color |
| **Muted Purple** | `#8b7eb8` | Secondary illustration color |
| **Warm Tan** | `#c4a882` | Background accents in illustrations |

---

## 3. Typography

### 3.1 Fonts

| Context | Font | Fallback |
|---|---|---|
| **Headings** | Poppins (SemiBold 600) | Arial, Helvetica |
| **Body Text** | Lora (Regular 400) | Georgia, serif |
| **Code / Project Names** | JetBrains Mono | Fira Code, Consolas, monospace |

### 3.2 Project Name Rendering

Project names should always be rendered in **monospace** with their accent color:

- `claude-team-cli` → JetBrains Mono, `#d97757`
- `claude-devlog-skill` → JetBrains Mono, `#6a9bcc`
- `claude-roadmap-skill` → JetBrains Mono, `#788c5d`

In Markdown contexts where color isn't available, use inline code formatting:

```
`claude-team-cli`
```

---

## 4. GitHub README Headers

### 4.1 Format

Each repo gets an SVG header banner (1280×320px) with:

- Dark background (`#141413`)
- Project name in monospace, colored with its accent
- A subtle tagline beneath in `#b0aea5`
- No logos, no icons — just clean typographic treatment

### 4.2 Placement

```markdown
<p align="center">
  <img src="assets/header.svg" alt="project-name" width="100%">
</p>
```

### 4.3 File Location

Store headers at `assets/header.svg` in each repo root.

---

## 5. Image Style Guide (for Medium & Documentation)

### 5.1 General Style

All illustrations follow a **flat vector / minimal cartoon** style:

- **Clean white or `#faf9f5` backgrounds** — no gradients, no textures
- **Simple geometric shapes** — circles, rounded rectangles, soft edges
- **Flat coloring** — no shadows, no 3D effects, no gradients
- **Minimal detail** — convey the concept, not photorealism
- **Outlined icons** preferred over filled (2px stroke weight)
- **Consistent character style** — if people appear, use simple silhouettes or avatar-style figures

### 5.2 Image Inventory

Below are the ~7 images needed (1 header + 6 content images). Use these as prompts for an image generation tool (Midjourney, DALL-E, etc.).

#### Image 1: Suite Header (Medium Article)
- **Size:** 1400×800px
- **Subject:** Developer at desk with laptop; 10 floating specialist icons in a semicircle above the screen (clipboard, shield, magnifying glass, wrench, chart, megaphone, rocket, database cylinder, paint palette, stopwatch)
- **Style:** Simple cartoon, flat vector, muted purples/blues/teals
- **Usage:** Medium article hero image

#### Image 2: Architecture Overview
- **Size:** 1200×675px
- **Subject:** High-level system diagram showing the three tools connected — terminal icon (CLI), scroll/log icon (Devlog), map/calendar icon (Roadmap) — with arrows flowing between them
- **Style:** Clean diagram-illustration hybrid; use accent colors for each tool
- **Usage:** Medium article, README overview sections

#### Image 3: CLI in Action
- **Size:** 1200×675px
- **Subject:** Stylized terminal window with colored text lines; small agent avatars appearing beside command output
- **Style:** Dark terminal aesthetic (`#141413` background), accent color highlights
- **Usage:** `claude-team-cli` README, Medium article

#### Image 4: Devlog Concept
- **Size:** 1200×675px
- **Subject:** A notebook or scroll unrolling with milestone markers, checkpoints, and small decision-tree branches
- **Style:** Flat illustration, Slate Blue (`#6a9bcc`) dominant accent
- **Usage:** `claude-devlog-skill` README, Medium article

#### Image 5: Roadmap Concept
- **Size:** 1200×675px
- **Subject:** A winding road or timeline with milestone flags, branching paths, and a horizon/destination point
- **Style:** Flat illustration, Sage Green (`#788c5d`) dominant accent
- **Usage:** `claude-roadmap-skill` README, Medium article

#### Image 6: Workflow / How It Works
- **Size:** 1200×675px
- **Subject:** A step-by-step flow (3–5 steps) showing: prompt → agent delegation → tool execution → output — using simple icons for each step
- **Style:** Horizontal flowchart-illustration hybrid; numbered steps; accent colors cycle through orange → blue → green
- **Usage:** Medium article, documentation

#### Image 7: Community / Extensibility
- **Size:** 1200×675px
- **Subject:** Puzzle pieces or modular blocks snapping together; one block is highlighted as "your custom skill"
- **Style:** Flat vector, warm/inviting tones, `#faf9f5` background
- **Usage:** Medium article conclusion, contributing docs

### 5.3 Prompt Template for Image Generation

When generating images with AI tools, prepend this base prompt to each description:

```
Simple, clean flat vector illustration on a white (#faf9f5) background.
Minimal detail, soft muted colors (palette: #d97757, #6a9bcc, #788c5d,
#7ab5b0, #8b7eb8, #141413). No text. No logos. Rounded shapes, 2px
outlines, no gradients or shadows. Suitable for a tech blog header.
```

Then append the specific image description from the inventory above.

---

## 6. Diagram Style (Mermaid / SVG)

When creating technical diagrams in documentation:

- Use **LR (left-to-right)** direction for flowcharts
- Node fill colors should use the accent color of the relevant project
- Edge/arrow color: `#b0aea5`
- Text in nodes: `#141413` on light fills, `#faf9f5` on dark fills
- Keep diagrams to **≤10 nodes** for readability
- Rounded rectangle nodes preferred (`([text])` or stadium shapes)

---

## 7. Do's and Don'ts

### ✅ Do

- Use the designated accent color for each project consistently
- Keep illustrations simple — if it takes more than 3 seconds to "get it," simplify
- Maintain generous whitespace in all visuals
- Use monospace for all project name references
- Keep GitHub headers typographic only (no icons/illustrations)

### ❌ Don't

- Don't use gradients, drop shadows, or 3D effects
- Don't mix accent colors within a single project's context
- Don't use photography or realistic illustrations
- Don't add the Anthropic logo (these are community/independent projects)
- Don't use more than 3 colors in a single illustration (pick from the palette)

---

## 8. File Naming Convention

All visual assets follow this pattern:

```
{project-short-name}-{description}-{size}.{ext}
```

Examples:
- `team-cli-header-1280x320.svg`
- `suite-architecture-1200x675.png`
- `devlog-concept-1200x675.png`

---

*Last updated: March 2026*
