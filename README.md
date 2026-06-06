# heartland-pos

**Signal F Holdings LLC — POS Intelligence Repository**

Private knowledge base, audit framework, and YR Hub pipeline integration for the Heartland Restaurant POS system.

---

## Purpose

This repository is the single source of truth for working with Heartland POS menu data. It contains:

- The core logic schema for how Heartland structures menu items, modifiers, pricing, and coursing
- Audit scripts that scan an existing menu and report issues before any changes are made
- A shadow build methodology that lets clients verify a rebuilt menu before going live
- YR Hub pipeline prompt templates for the three-agent review (Ruffo → Nemotron → Gemini)

---

## Folder Structure

```
heartland-pos/
├── knowledge/                  ← How Heartland works (the intelligence layer)
│   ├── coursing-logic.md
│   ├── modifier-architecture.md
│   ├── pricing-rules.md
│   ├── temporal-logic.md
│   └── midnight-rule.md
├── audit/                      ← Scripts that analyze an existing menu
│   ├── scripts/
│   │   ├── modifier-scan.py    ← Finds cross-contamination
│   │   ├── redundancy-check.py ← Finds duplicated entries (pepperoni × 22)
│   │   └── 86-risk-report.py   ← Shows cascade risk if an item is disabled
│   └── report-template.md      ← Client-facing audit report format
├── rebuild/                    ← Shadow build tools and cutover process
│   ├── shadow-build-guide.md
│   └── cutover-instructions.md
├── prompts/                    ← YR Hub pipeline prompt templates
│   ├── ruffo-prompt.md
│   ├── nemotron-prompt.md
│   └── gemini-prompt.md
├── docs/                       ← Reference documents
└── exports/                    ← Sample clean output files
```

---

## The Workflow

### Step 1 — Audit
Log into the client's Heartland portal manually. Trigger the audit script. Review the report. Present findings to client. Wait for approval before making any changes.

### Step 2 — Scope Agreement
If only 5 items need changing: targeted fix.
If modifiers are a mess: propose full rebuild. Get written approval.

### Step 3 — Shadow Build
Build v2 versions of every channel (Dine-In v2, Carry-Out v2, Pick-Up v2, Online v2) in parallel with the live system. Client verifies each channel. Client controls their own cutover. No downtime, ever.

### Step 4 — YR Hub Pipeline Review
Paste normalized output into YR Hub Expo Proxy Pipeline:
- **Ruffo Coder** — validates the code/script logic
- **Nemotron Architect** — validates schema integrity and modifier isolation
- **Gemini Integrator** — writes the integration spec for DoorDash / menu board / website

### Step 5 — Cutover
Client receives cutover instructions. They flip the switch when confident. Old channels remain available as fallback until explicitly retired.

---

## Core Philosophy

- **Audit before action** — never change a live system without a written report first
- **Shadow builds only** — clients never lose revenue during a rebuild
- **Modifier isolation by default** — no cross-contamination unless explicitly flagged
- **Never 86 in the system** — contact the customer directly; protect revenue streams
- **One repo per POS system** — clean separation, no rule conflicts between systems

---

## Maintained by

Christopher Bailey — Signal F Holdings LLC  
[YR Hub](https://yr-hubspecialkeyneeded.pplx.app) — AI Agent Platform
