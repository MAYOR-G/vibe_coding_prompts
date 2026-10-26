# 03. UI/UX & Design System Prompt (`design.md`)

> **How to use:** Paste this prompt in the **same chat session** right after finalizing `PRD.md`. The AI will first present 3 tailored visual/color directions for you to choose from (or let you specify your own), and once you pick one, it will generate the complete `DESIGN.md` file.

---

## Prompt

Now, act as a Senior Product Designer and Design Systems Architect specializing in modern, high-craft production web applications.

### CONTEXT
Use our approved **`PRD.md`** and conversation history above as the product source of truth. Every screen, component, and user flow in the design system must directly support the features and pages defined in `PRD.md`.

### WORKFLOW (2-STEP PROCESS)

#### STEP 1: Visual Direction & Color Palette Selection (Ask First)
Unless I have already told you my exact color and theme preferences, **do NOT generate the full `DESIGN.md` yet.** First, analyze the product's audience and vibe from `PRD.md`, and present me with:
1. **3 Distinct Visual & Color Directions** tailored to this specific product (e.g., Option A: Crisp Minimalist Light, Option B: High-Contrast Dark-First, Option C: Warm Editorial / Brand-Forward). For each option, list:
   - Theme polarity (Light mode only, Dark mode only, or System/Dual theme)
   - Exact Hex codes for Primary Background, Surface/Card, Primary Text, Secondary Text, Primary Accent, and Border
   - Recommended Font Pairing (Heading, Body, and Monospace)
   - Overall aesthetic feel (border sharpness, depth style)
2. **2 to 3 quick design questions** covering any layout preferences (e.g., sidebar vs. top navbar, compact/dense data view vs. spacious layout, any reference websites I admire).

Wait for my reply. Once I select an option (or customize it), proceed immediately to **Step 2**.

---

#### STEP 2: Generate the Complete `DESIGN.md`
Once my visual direction is locked in, create a comprehensive **`DESIGN.md`** that serves as the single source of truth for the application's UI/UX so coding agents build a cohesive, non-generic interface.

Structure `DESIGN.md` with these exact sections:

1. **Visual Direction & Design Principles**
   - Core aesthetic philosophy and brand personality.
   - 3 to 5 strict UX principles guiding layout and interaction trade-offs.
   - **Anti-Patterns (What to Avoid):** Explicit rules banning generic AI-generated SaaS clichés (e.g., random neon purple-to-blue gradients, overused glowing blobs, inconsistent border radii, cluttered cards).

2. **Color System & Semantic Tokens**
   - Complete color palette table with **Exact Hex / RGB values** and semantic token names:
     - **Backgrounds:** Page canvas, elevated surface/card, modal overlay, subtle hover background.
     - **Typography:** Primary text, secondary/body text, muted/placeholder text, inverted text.
     - **Accents & Brand:** Primary interactive accent (default, hover, active), secondary accent.
     - **Borders & Dividers:** Subtle hairline border, active/focused input border.
     - **Semantic Status Colors:** Success, Warning, Danger/Error, and Info (background tint, border, and text/icon hex codes).

3. **Typography System**
   - Font families (`Heading`, `Body`, `Monospace`) and fallback stacks.
   - Complete type scale (`Display`, `H1`, `H2`, `H3`, `H4`, `Body Large`, `Body Default`, `Small/Caption`, `Micro/Label`) with exact font sizes (`rem`/`px`), font weights, line heights, and letter spacing.
   - Readability rules (maximum line width `ch` for prose, number alignment using `tabular-nums`).

4. **Spacing, Layout Grid & Elevation System**
   - Base spacing unit (e.g., 4px/8px spatial scale) and standard padding/gap tokens.
   - Container max-widths, page margins, and column grid specifications.
   - **Border Radius Scale:** Exact radii for small elements (badges/inputs), medium elements (buttons/cards), and large containers (modals/panels).
   - **Depth & Elevation:** How surfaces are separated (e.g., 1px subtle borders vs. layered shadows) with exact shadow/border values.

5. **Reusable Component Specifications**
   - Define consistent visual and behavioral rules (variants, sizes, and `default`, `hover`, `focus`, `active`, `disabled` states) for:
     - Buttons (Primary, Secondary, Outline, Ghost, Destructive)
     - Cards & Containers
     - Badges, Pills & Status Indicators
     - Modals, Drawers & Confirmation Dialogs
     - Tables, Lists & Data Display items
     - Toast Notifications & Alerts

6. **Navigation & Form UX Rules**
   - Primary navigation pattern (desktop and mobile adaptation), active route indicators, and breadcrumbs.
   - Form layout standards: input height, label placement, helper text, inline validation timing, required field indicators, and submit button behavior.

7. **UI Feedback States (Loading, Empty & Error States)**
   - **Loading States:** Exact visual treatment (skeleton shimmer matching layout geometry vs. inline spinner) and prevention of cumulative layout shift (CLS).
   - **Empty States:** Visual hierarchy, icon/illustration style, concise copy rules, and primary Call-to-Action placement.
   - **Error States:** Inline field errors, toast banners, and full-page fallback views (404, 500, offline).

8. **Micro-Interactions & Motion Guidelines**
   - Purposeful animation rules: transition durations (e.g., `150ms` micro-hover, `250ms` drawer/modal transition), easing curves (`cubic-bezier`), and reduced-motion (`prefers-reduced-motion`) compliance.

9. **Responsive Behavior & Breakpoints**
   - Exact viewport breakpoints (`sm`, `md`, `lg`, `xl`).
   - How multi-column grids, navigation bars, data tables, and padding adapt across mobile, tablet, and desktop screens.

10. **Accessibility (a11y) Standards**
    - WCAG AA/AAA contrast ratio minimums, visible keyboard focus ring specifications, touch target minimum sizes (`44x44px`), and semantic HTML/ARIA requirements.

11. **Page-by-Page Layout Blueprint**
    - Map every single page/screen listed in `PRD.md` to its exact layout structure:
      - Layout shell used (e.g., Marketing Shell, Auth Centered Card, App Sidebar Shell).
      - Visual hierarchy, section breakdown, and component placement from top to bottom.

### STRICT RULES
- **Zero Implementation Code:** Do not write CSS files, Tailwind configs, or React/HTML code. Write clear, deterministic design specifications and token tables.
- **Usability Over Decoration:** Every visual rule must serve clarity, scannability, and speed. Define reusable system tokens, never one-off page hacks.

### OUTPUT FORMAT (FOR STEP 2)
Return the complete `DESIGN.md` inside a single clean Markdown code block (` ```markdown ... ``` `) ready to save directly as `DESIGN.md`.