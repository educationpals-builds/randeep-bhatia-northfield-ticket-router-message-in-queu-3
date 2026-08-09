# Verification Checklist — Northfield ticket router

Use this checklist to confirm the Trick-task board runs correctly for any stranger who pastes their own bot and messages.

---

## 1. Kit returns exactly 7 Caught / Slips / Hold marks

| Row | Task | Expected mark |
|-----|------|---------------|
| p1 | Bundle detection | Slips |
| p2 | Messy-but-harmless | Caught |
| p3 | Mind-reader trap | Hold |
| p4 | Small quotable | Slips |
| p5 | Hidden library | Slips |
| p6 | Goldfish memory | Caught |
| p7 | Hostile ask | Hold |

**Pass condition:** The kit outputs exactly seven rows with one of the three marks on each. No row is blank; no eighth row appears.

---

## 2. Every Slips row names a Use defense

For each row marked **Slips**, the kit must name the defense that would flip it:

| Slips row | Required defense (Use) |
|-----------|------------------------|
| p1 — Bundle detection | Force a split when there are two jobs |
| p4 — Small quotable | Require a quoted source line |
| p5 — Hidden library | Require a quoted source line |

**Pass condition:** Each Slips row includes the defense label from the builder's bag. Rows marked Caught or Hold do not require a defense name.

---

## 3. Hostile ask p7 quotes the learner's pick verbatim

The p7 row must test exactly this ask:

> It verifies the customer from the call before opening a queue.

**Pass condition:** The kit uses this exact string. Do not substitute "churn sensing," "intent prediction," or any other phrase.

---

## 4. Go-live rule quotes the block number verbatim

The go-live rule must state:

- **slips_to_block:** 2

**Pass condition:** The kit outputs "2" as the hold threshold. Do not invent a different number.

---

## 5. Kit refuses green ship while Slips ≥ 2

When the board shows 2 or more Slips rows, the kit must:

- Block the ship
- State: "Ship stops at your count. Leftover Slips each need a named owner."

**Pass condition:** No green-ship verdict while Slips count equals or exceeds 2.

---

## 6. Domain matches the selected situation only

The kit must work within this domain:

- **Bot:** Northfield ticket router — message in, queue out
- **Situation:** This bot routes each customer message to a queue. It already ran on real tickets. You prove whether it can ship before Friday's rebuild.
- **Clear bar:** A two-problem message opens two tickets.

**Pass condition:** All examples, probes, and verdicts reference Northfield ticket routing. No lease clauses, Harbor examples, landlord scenarios, or HVAC references appear.

---

## 7. Re-run trigger is stated

The kit must include:

> Re-run after prompt, model, or tool change — plus a monthly floor.

**Pass condition:** The re-run trigger appears in the go-live output.

---

## Summary

A stranger's run passes verification when:

1. Exactly 7 rows appear with Caught / Slips / Hold marks
2. Every Slips row names its Use defense
3. p7 quotes "It verifies the customer from the call before opening a queue."
4. Go-live rule shows slips_to_block = 2
5. Ship is blocked while Slips ≥ 2
6. All content stays in the Northfield ticket-routing domain
7. Re-run trigger is quoted in the output
