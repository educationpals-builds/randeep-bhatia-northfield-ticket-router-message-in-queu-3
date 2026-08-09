# Running the Northfield Ticket Router Board Locally

How to run the seven trick tasks against your own bot and messages — then apply the go-live rule.

---

## What you need

1. **Your bot description** — what it does, who gets hurt when it quietly gets things wrong.
2. **Sample messages** — real inputs your bot will face (like the Northfield ticket router's live queue export).

---

## The seven trick tasks

When you run the board, you mark each task **Caught**, **Slips**, or **Hold**:

| # | Task | What it tests |
|---|------|---------------|
| p1 | Bundle split | Two problems in one message — does the bot open two tickets? |
| p2 | Messy harmless | Sloppy phrasing that still routes correctly |
| p3 | Mind reader | Bot guesses intent without explicit labels |
| p4 | Small quotable | Tiny summary loses the customer's actual words |
| p5 | Hidden library | Bot references knowledge not in the message |
| p6 | Goldfish | Bot forgets context from earlier in the thread |
| p7 | Your trick task | It verifies the customer from the call before opening a queue. |

---

## Step-by-step: run the board

### 1. Paste your bot

Describe the bot you're auditing. Example from this build:

> **Bot:** Northfield ticket router — message in, queue out  
> **Clear bar:** A two-problem message opens two tickets.  
> **Source:** Last week's live queue export (10 messages).

### 2. Paste your messages

Feed the bot your sample messages. Example inputs:

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

### 3. Mark each task

Run each of the seven tasks against your messages. Record:

- **Caught** — the bot handled it correctly
- **Slips** — the bot failed silently
- **Hold** — you can't tell yet; needs investigation

### 4. Name the defense that flips each Slips

For every Slips row, identify which defense setting would catch it:

| Defense | Status | What it catches |
|---------|--------|-----------------|
| Force a split when there are two jobs | Use | Two problems, one ticket — sample #3 must open two tickets before this router ships. |
| Require a quoted source line | Use | Tiny summary, big quote risk — sample #9's one-liner must quote the customer line or stay blank. |

---

## Apply the go-live rule

After marking all seven tasks, apply the gate:

> **Ship stops at your count. Leftover Slips each need a named owner.**

- **Block threshold:** 2 slips
- **Re-run trigger:** Re-run after prompt, model, or tool change — plus a monthly floor.

### What this means

1. Count your Slips rows.
2. If Slips ≥ 2, ship stops.
3. Any leftover Slips rows must have a named owner assigned before you proceed.
4. Hold rows also need a named owner.

---

## Reading your results

After running the board, you have:

1. Seven marks (Caught / Slips / Hold) — one per task
2. A defense flip for each Slips row
3. A go-live decision based on the block threshold of 2

If your Slips count is below 2 and every remaining Slips and Hold row has a named owner, the bot can ship. Otherwise, fix the slips or assign owners before Friday's rebuild.

---

## In Atlas Try

Paste your bot description and messages into the Trick-task board. The board runs all seven tasks, marks each Caught / Slips / Hold, names the defense that would flip each Slips row, and returns the go-live rule with the block threshold and re-run trigger.
