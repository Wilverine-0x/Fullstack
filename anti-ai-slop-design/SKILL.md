---
name: anti-ai-slop-design
description: Guidelines and rules to eliminate AI slop, clichés, and generic templates in frontend UI/UX design, ensuring human-grade editorial quality, information density, and brand polish.
---

# Anti-AI-Slop Professional UI/UX Design System

Use this skill whenever designing or implementing user interfaces, landing pages, SaaS dashboards, and marketing web experiences to ensure they look crafted by elite, human product designers rather than generic AI generators.

## 1. What Defines "AI Slop" & What to Avoid

| AI Slop Anti-Pattern | Human-Crafted Alternative |
| :--- | :--- |
| **Generic Purple/Cyan/Magenta gradients** on deep dark backgrounds with blurry orbs. | **Intentional brand palette**: High-trust editorial tones (Deep Forest Green/Emerald, Crisp Slate, Warm Off-White, Warm Charcoal). |
| **Empty marketing buzzwords** ("Unleash AI synergies", "Revolutionize your workflow with next-gen intelligence"). | **Concrete, specific value propositions**: Real numbers, direct benefits, localized terminology, and clear problem-solving statements. |
| **Excessive glowing cards with oversized borders** and low contrast text. | **Clean 1px borders (`border-border/60`)**, crisp typography, high accessibility contrast ratio (WCAG AAA/AA). |
| **Floaty illustrations or generic 3D rendered shapes** that communicate nothing. | **Rich product UI mockups, interactive simulators**, authentic receipt/document previews, and realistic live components. |
| **Loose, low-density layouts** with giant empty gaps. | **Professional density**: Informative tables, tabular figures (`font-mono` / `tabular-nums`), badges with status dots, and crisp information hierarchy. |
| **Fictional placeholder data** ("Lorem Ipsum", "John Doe spent $999.00"). | **Hyper-realistic domain data**: Real local vendor names (e.g., Alfamart, Indomaret, SPBU Pertamina, Tokopedia, Shopee, BCA, Mandiri), accurate tax codes (PPN 11%, PPh 23, PPh Final 0.5%), realistic line-item totals with proper thousand separators (`Rp 48.500`). |

## 2. Visual Design System Standards

### A. Color Palette (Finance & Trust)
- **Primary / Emerald**: `#047857` (emerald-700), `#065F46` (emerald-800), `#0D5C3A` (deep forest)
- **Primary Foreground**: `#FFFFFF`
- **Background (Light)**: `#FBFBF9` (warm off-white/canvas) or `#FFFFFF`
- **Card Background**: `#FFFFFF` with subtle border `#E5E7EB` / `#E2E4DC`
- **Dark Mode**: `#0F1412` (rich warm black/charcoal), `#161D1A` (card surface), `#222D28` (borders)
- **Status Colors**:
  - Success / Matched: `#059669` (Emerald)
  - Warning / Review: `#D97706` (Amber)
  - Neutral / Pending: `#64748B` (Slate)
  - Error / Mismatch: `#DC2626` (Rose)

### B. Typography & Micro-copy
- **Font Stack**: Clean, modern grotesk/sans (e.g. Plus Jakarta Sans, Inter, Geist, Satoshi) for headings and body.
- **Figures**: Always use `tabular-nums font-mono` for currency, transaction amounts, percentages, and timestamps.
- **Bilingual Polish**: When implementing dual-language (ID/EN), ensure natural idiomatic phrasing, not literal machine translation.

### C. Layout & Interactivity
- **Sticky, Blur-backdrop Navigation**: Compact headers with crisp active states, seamless language toggle (ID 🇮🇩 / EN 🇬🇧), and dark/light theme switchers.
- **Interactive Demonstrations**: Provide working receipt scanner simulators, interactive tax calculators, and live reconciliation previews.
- **Tactile Feedback**: Subtle button press states (`active:scale-[0.98]`), gentle transitions (`transition-all duration-200`), and smooth tooltips.
