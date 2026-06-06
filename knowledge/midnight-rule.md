# The Midnight Rule — Heartland POS

## The Problem

Heartland calculates time ranges within a single calendar day. When a time range crosses midnight, the system interprets the end time as being earlier than the start time — which it treats as an invalid or zero-length range.

**Result:** Late-night specials, bar pricing, and after-hours items silently fail or behave unpredictably when configured as a single crossing range.

---

## The Rule

> Any pricing rule or item schedule whose active window crosses midnight MUST be split into two separate entries: one for the pre-midnight portion and one for the post-midnight portion.

---

## Example

**Wrong (single crossing range):**
```
Bar Special: 9:00 PM – 2:00 AM
→ System sees end time (2:00 AM) < start time (9:00 PM)
→ Rule fails silently or behaves unexpectedly
```

**Right (split entries):**
```
Bar Special — Part 1:
  Start: 9:00 PM
  End:   11:59 PM
  Day:   Saturday

Bar Special — Part 2:
  Start: 12:00 AM
  End:   2:00 AM
  Day:   Sunday  ← Note: the day increments after midnight
```

---

## Day Increment Rule

When splitting a midnight-crossing range, the post-midnight entry must be assigned to the NEXT calendar day.

| Original Range | Pre-Midnight Entry | Post-Midnight Entry |
|----------------|--------------------|---------------------|
| Fri 10PM – Sat 1AM | Fri: 10:00 PM – 11:59 PM | Sat: 12:00 AM – 1:00 AM |
| Sat 9PM – Sun 2AM | Sat: 9:00 PM – 11:59 PM | Sun: 12:00 AM – 2:00 AM |
| Daily 11PM – 3AM | Every day: 11:00 PM – 11:59 PM | Every day: 12:00 AM – 3:00 AM |

---

## Audit Check

The modifier-scan and pricing audit scripts automatically flag any time range where:
- `end_time < start_time` (uncorrected crossing range)
- `end_time == "12:00 AM"` with `start_time > "12:00 PM"` (likely meant to run past midnight)

These are reported as **MIDNIGHT RULE VIOLATIONS** in the client audit report.

---

## Agent Instructions

**Ruffo:** When writing any time range to the system via script, always check if end_time < start_time. If true, automatically split into two entries and assign the correct day to the post-midnight portion. Never write a crossing range as a single entry.

**Nemotron:** During audit, count all midnight rule violations and include them prominently in the audit report. These are common and clients are rarely aware they exist. Frame them as "silent failures" — the item appears to be configured but is not actually activating correctly.
