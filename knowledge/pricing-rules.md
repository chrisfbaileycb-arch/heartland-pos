# Pricing Rules (Dynamic) — Heartland POS

## Core Concept

Heartland supports layered dynamic pricing that goes beyond a single base price. Rules can adjust, override, or discount prices based on context, time, location, or combinations of those factors.

---

## Rule Types

| Rule Type | Description | Example |
|-----------|-------------|---------|
| Force Price | Overrides base price entirely | Happy Hour beer: always $3.00 |
| Dollar Adjust | Adds or subtracts a fixed dollar amount | Late night: +$1.00 surcharge |
| Percentage Adjust | Adds or subtracts a percentage | Employee discount: -20% |
| Multiplier | Multiplies base price by a factor | "Extra" modifier: 2.0x ingredient cost |

Negative values on Dollar Adjust and Percentage Adjust create discounts.

---

## Priority Hierarchy

When multiple rules apply to the same item simultaneously:

```
Rules are evaluated top-to-bottom.
The FIRST matching rule wins.
No subsequent rules apply to the same item in the same evaluation pass.
```

**Implication:** Order your rules carefully. The most specific rule must always appear above the most general rule, or the general rule will fire first and the specific rule will never be reached.

---

## Scope Filters

Every pricing rule supports optional scope restrictions:

| Filter | Description |
|--------|-------------|
| RoomID | Restricts rule to a specific dining room or zone |
| LocationID | Restricts rule to a specific franchise location or site |
| DayOfWeek | Restricts rule to specific days |
| TimeRange | Restricts rule to specific time windows |
| ChannelID | Restricts rule to Dine-In, Carry-Out, Online, etc. |

Rules without scope filters apply universally.

---

## Post-Tax Adjustments

```
ApplyPostTax: boolean

If true  → adjustment is applied AFTER tax calculation
           (does not change the original tax liability)

If false → adjustment is applied BEFORE tax calculation
           (changes the taxable amount)
```

**Important:** Employee discounts and comps should typically be Post-Tax = false (reduces taxable amount). Surcharges that are not taxable should be Post-Tax = true.

---

## Common Rule Patterns

### Happy Hour
```
Rule: Force Price
Item: Draft Beer
Force Price: $3.00
DayOfWeek: Monday–Friday
TimeRange: 4:00 PM – 6:00 PM
RoomID: Bar only
Priority: 1 (highest)
```

### Employee Discount
```
Rule: Percentage Adjust
Value: -20%
ApplyPostTax: false
Trigger: Employee authorization code
Priority: 2
```

### Late Night Surcharge
```
Rule: Dollar Adjust
Value: +$1.00
TimeRange: 10:00 PM – close
ChannelID: Online, Carry-Out
ApplyPostTax: true
Priority: 3
```

### Extra Topping Multiplier
```
Rule: Multiplier
Context: "Extra" modifier selected
Value: 2.0x base ingredient cost
Item-scoped: true (applies only to the parent item's ingredient cost)
```

---

## Agent Instructions

**Nemotron:** When auditing, identify any item with more than 3 overlapping pricing rules. Flag conflicts where two Force Price rules could theoretically match the same item simultaneously — this is a configuration error since only the first rule fires. Check that all discount rules have ApplyPostTax set correctly.

**Ruffo:** When building pricing rules in script, always sort rules by specificity before writing them to the system. Most specific (narrowest scope) must be at the top of the stack.
