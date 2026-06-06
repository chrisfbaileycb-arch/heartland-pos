# Temporal Logic — Heartland POS

## Core Concept

Temporal logic controls when menu items, pricing rules, and modifier groups are active. Items can be scheduled to appear and disappear automatically based on date, day of week, and time of day.

---

## Key Attributes

| Attribute | Type | Description |
|-----------|------|-------------|
| StartDate | Date (YYYY-MM-DD) | When the item/rule becomes active |
| EndDate | Date (YYYY-MM-DD) | When the item/rule expires |
| DayOfWeek | Array | Which days the rule is active [Mon, Tue, Wed...] |
| TimeRanges | Array of ranges | One or more start/end time windows |
| IsRecurring | Boolean | Whether the schedule repeats or is one-time only |

---

## Multi-Part Time Ranges

A single item or rule can have multiple time windows in the same day.

**Example: Lunch AND Dinner special (not available in between)**
```
TimeRanges: [
  { start: "11:00 AM", end: "2:00 PM" },
  { start: "5:00 PM", end: "9:00 PM" }
]
```

This is preferable to two separate items. Use multi-part ranges to keep the menu clean.

---

## Seasonal Items

Use StartDate and EndDate for limited-time offerings:
```
Item: Pumpkin Spice Latte
StartDate: 2024-09-01
EndDate: 2024-11-30
IsRecurring: false
```

For annually recurring items, set IsRecurring = true and the system will reactivate on the same dates each year.

---

## Day-of-Week Scheduling

```
Item: Fish Fry Special
DayOfWeek: [Friday]
TimeRange: 11:00 AM – 9:00 PM
```

Items not active on the current day are hidden from the ordering interface automatically. No manual enabling/disabling required.

---

## The Midnight Rule

See `midnight-rule.md` for full detail.

**Summary:** Any time range that crosses midnight (e.g., 10:00 PM – 2:00 AM) must be split into two separate entries to avoid system miscalculation:

```
Entry 1: 10:00 PM – 11:59 PM  (Day 1)
Entry 2: 12:00 AM – 2:00 AM   (Day 2)
```

---

## Agent Instructions

**Nemotron:** When auditing, flag any TimeRange with an end time earlier than its start time — this almost always indicates an uncorrected midnight-crossing range. Flag any item with StartDate set but no EndDate as a potential "forgotten special" that may be running indefinitely.

**Gemini:** When writing integration specs for DoorDash or online menu boards, note that external platforms do not always support multi-part time ranges natively. Document any items that require manual scheduling on the third-party platform side.
