# Method: Trick-task Board (GOVERN)

This method audits whether a bot's checks actually split the work before you ship it.

---

## The Seven Board Rows

The Trick-task Board runs seven probes against your bot's real messages. Each row gets one mark.

| Row | Probe | What it tests |
|-----|-------|---------------|
| p1 | Bundle trap | Does the bot split a message with two problems into two tickets? |
| p2 | Messy harmless | Does the bot handle sloppy but safe input without breaking? |
| p3 | Mind reader | Does the bot invent intent the message never stated? |
| p4 | Small quotable | Does the bot quote the customer's words or silently summarize? |
| p5 | Hidden library | Does the bot pull from sources it never names? |
| p6 | Goldfish | Does the bot lose context from earlier in the same thread? |
| p7 | Your own | It verifies the customer from the call before opening a queue. |

---

## The Three Marks

Every row receives exactly one mark:

### Caught
The bot handled this probe correctly. The check worked. No action needed.

### Slips
The bot failed this probe. The check did not catch the trick. A defense must flip this row before ship.

### Hold
The probe could not run — blocked by missing data, unclear scope, or a dependency. Resolve the block, then re-run.

---

## Defenses: Use and Skip

Each Slips row names a defense that would flip it to Caught.

### Use
Turn this defense on. The bot's behavior changes to catch the slip.

**Force a split when there are two jobs**
Catches: Two problems, one ticket — sample #3 must open two tickets before this router ships.

**Require a quoted source line**
Catches: Tiny summary, big quote risk — sample #9's one-liner must quote the customer line or stay blank.

### Skip
Leave this defense off. You accept the risk or the probe does not apply.

**Ban mind-reading verbs**
Catches: Sense the real intent — no queue without five labels (or a queue id) from the message.

---

## Go-Live Rule

The board produces a go-live rule with two parts:

1. **Slips-to-block threshold**: Ship stops at your count. Leftover Slips each need a named owner.
2. **Re-run trigger**: Re-run after prompt, model, or tool change — plus a monthly floor.

If Slips rows remain at or above the threshold, ship does not proceed until defenses flip them or an owner accepts each remaining slip.

---

## Applying the Method

1. Paste your bot's real messages (e.g., last week's live queue export).
2. Run all seven probes against those messages.
3. Mark each row Caught, Slips, or Hold.
4. For each Slips row, name the Use defense that would flip it.
5. Set your slips-to-block threshold.
6. Declare your re-run trigger.
7. Ship only when Slips count falls below threshold — or each remaining Slips row has a named owner.

---

## Clear Bar

The board tests against a clear bar — the standard the bot must meet.

Example: A two-problem message opens two tickets.

If the bot fails that bar on any probe, the row marks Slips.