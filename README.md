# Trick-task board

A stranger describes the bot they're about to trust — what it does, who gets hurt when it quietly gets things wrong, and a few real messages it will face. The kit runs seven trick tasks against those messages, marks each **Caught / Slips / Hold**, names the Use defense that would flip each Slips row, and returns a go-live rule quoting the Slips-to-block number and the re-run trigger.

---

## Worked example

**Bot:** Northfield ticket router — message in, queue out

**Clear bar:** A two-problem message opens two tickets.

**Source:** Last week's live queue export (10 messages).

**Sample messages:**

> Refund for wrong size — not a shipping question.  
> It broke again after you fixed it yesterday.  
> Where's my order? Also the promo code never applied.  
> Cancel the subscription but keep the open return.  
> Billing charged twice; chat said shipping had the tracking.  
> Password reset loop — agent told me to email support@.  
> Damaged box on delivery; I need a replacement and a pickup.  
> Can someone escalate? I've been in Billing for three days.  
> Store credit never showed; ticket said Refunds owns it.  
> App crash on checkout — same as last week's incident thread.

---

## The seven trick tasks

| Row | Trick task | Mark | Note |
|-----|------------|------|------|
| p1 | **Bundle split** — Does the router open two tickets when a message holds two problems? ("Where's my order? Also the promo code never applied.") | **Slips** | Two problems, one ticket — sample #3 must open two tickets before this router ships. |
| p2 | **Messy harmless** — Does the router handle a rambling but single-issue message without inventing extra queues? | **Caught** | |
| p3 | **Mind reader** — Does the router guess intent without explicit labels? ("Can someone escalate? I've been in Billing for three days.") | **Hold** | Sense the real intent — no queue without five labels (or a queue id) from the message. |
| p4 | **Small quotable** — Does the router summarize without quoting the customer's own words? ("Store credit never showed; ticket said Refunds owns it.") | **Slips** | Tiny summary, big quote risk — sample #9's one-liner must quote the customer line or stay blank. |
| p5 | **Hidden library** — Does the router rely on knowledge not in the message or its metadata? | **Slips** | |
| p6 | **Goldfish** — Does the router forget context from earlier in the same thread? | **Caught** | |
| p7 | **Your own trick** — It verifies the customer from the call before opening a queue. | **Hold** | |

---

## Defenses that catch Slips

| Defense | Status | What it catches |
|---------|--------|-----------------|
| **Force a split when there are two jobs** | **Use** | Catches: Two problems, one ticket — sample #3 must open two tickets before this router ships. |
| **Ban mind-reading verbs** | Skip | Catches: Sense the real intent — no queue without five labels (or a queue id) from the message. |
| **Require a quoted source line** | **Use** | Catches: Tiny summary, big quote risk — sample #9's one-liner must quote the customer line or stay blank. |

---

## Go-live rule

**Slips to block:** 2

**Gate sentence:** Ship stops at your count. Leftover Slips each need a named owner.

**Re-run trigger:** Re-run after prompt, model, or tool change — plus a monthly floor.

The Northfield ticket router cannot ship while 2 or more Slips rows remain open. Each leftover Slips row must name an owner before launch.

---

## One-paste rebuild

```text
Bot: Northfield ticket router — message in, queue out
Clear bar: A two-problem message opens two tickets.
Source: Last week's live queue export (10 messages).

Sample messages:
Refund for wrong size — not a shipping question.
It broke again after you fixed it yesterday.
Where's my order? Also the promo code never applied.
Cancel the subscription but keep the open return.
Billing charged twice; chat said shipping had the tracking.
Password reset loop — agent told me to email support@.
Damaged box on delivery; I need a replacement and a pickup.
Can someone escalate? I've been in Billing for three days.
Store credit never showed; ticket said Refunds owns it.
App crash on checkout — same as last week's incident thread.

Defenses (Use):
- Force a split when there are two jobs
- Require a quoted source line

Go-live rule:
- Slips to block: 2
- Re-run trigger: Re-run after prompt, model, or tool change — plus a monthly floor.
```

Paste your own bot, clear bar, sample messages, and defenses. The board returns seven Caught/Slips/Hold marks, names the defense that would flip each Slips, and applies your go-live rule.

<!-- educationpals-build-verified -->
