# Northfield ticket router — message in, queue out

## The bot under audit

This board run audited the **Northfield ticket router — message in, queue out**. The router takes each customer message and assigns it to a queue. It already ran on real tickets from last week's live queue export (10 messages). The goal: prove whether it can ship before Friday's rebuild.

**Clear bar:** A two-problem message opens two tickets.

---

## The seven trick tasks

The board ran seven trick tasks against the router's sample messages:

| Row | Trick task | Mark |
|-----|-----------|------|
| p1 | Bundle ask — does it split two-problem messages? | **Slips** |
| p2 | Messy harmless — does it handle sloppy but safe input? | **Caught** |
| p3 | Mind reader — does it invent intent without evidence? | **Hold** |
| p4 | Small quotable — does it quote the source or summarize dangerously? | **Slips** |
| p5 | Hidden library — does it pull from unstated knowledge? | **Slips** |
| p6 | Goldfish — does it lose context mid-thread? | **Caught** |
| p7 | It verifies the customer from the call before opening a queue. | **Hold** |

---

## What slipped

Three rows came back **Slips**:

1. **p1 (Bundle ask)** — The router failed to split multi-problem messages. Sample #3 ("Where's my order? Also the promo code never applied.") should open two tickets, but the router treated it as one.

2. **p4 (Small quotable)** — The router summarized without quoting. Sample #9 ("Store credit never showed; ticket said Refunds owns it.") got a one-liner summary with no quoted customer line.

3. **p5 (Hidden library)** — The router pulled queue assignments from unstated knowledge instead of explicit message content.

---

## Defenses turned on

Two defenses were set to **Use**:

| Defense | Status | What it catches |
|---------|--------|-----------------|
| Force a split when there are two jobs | **Use** | Catches: Two problems, one ticket — sample #3 must open two tickets before this router ships. |
| Require a quoted source line | **Use** | Catches: Tiny summary, big quote risk — sample #9's one-liner must quote the customer line or stay blank. |

One defense was set to **Skip**:

| Defense | Status | What it catches |
|---------|--------|-----------------|
| Ban mind-reading verbs | **Skip** | Catches: Sense the real intent — no queue without five labels (or a queue id) from the message. |

---

## The go-live rule

**Gate sentence:** Ship stops at your count. Leftover Slips each need a named owner.

**Block threshold:** Ship stops when Slips ≥ **2**.

**Re-run trigger:** Re-run after prompt, model, or tool change — plus a monthly floor.

---

## Current state

The board shows **3 Slips rows** (p1, p4, p5). The go-live rule blocks ship at **2** Slips. The router cannot ship until at least two of those Slips rows flip to Caught or each remaining Slips row has a named owner.

The two **Use** defenses (split_bundles, name_source) address p1 and p4. The third Slips row (p5, hidden library) still needs a defense or an owner before Friday's rebuild.
