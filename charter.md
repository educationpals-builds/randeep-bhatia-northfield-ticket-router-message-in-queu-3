# Charter: Trick-task board

## Who this serves

Teams shipping a bot that routes customer messages to queues. Before go-live, someone must prove the router handles edge cases — bundled problems, vague requests, missing context. This board gives that person seven trick tasks to run against real messages, a mark for each, and a rule that blocks ship until the count clears.

**Worked example:** Northfield ticket router — message in, queue out

---

## The clear bar

> A two-problem message opens two tickets.

If the router can't meet this bar on the sample messages, it doesn't ship.

---

## What the three marks mean

| Mark | Meaning |
|------|---------|
| **Caught** | The router handled this trick task correctly on the sample messages. No action needed. |
| **Slips** | The router failed this trick task. A defense must flip it before ship. |
| **Hold** | The trick task can't be tested yet — blocked by missing data, unclear scope, or dependency on another system. |

---

## The seven trick tasks

1. **p1 — Bundle split** → Does the router open separate tickets when one message contains two problems?  
2. **p2 — Messy harmless** → Does the router handle sloppy but routine messages without breaking?  
3. **p3 — Mind reader** → Does the router avoid guessing intent when the message doesn't state it?  
4. **p4 — Small quotable** → Does the router preserve the customer's words instead of summarizing them away?  
5. **p5 — Hidden library** → Does the router catch references to prior tickets or threads?  
6. **p6 — Goldfish** → Does the router handle repeated issues without losing context?  
7. **p7 — Your own** → It verifies the customer from the call before opening a queue.

---

## Specimen messages tested

Source: Last week's live queue export (10 messages).

- Refund for wrong size — not a shipping question.
- It broke again after you fixed it yesterday.
- Where's my order? Also the promo code never applied.
- Cancel the subscription but keep the open return.
- Billing charged twice; chat said shipping had the tracking.
- Password reset loop — agent told me to email support@.
- Damaged box on delivery; I need a replacement and a pickup.
- Can someone escalate? I've been in Billing for three days.
- Store credit never showed; ticket said Refunds owns it.
- App crash on checkout — same as last week's incident thread.

---

## Go-live commitment

**Gate sentence:** Ship stops at your count. Leftover Slips each need a named owner.

**Block threshold:** Ship stops when Slips ≥ 2.

**Re-run trigger:** Re-run after prompt, model, or tool change — plus a monthly floor.

---

## Defenses available

| Defense | Status | What it catches |
|---------|--------|-----------------|
| Force a split when there are two jobs | **Use** | Two problems, one ticket — sample #3 must open two tickets before this router ships. |
| Ban mind-reading verbs | Skip | Sense the real intent — no queue without five labels (or a queue id) from the message. |
| Require a quoted source line | **Use** | Tiny summary, big quote risk — sample #9's one-liner must quote the customer line or stay blank.

---

## Commitment

This board runs before every ship decision. If Slips ≥ 2, the router does not go live. Each remaining Slips row names an owner who will fix it or accept the risk in writing.
