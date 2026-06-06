# Coursing & Kitchen Flow — Heartland POS

## Core Concept

Coursing controls the order in which items are sent to the kitchen and when they are prepared relative to other items in the same ticket.

---

## Key Attributes

| Attribute | Values | Notes |
|-----------|--------|-------|
| Course Number | 0–9 | Default is 0 (unassigned). Lower numbers fire first. |
| Rush Flag | Boolean | Bypasses normal course sequence. Item fires immediately. |
| Hold Flag | Boolean | Delays firing until manually released by staff. |
| Priority | High / Normal / Low | Affects kitchen queue order within the same course. |

---

## Configuration Rules

### Default Course Assignment
- Every item defaults to Course 0 (unassigned) unless explicitly set
- Staff can manually override course assignment on a per-ticket basis
- Course assignments are suggestions to the kitchen, not hard locks

### Rush Override
- Setting Rush = true fires the item to the kitchen immediately
- Bypasses any Hold flags on the same item
- Use sparingly — overuse degrades kitchen flow

### Hold Logic
- Hold = true delays the item until staff manually releases it
- Useful for items that should fire after a specific event (e.g., after appetizers are cleared)
- Hold takes priority over course number but not over Rush

### Priority Within a Course
- When multiple items share the same course number, Priority determines queue order
- High priority items appear at the top of the kitchen display
- Default is Normal for all items

---

## Practical Notes for Menu Build

- Set explicit course numbers for full-service restaurants (appetizers = 1, entrees = 2, desserts = 3)
- Quick-service and carry-out builds typically leave everything at Course 0
- Do not use Rush as a substitute for proper course configuration
- Hold is most useful for premium tasting-menu style operations

---

## Agent Instruction (Nemotron)

When auditing a menu, flag any item with Course Number = 0 that appears to be an entree or appetizer — these likely need explicit course assignment. Flag any item with both Rush and Hold set simultaneously as a configuration error.
