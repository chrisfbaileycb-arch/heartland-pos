# Heartland POS Audit Report
**Signal F Holdings LLC — Confidential**

---

**Client:** [Restaurant Name]  
**Location:** [Address]  
**Date of Audit:** [Date]  
**Conducted by:** Christopher Bailey, Signal F Holdings LLC  
**System:** Heartland Restaurant POS  

---

## Executive Summary

> [2–3 sentence plain-English summary of what was found.
> Example: "Your current menu configuration has 14 modifier cross-contamination issues. The most critical is the 'pepperoni' modifier, which if disabled would affect 22 separate menu items across 4 categories. This report details each issue and proposes a path to resolution."]

---

## Section 1 — Modifier Cross-Contamination

### What This Means
A modifier is "cross-contaminated" when the same entry is shared across multiple menu categories. This creates a dangerous situation: disabling one ingredient accidentally disables it everywhere it appears, even in unrelated dishes.

### Findings

| Severity | Modifier | Categories Affected | Items at Risk |
|----------|----------|---------------------|---------------|
| HIGH | [modifier name] | [list] | [count] |
| MEDIUM | [modifier name] | [list] | [count] |

### 86 Risk Summary
[List the top 3 most dangerous modifiers to disable and what would break.]

**Recommendation:**  
Rebuild modifier groups so each category has its own isolated copy. Set Is Shared = false on all ingredient-type modifiers.

---

## Section 2 — Redundant Data Entry

### Findings
The following modifiers appear to have been entered manually multiple times instead of configured as a proper group:

| Modifier | Times Entered | Maintenance Risk |
|----------|--------------|-----------------|
| [name] | [count] | [description] |

**Recommendation:**  
Consolidate to one properly configured modifier group per ingredient type.

---

## Section 3 — Midnight Rule Violations

### What This Means
Heartland calculates time ranges within a single calendar day. Time ranges that cross midnight (e.g., 10 PM – 2 AM) must be split into two entries or they fail silently — the pricing rule or item appears configured but is not actually activating.

### Findings

| Item / Rule | Configured Range | Status |
|-------------|-----------------|--------|
| [name] | [range] | SILENT FAILURE |

**Recommendation:**  
Split each crossing range into two entries. See knowledge/midnight-rule.md for the exact correction procedure.

---

## Section 4 — Duplicate Items

| Item Name | Occurrences | Notes |
|-----------|------------|-------|
| [name] | [count] | [intentional or error] |

---

## Section 5 — Overall Assessment

**[ ] Quick Fix** — Fewer than 10 issues, targeted corrections only.  
**[ ] Targeted Rebuild** — Specific categories need restructuring.  
**[ ] Full Rebuild Recommended** — Systemic modifier architecture issues throughout.  

---

## Proposed Scope of Work

[If Full Rebuild is recommended, describe what would be done:]

A shadow build will be created — complete parallel versions of each ordering channel:
- **Dine-In v2** (runs alongside current Dine-In)
- **Carry-Out v2** (runs alongside current Carry-Out)
- **Pick-Up v2** (runs alongside current Pick-Up)
- **Online v2** (runs alongside current Online)

**Your current live system remains fully operational throughout the entire process.**

You will be able to verify each v2 channel before any changes go live. You control when — and if — you switch over. There is no mandatory cutover date.

---

## Authorization

By signing below, you authorize Signal F Holdings LLC to proceed with the scope of work described in Section 5.

**Client Signature:** _________________________ **Date:** ___________  
**Print Name:** _________________________  

---

*This report was generated using the Signal F Holdings LLC Heartland POS Audit Framework.*  
*All findings are based on a read-only scan of your current menu configuration.*  
*No changes have been made to your live system.*
