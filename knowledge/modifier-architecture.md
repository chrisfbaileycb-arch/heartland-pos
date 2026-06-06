# Modifier Architecture — Heartland POS

## Core Philosophy (Signal F Holdings Standard)

> Modifiers are isolated to the item they belong to by default.
> No modifier cross-contaminates another item unless explicitly and intentionally flagged as shared.
> This protects revenue: 86ing one ingredient never accidentally disables unrelated menu items.

---

## Key Attributes

| Attribute | Type | Description |
|-----------|------|-------------|
| Min Choices | Integer | Minimum number of modifier selections required |
| Max Choices | Integer | Maximum number of modifier selections allowed |
| Included Count | Integer | Number of selections included in base price |
| Half & Half Toggle | Boolean | Allows modifier to apply to half the item (pizza, sandwiches) |
| Is Shared | Boolean | If false (default), modifier is scoped to this item only |
| Sub-Item Flag | Boolean | Allows modifier to import its own modifier set |

---

## Isolation Rules

### Default Behavior
```
ISOLATED_BY_DEFAULT: true

Every modifier belongs exclusively to its parent item unless Is Shared = true.
```

### Shared Modifiers (Explicitly Flagged Only)
Only these modifier types should ever be shared across items:
- **Size** (Small, Medium, Large) — applies to entire categories
- **Preparation style** (Well Done, Medium Rare) — applies to proteins across categories
- **Temperature** (Hot, Iced) — applies to beverages across categories

### Item-Scoped Modifiers (Never Shared)
These must ALWAYS be item-scoped, never shared:
- Individual toppings (pepperoni, extra cheese, jalapeños)
- Specific sauces tied to one dish
- Protein add-ons that differ by dish context
- Any modifier whose price varies by parent item

---

## The Pepperoni Problem (Why This Matters)

**Wrong (default Heartland setup for most mom & pops):**
```
Pepperoni modifier → attached to: Pizza, Calzone, Stromboli, Pasta, Salad
86 pepperoni → disables it across ALL 5 categories simultaneously
Revenue loss across entire menu
```

**Right (Signal F Holdings standard):**
```
MODIFIER GROUP: Pizza Toppings
  └── pepperoni [item-scoped, Pizza only]

MODIFIER GROUP: Pasta Add-ons
  └── pepperoni [item-scoped, Pasta only]

MODIFIER GROUP: Salad Proteins
  └── pepperoni [item-scoped, Salad only]

86 pizza pepperoni → only affects Pizza category
Pasta and Salad pepperoni remain fully operational
Revenue protected
```

---

## Sub-Item Architecture

When a modifier is complex enough to have its own modifiers, use the Sub-Item flag.

**Example: "Side Salad" as a modifier on an entree**
```
Entree: Grilled Chicken
  └── Modifier: Side Salad [Sub-Item = true]
        └── Sub-modifiers: Dressing (Ranch, Italian, Balsamic)
                           Size (Small, Large)
```

The Side Salad modifier imports its own modifier group. This prevents listing every salad/dressing combination as a top-level modifier on the entree.

---

## Half & Half Logic

- Only enable on items where it makes semantic sense (pizza, sub sandwiches)
- When Half & Half = true, the modifier price is calculated at 50% per half
- Both halves must be from the same modifier group
- Do not enable Half & Half on items where portion logic doesn't apply (pasta, beverages)

---

## Modifier Multiplier Pricing

For "Extra" or "Double" contexts:
```
Base ingredient: Cheese = $1.00
Multiplier modifier: "Extra" = 2.0x
Calculated add-on: $2.00 (not $1.00 + $1.00 — use the multiplier, not stacking)
```

---

## Agent Instructions

**Ruffo:** When building modifier groups, always default Is Shared = false. Only set Is Shared = true when explicitly instructed. Check that no topping-type modifier appears on more than one item category.

**Nemotron:** When auditing, flag every modifier that appears identically named across multiple categories. Run the 86-risk-report to quantify cascade damage. Present the client with the revenue impact of the current cross-contamination before recommending a rebuild.
