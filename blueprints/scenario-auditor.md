# Northfield ticket router — Trick-task board blueprint

This blueprint runs the seven-row board against any bot that routes customer messages to queues. A stranger pastes their bot description, their stakes, and a few real messages. The board returns Caught / Slips / Hold for each trick task, names the defense that would flip each Slips, and applies the go-live rule.

---

## Intake paste shape

The stranger provides:

1. **Bot name and job** — what the bot does (e.g., "routes each customer message to a queue")
2. **Stakes** — who gets hurt when it quietly gets things wrong
3. **Sample messages** — 5–10 real messages the bot will face

---

## The seven trick tasks

| Row | Task | Question |
|-----|------|----------|
| p1 | Bundle split | Does a two-problem message open two tickets? |
| p2 | Messy but harmless | Does the bot handle messy formatting without breaking? |
| p3 | Mind reader | Does the bot infer intent without explicit labels? |
| p4 | Small quotable | Does the bot quote the customer line or stay blank? |
| p5 | Hidden library | Does the bot rely on undocumented routing rules? |
| p6 | Goldfish | Does the bot remember context from prior messages? |
| p7 | Your own | It verifies the customer from the call before opening a queue. |

---

## Worked example: Northfield ticket router

**Bot:** Northfield ticket router — message in, queue out

**Standard line:** A two-problem message opens two tickets.

**Source:** Last week's live queue export (10 messages).

**Sample messages:**

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

## Board marks for Northfield ticket router

| Row | Task | Verdict | Note |
|-----|------|---------|------|
| p1 | Bundle split | **Slips** | Message #3 ("Where's my order? Also the promo code never applied.") contains two problems but router may open one ticket. |
| p2 | Messy but harmless | **Caught** | Router handles messy formatting without breaking. |
| p3 | Mind reader | **Hold** | Cannot verify — need to see if router infers intent without explicit labels. |
| p4 | Small quotable | **Slips** | Message #9 ("Store credit never showed; ticket said Refunds owns it.") is a one-liner that must quote the customer line or stay blank. |
| p5 | Hidden library | **Slips** | Router may rely on undocumented routing rules for queue assignment. |
| p6 | Goldfish | **Caught** | Router handles context from prior messages (message #2 references "yesterday"). |
| p7 | Your own | **Hold** | It verifies the customer from the call before opening a queue. — Cannot verify without call integration details. |

---

## Caught / Slips / Hold chips

- **Caught** — The bot handles this task correctly. No action needed.
- **Slips** — The bot fails this task. Name the defense that would flip it.
- **Hold** — Cannot verify. Need more information or access to test.

---

## Use defenses

When a row marks **Slips**, name the defense that would flip it:

| Defense | Status | Catches |
|---------|--------|---------|
| Force a split when there are two jobs | **on** | Two problems, one ticket — sample #3 must open two tickets before this router ships. |
| Ban mind-reading verbs | off | Sense the real intent — no queue without five labels (or a queue id) from the message. |
| Require a quoted source line | **on** | Tiny summary, big quote risk — sample #9's one-liner must quote the customer line or stay blank. |

**Slips → Defense mapping for Northfield:**

- p1 (Bundle split) Slips → Use: **Force a split when there are two jobs**
- p4 (Small quotable) Slips → Use: **Require a quoted source line**
- p5 (Hidden library) Slips → No matching defense currently on; needs named owner

---

## Go-live gate

**Gate rule:** Ship stops at your count. Leftover Slips each need a named owner.

**Slips to block:** 2

**Current Slips count:** 3 (p1, p4, p5)

**Verdict:** Ship blocked. 3 Slips exceeds the threshold of 2.

**Leftover Slips needing named owner:**
- p5 (Hidden library) — no defense currently flips this; assign owner before ship

**Re-run trigger:** Re-run after prompt, model, or tool change — plus a monthly floor.

---

## Running the board on a stranger's bot

1. Stranger pastes bot name, stakes, and sample messages
2. Walk each of the seven trick tasks against their messages
3. Mark each row Caught / Slips / Hold
4. For each Slips, name the defense that would flip it
5. Count total Slips
6. If Slips ≥ 2, ship stops
7. Any Slips without a matching defense needs a named owner
8. Return the board with marks, defenses, and go-live verdict
