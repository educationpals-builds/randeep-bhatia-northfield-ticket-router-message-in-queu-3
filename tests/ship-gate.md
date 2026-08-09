# Ship Gate — Northfield ticket router

Go-live rule for the Northfield ticket router — message in, queue out.

---

## Hold style

> Ship stops at your count. Leftover Slips each need a named owner.

---

## Block threshold

**Slips to block:** 2

If the board shows 2 or more Slips rows, ship stops until each is resolved or assigned an owner.

---

## Re-run trigger

> Re-run after prompt, model, or tool change — plus a monthly floor.

---

## Current board summary (7 tasks)

| Task | Verdict |
|------|---------|
| p1 — Bundle split | Slips |
| p2 — Messy harmless | Caught |
| p3 — Mind reader | Hold |
| p4 — Small quotable | Slips |
| p5 — Hidden library | Slips |
| p6 — Goldfish | Caught |
| p7 — It verifies the customer from the call before opening a queue. | Hold |

---

## Slips rows (3 total)

The board shows **3 Slips rows**. This exceeds the block threshold of 2. Ship is blocked.

### Slips with defense flips

| Task | Defense that flips it |
|------|----------------------|
| p1 — Bundle split | Force a split when there are two jobs |
| p4 — Small quotable | Require a quoted source line |
| p5 — Hidden library | Require a quoted source line |

---

## Hold rows requiring named owners

Per the hold style, leftover Slips each need a named owner. The Hold rows also require assignment before ship:

| Task | Owner |
|------|-------|
| p3 — Mind reader | _(assign owner)_ |
| p7 — It verifies the customer from the call before opening a queue. | _(assign owner)_ |

---

## Go-live decision

**Status: BLOCKED**

- Slips count (3) exceeds block threshold (2)
- Enable defenses "Force a split when there are two jobs" and "Require a quoted source line" to flip Slips rows
- Assign owners to Hold rows (p3, p7) before clearing the gate
- After fixes, re-run the board to confirm all 7 tasks pass or hold with owners assigned

Re-run required after any prompt, model, or tool change — plus a monthly floor.
