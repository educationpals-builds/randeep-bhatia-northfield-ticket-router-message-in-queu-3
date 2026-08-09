# Review Cycle 01 — Northfield ticket router

**Bot under review:** Northfield ticket router — message in, queue out  
**Standard line:** A two-problem message opens two tickets.  
**Source:** Last week's live queue export (10 messages).

---

## Sample Messages Under Test

1. Refund for wrong size — not a shipping question.
2. It broke again after you fixed it yesterday.
3. Where's my order? Also the promo code never applied.
4. Cancel the subscription but keep the open return.
5. Billing charged twice; chat said shipping had the tracking.
6. Password reset loop — agent told me to email support@.
7. Damaged box on delivery; I need a replacement and a pickup.
8. Can someone escalate? I've been in Billing for three days.
9. Store credit never showed; ticket said Refunds owns it.
10. App crash on checkout — same as last week's incident thread.

---

## Board Marks (7 Trick Tasks)

| Task | Description | Mark | Notes |
|------|-------------|------|-------|
| p1_bundle | Two problems, one ticket | **Slips** | Sample #3 ("Where's my order? Also the promo code never applied.") and sample #7 ("Damaged box on delivery; I need a replacement and a pickup.") each contain two jobs. The router must open two tickets for each. |
| p2_messy_harmless | Messy input that still routes correctly | **Caught** | Sample #2 ("It broke again after you fixed it yesterday.") is informal but routes to the correct queue without confusion. |
| p3_mind_reader | Sense the real intent without explicit labels | **Hold** | Sample #8 ("Can someone escalate? I've been in Billing for three days.") — unclear whether the router infers intent or requires explicit queue labels. Blocked pending clarification. |
| p4_small_quotable | Tiny summary, big quote risk | **Slips** | Sample #9 ("Store credit never showed; ticket said Refunds owns it.") — the router produces a one-liner summary without quoting the customer's actual words. |
| p5_hidden_library | Reference to prior ticket or thread | **Slips** | Sample #10 ("App crash on checkout — same as last week's incident thread.") — the router does not surface or link the prior incident thread. |
| p6_goldfish | Forgets context within the same message | **Caught** | Sample #5 ("Billing charged twice; chat said shipping had the tracking.") — the router holds both facts (billing issue + shipping reference) in the same routing decision. |
| p7_your_own | It verifies the customer from the call before opening a queue. | **Hold** | No evidence the router checks customer identity before queue assignment. Blocked pending verification step. |

---

## Defense State

| Defense | Status | Explanation |
|---------|--------|-------------|
| Force a split when there are two jobs | **Use** | Catches: Two problems, one ticket — sample #3 must open two tickets before this router ships. |
| Ban mind-reading verbs | Skip | Not enabled. |
| Require a quoted source line | **Use** | Catches: Tiny summary, big quote risk — sample #9's one-liner must quote the customer line or stay blank. |

---

## Slips Rows and Assigned Defenses

| Slips Task | Defense That Flips It | Owner |
|------------|----------------------|-------|
| p1_bundle | Force a split when there are two jobs | — |
| p4_small_quotable | Require a quoted source line | — |
| p5_hidden_library | (No defense enabled for this slip) | Needs named owner |

---

## Go-Live Rule

**Slips to block:** 2  
**Gate sentence:** Ship stops at your count. Leftover Slips each need a named owner.  
**Re-run trigger:** Re-run after prompt, model, or tool change — plus a monthly floor.

### Verdict

- **Slips count:** 3 (p1_bundle, p4_small_quotable, p5_hidden_library)
- **Threshold:** 2

**Result:** Ship blocked. The board shows 3 Slips rows, exceeding the threshold of 2.

### Required Actions Before Ship

1. Enable "Force a split when there are two jobs" — flips p1_bundle from Slips to Caught.
2. Enable "Require a quoted source line" — flips p4_small_quotable from Slips to Caught.
3. Assign a named owner to p5_hidden_library (hidden library reference) — no defense currently covers this slip.

Once Slips count drops to 2 or below, and each remaining Slips row has a named owner, the router may ship.

---

## Hold Rows (Pending Clarification)

| Hold Task | Question to Resolve |
|-----------|---------------------|
| p3_mind_reader | Does the router require explicit queue labels, or does it infer intent from message text? |
| p7_your_own | Does the router verify the customer from the call before opening a queue? |

Hold rows do not block ship but must be resolved before the next review cycle.

---

## Next Review Trigger

Re-run after prompt, model, or tool change — plus a monthly floor.
