# Trick-task board — Northfield ticket router

Seven trick tasks run against the Northfield ticket router — message in, queue out. Each row shows the test message, what the bot did, the verdict for this run, and the defense that flips any Slips.

**Standard:** A two-problem message opens two tickets.

**Source:** Last week's live queue export (10 messages).

---

## p1 · Bundle trap

**Task:** Does the router split a message with two problems into two tickets?

**Test message:**  
> Where's my order? Also the promo code never applied.

**What the bot did:** Routed to a single queue (Order Status) without opening a second ticket for the promo code issue.

**Verdict:** Slips

**Use defense to flip:** Force a split when there are two jobs — Catches: Two problems, one ticket — sample #3 must open two tickets before this router ships.

---

## p2 · Messy-but-harmless trap

**Task:** Does the router handle a messy message that still has a clear single queue?

**Test message:**  
> Refund for wrong size — not a shipping question.

**What the bot did:** Routed to Refunds queue correctly despite the extra clarification text.

**Verdict:** Caught

---

## p3 · Mind-reader trap

**Task:** Does the router infer intent without explicit labels from the message?

**Test message:**  
> Can someone escalate? I've been in Billing for three days.

**What the bot did:** Blocked — router attempted to infer escalation path without five labels (or a queue id) from the message.

**Verdict:** Hold

**Owner required:** Assign owner before ship.

---

## p4 · Small-quotable trap

**Task:** Does the router preserve the customer's exact words when summarizing?

**Test message:**  
> Store credit never showed; ticket said Refunds owns it.

**What the bot did:** Summarized as "credit issue" without quoting the customer line.

**Verdict:** Slips

**Use defense to flip:** Require a quoted source line — Catches: Tiny summary, big quote risk — sample #9's one-liner must quote the customer line or stay blank.

---

## p5 · Hidden-library trap

**Task:** Does the router reference prior context it shouldn't have access to?

**Test message:**  
> App crash on checkout — same as last week's incident thread.

**What the bot did:** Referenced "last week's incident thread" as if it had access to historical ticket data, routing based on assumed context.

**Verdict:** Slips

**Use defense to flip:** Require a quoted source line — Catches: Tiny summary, big quote risk — sample #9's one-liner must quote the customer line or stay blank.

---

## p6 · Goldfish trap

**Task:** Does the router handle a message that references a prior interaction?

**Test message:**  
> It broke again after you fixed it yesterday.

**What the bot did:** Routed to Repairs queue without assuming access to yesterday's ticket — treated as new repair request.

**Verdict:** Caught

---

## p7 · Your own trick task

**Task:** It verifies the customer from the call before opening a queue.

**Test message:**  
> Password reset loop — agent told me to email support@.

**What the bot did:** Blocked — router cannot verify customer identity from the call before opening a queue.

**Verdict:** Hold

**Owner required:** Assign owner before ship.

---

## Summary

| Task | Verdict | Defense (if Slips) |
|------|---------|-------------------|
| p1 · Bundle trap | Slips | Force a split when there are two jobs |
| p2 · Messy-but-harmless trap | Caught | — |
| p3 · Mind-reader trap | Hold | Owner required |
| p4 · Small-quotable trap | Slips | Require a quoted source line |
| p5 · Hidden-library trap | Slips | Require a quoted source line |
| p6 · Goldfish trap | Caught | — |
| p7 · Your own trick task | Hold | Owner required |

**Slips count this run:** 3  
**Hold count this run:** 2  
**Caught count this run:** 2
