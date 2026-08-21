# **Resume Site Color Palette**

**Target Profile:** Tatenda Uta — AI & Analytics Decision Partner

**Design Aesthetic:** Enterprise AI & Modern Data Analytics (High-Trust, Clean, Human)

**Inspiration Standard:** Scale AI, Vercel, Stripe, Palantir

**Status:** Implemented in `index.html` as of 2026-08-21. This document describes what's actually live, not just a proposal — if you change a color in the site, update this file to match.

## **1. Executive Summary & Design Rationale**

This color architecture moves away from typical "generated AI templates" (pitch-black backgrounds, neon purples, aggressive glowing gradients) toward an enterprise-ready, human-designed palette. It balances technical authority (SQL, Python, AI prompts) with executive clarity ($1.4M+ revenue impact, strategic alignment).

Every color used on the site is a **stock Tailwind CSS default** — `slate`, `blue`, and `emerald` — applied directly as utility classes (`text-slate-900`, `bg-blue-600`, `bg-emerald-50/80`, etc.). No custom hex values or arbitrary `[#...]` classes remain in the markup. This was a deliberate implementation choice: it needs no build step or config translation layer, and it's trivially greppable.

## **2. Palette Architecture (60-30-10 Rule)**

| Category | Tailwind Classes | Description & Usage |
| :---- | :---- | :---- |
| **Base Canvas (60%)** | `bg-slate-50` (`#F8FAFC`) / `bg-white` for cards | Porcelain off-white page background with white cards floating on top for separation. |
| **Primary Text & Structure (30%)** | `text-slate-900` (`#0F172A`) / `text-slate-700` (`#334155`) / `text-slate-500` (`#64748B`) / `text-slate-400` (`#94A3B8`) | A 4-tier neutral hierarchy: headings, body copy, captions, and tertiary labels respectively. |
| **Primary Accent — Analytics** | `blue-600` (`#2563EB`) solid / `blue-800` (`#1E40AF`) hover-deep | All interactive elements: CTA buttons, nav active states, icons, links, focus/hover states. |
| **Secondary Accent — ROI & Growth** | `emerald-600`/`emerald-800` text, `emerald-50/80` tint bg, `emerald-200` border, `emerald-500` for status dots | Used **strictly** for revenue/dollar callouts and verified/positive indicators — see Section 4 for the exact list of where this applies. Never used as a generic third UI color. |
| **Borders / dividers** | `border-slate-200` default, `border-slate-300` on hover | Replaces the old translucent `border-black/10`/`/15` utilities site-wide. |

## **3. Tailwind Config**

`index.html`'s inline `tailwind.config` still declares a documentation-only custom palette (unused by any class in the markup — everything uses the stock classes above instead):

```js
tailwind.config = {
    theme: {
        extend: {
            fontFamily: {
                sans: ['Inter', 'sans-serif'],
            },
            colors: {
                canvas: {
                    light: '#F8FAFC',
                    card: '#FFFFFF',
                    border: '#E2E8F0',
                    hover: '#CBD5E1'
                },
                brand: {
                    blue: '#2563EB',
                    navy: '#1E40AF',
                    emerald: '#059669',
                    slate: '#0F172A',
                    muted: '#64748B'
                }
            }
        }
    }
}
```

## **4. Specific Component Guidelines (as implemented)**

### **A. Header / Logo Badge**

The "TU" logo badge next to the name is **white background, `text-blue-600`, `border-blue-200`** — matching the favicon exactly (both are white-bg/cobalt-blue "TU"). This is a deliberate deviation from a generic "dark logo badge" pattern, chosen so the header mark and the browser-tab favicon read as the same brand mark.

### **B. Status Badge**

The "NDA Protected & Generalized" pill: `bg-slate-100 text-slate-700 border-slate-200`, with an animated `bg-emerald-500` pulse dot (both the ping and the solid dot span).

### **C. Cards & Metric Tiles**

* **Standard container:** `bg-white border border-slate-200 shadow-sm hover:border-slate-300`
* **Initiative / Scale tile** (non-dollar tiles, e.g. "20+ Cross-Functional Initiatives," "Product • AI • Growth • Risk," skill/leadership icon boxes): `bg-blue-50/80 border-blue-200 text-blue-800`
* **Revenue / Dollar Impact tile:** `bg-emerald-50/80 border-emerald-200 text-emerald-800` — applies to:
  - The homepage "$1.4M+ Incremental Revenue Engineered" tile
  - Each project detail page's "Estimated Impact" tile, **conditionally**: `renderProjectDetail()` checks whether `project.impactRange` contains `$` and picks emerald (dollar figure, ~7 of 11 projects) or blue (non-dollar metric like "50% Faster Reporting Cycles," ~4 of 11)

### **D. Buttons & Nav**

* **Primary CTA** ("Let's Connect," "Explore Projects," filter-pill active state, nav active state): `bg-blue-600 hover:bg-blue-800 text-white`
* **Secondary/outline button** ("Contact Tatenda," "View Resume"): `bg-white border border-slate-300 text-slate-900 hover:bg-slate-100`
* **Text link pairs** (e.g. "View All Projects"): resting `text-blue-800`, hover `text-blue-600` — brightens on hover, intentional direction

### **E. Diagnostic Terminal (Code Context)**

Not currently used anywhere in `index.html` (no dark terminal/code-block component exists on the site). If one is added later, use `bg-slate-900 border border-slate-800 text-slate-300` with `amber-500` for warnings and `sky-400` for function/logic accents, per the original spec.

## **5. Anti-Patterns to Avoid**

1. ❌ **No pitch-black backgrounds** — don't use `#000000`/`bg-black` as a body or section background.
2. ❌ **No neon purple/pink gradients.**
3. ❌ **No low-contrast text** — verify 4.5:1 WCAG contrast before introducing a new text color, especially at small caption sizes (the `text-slate-400` tier is the lightest in use and is reserved for non-essential micro-labels only).
4. ❌ **No excessive glow effects** — keep shadows subtle (`shadow-sm`/`shadow-md`, low-opacity color shadows like `shadow-blue-600/20`).
5. ❌ **No emerald as a generic accent** — if you're tempted to reach for emerald because "we need a third color here," that's a sign to use blue instead. Emerald is reserved for money and verified/positive indicators only (Section 4B).
