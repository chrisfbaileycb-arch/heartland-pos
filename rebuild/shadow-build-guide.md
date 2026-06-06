# Shadow Build Methodology
**Signal F Holdings LLC — Heartland POS**

---

## Core Principle

> A client's live revenue stream is never interrupted. Ever.
> The rebuilt menu runs in parallel as a "v2" channel until the client
> is fully confident and chooses to switch on their own terms.

---

## What Is a Shadow Build?

A shadow build creates complete duplicate versions of every ordering channel inside the same Heartland system. The current live channels keep processing orders normally while you build, test, and refine the v2 versions alongside them.

```
LIVE (untouched, processing orders)     SHADOW (being built, safe to test)
────────────────────────────────────    ──────────────────────────────────
Dine-In        [LIVE]              →    Dine-In v2      [BUILDING]
Carry-Out      [LIVE]              →    Carry-Out v2    [BUILDING]
Pick-Up        [LIVE]              →    Pick-Up v2      [BUILDING]
Online         [LIVE]              →    Online v2       [BUILDING]
```

No customer ever sees a v2 channel until the client explicitly activates it.

---

## Phase 1 — Pre-Build Checklist

Before creating any v2 channel, confirm:

- [ ] Audit report has been delivered and reviewed by client
- [ ] Client has signed the authorization form (audit/report-template.md)
- [ ] Scope of work is agreed in writing (Quick Fix / Targeted / Full Rebuild)
- [ ] You have admin menu access credentials for this client
- [ ] You have logged into the portal and confirmed access is active
- [ ] A clean export of the current menu has been saved to `exports/` folder

---

## Phase 2 — Channel Creation

### Naming Convention
All shadow channels follow this exact naming pattern:
```
[Original Channel Name] v2
```

Examples:
- `Dine-In` → `Dine-In v2`
- `Carry-Out` → `Carry-Out v2`
- `Online Ordering` → `Online Ordering v2`
- `Bar Tab` → `Bar Tab v2`

**Why v2 and not "New" or "Test"?**
"v2" communicates to the client that this is a professional versioned build, not an experiment. It also makes the cutover conversation natural: "When you're ready, we activate v2 and retire v1."

### Build Order
Build channels in this sequence — lowest risk to highest:
1. Carry-Out v2 (least complex, good for validating modifier structure)
2. Pick-Up v2 (similar to Carry-Out, quick to verify)
3. Online v2 (validates that modifiers display correctly on ordering platform)
4. Dine-In v2 (most complex — coursing, table routing, kitchen flow)
5. Bar / Specialty channels last (temporal logic, happy hour pricing)

---

## Phase 3 — Build Standards

### Modifier Architecture
Apply Signal F Holdings modifier isolation standard to ALL v2 channels:
- Every ingredient-type modifier: Is Shared = false
- Every size/preparation modifier: evaluate for shared status individually
- No modifier group crosses category boundaries without explicit client instruction
- Run modifier-scan.py against each v2 channel after build to verify zero violations

### Pricing Rules
- Verify all pricing rules have correct ApplyPostTax values
- Check all dynamic rules for priority conflicts
- Run midnight rule check on every time-range-based rule

### Coursing (Dine-In v2 only)
- Assign explicit course numbers to appetizers (1), entrees (2), desserts (3)
- Verify Rush and Hold flags are not set simultaneously on any item
- Confirm kitchen routing is correct for the dining room layout

---

## Phase 4 — Client Verification

When a v2 channel is ready for client review:

1. Give the client access to the v2 channel (Heartland staff preview mode)
2. Walk them through each category and modifier group
3. Have them place test orders on v2 — verify pricing, modifiers, and output
4. Collect any correction requests in writing
5. Apply corrections to v2 only — never touch v1 during this phase
6. Repeat until client signs off on that channel

**Verification checklist per channel:**
- [ ] All menu items present and correctly named
- [ ] All modifier groups correct and item-scoped
- [ ] Pricing accurate across all items
- [ ] Time-based rules firing correctly
- [ ] Midnight rules split correctly
- [ ] Test order placed and verified end-to-end

---

## Phase 5 — Cutover Instructions

See `cutover-instructions.md` for the full client-facing guide.

**Summary:**
- Client activates v2 channels themselves
- v1 channels remain available as fallback for minimum 2 weeks
- Client retires v1 channels when fully confident
- Signal F Holdings available for support during transition period

---

## Phase 6 — Post-Cutover

After client has been live on v2 for 2 weeks with no issues:
- Archive the export of the original v1 menu to `exports/[client]-v1-archive.json`
- Update this repo with any client-specific lessons learned
- v1 channels can be retired by the client at their discretion
