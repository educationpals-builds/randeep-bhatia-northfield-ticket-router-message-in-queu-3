# Northfield ticket router — Scenario Analyzer

How the analyzer reads a stranger's paste into the seven board rows and defenses for the Trick-task board.

---

## Input shape

A stranger pastes:

1. **Bot description** — what the bot does, who it serves, what breaks when it fails silently
2. **Standard line** — the rule the bot must honor (e.g., "A two-problem message opens two tickets.")
3. **Sample messages** — real messages the bot will face

---

## Analyzer walkthrough

The analyzer maps each stranger paste to the seven trick tasks below. For each task, it returns **Caught**, **Slips**, or **Hold**.

### Task 1: Bundle split (p1_bundle)

**What to check:** Does the bot split a message that contains two distinct problems into two tickets?

**Observable:** Count the tickets opened. A message like:

> Where's my order? Also the promo code never applied.

must produce two tickets if the standard says "A two-problem message opens two tickets."

**Defense available:** Force a split when there are two jobs

---

### Task 2: Messy but harmless (p2_messy_harmless)

**What to check:** Does the bot route a messy message that still lands in the right queue?

**Observable:** The queue assignment matches the dominant issue. A message like:

> Refund for wrong size — not a shipping question.

should route to Refunds, not Shipping, even though shipping is mentioned.

**Defense available:** None required if Caught.

---

### Task 3: Mind reader (p3_mind_reader)

**What to check:** Does the bot infer intent without explicit labels or queue IDs in the message?

**Observable:** The bot must not guess. A message like:

> Can someone escalate? I've been in Billing for three days.

requires five labels or a queue ID from the message — not a guess about what the customer "really wants."

**Defense available:** Ban mind-reading verbs

---

### Task 4: Small quotable (p4_small_quotable)

**What to check:** Does the bot quote the customer's own words when summarizing?

**Observable:** The summary must include a quoted source line. A message like:

> Store credit never showed; ticket said Refunds owns it.

must quote the customer line or stay blank — never paraphrase without attribution.

**Defense available:** Require a quoted source line

---

### Task 5: Hidden library (p5_hidden_library)

**What to check:** Does the bot rely on undocumented routing rules or internal lookups the stranger can't see?

**Observable:** Every routing decision must trace to a visible rule. A message like:

> Billing charged twice; chat said shipping had the tracking.

must route based on explicit criteria, not a hidden lookup table.

**Defense available:** Require a quoted source line

---

### Task 6: Goldfish (p6_goldfish)

**What to check:** Does the bot remember context from earlier in the same thread?

**Observable:** A message like:

> It broke again after you fixed it yesterday.

must route with awareness of the prior fix — not treat it as a brand-new issue.

**Defense available:** None required if Caught.

---

### Task 7: Customer verification (p7_your_own)

**What to check:** It verifies the customer from the call before opening a queue.

**Observable:** The bot must confirm customer identity before creating a queue entry. A message like:

> Password reset loop — agent told me to email support@.

must not open a queue until the customer is verified from the call.

**Defense available:** Custom verification step

---

## Defense mapping

When a task returns **Slips**, the analyzer names the defense that would flip it:

| Task | Defense (if Slips) |
|------|-------------------|
| p1_bundle | Force a split when there are two jobs |
| p3_mind_reader | Ban mind-reading verbs |
| p4_small_quotable | Require a quoted source line |
| p5_hidden_library | Require a quoted source line |
| p7_your_own | Custom verification step |

Current defense state from the builder's board:

- **split_bundles:** on
- **rewrite_mind_read:** off
- **name_source:** on

---

## Go-live rule

After marking all seven tasks, the analyzer applies the go-live rule:

- **slips_to_block:** 2
- **Gate sentence:** Ship stops at your count. Leftover Slips each need a named owner.
- **Re-run trigger:** Re-run after prompt, model, or tool change — plus a monthly floor.

If the stranger's bot has 2 or more Slips rows, ship stops. Each leftover Slips row must have a named owner before launch.

---

## Output shape

The analyzer returns:

1. **Board marks** — Caught / Slips / Hold for each of the seven tasks
2. **Defense calls** — which defense flips each Slips row
3. **Go-live rule** — whether the bot can ship, and what remains

---

## Worked example: Northfield ticket router

**Bot:** Northfield ticket router — message in, queue out

**Standard:** A two-problem message opens two tickets.

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

**Board result:**

| Task | Mark |
|------|------|
| p1_bundle | Slips |
| p2_messy_harmless | Caught |
| p3_mind_reader | Hold |
| p4_small_quotable | Slips |
| p5_hidden_library | Slips |
| p6_goldfish | Caught |
| p7_your_own | Hold |

**Slips count:** 3

**Go-live decision:** Ship stops (3 ≥ 2). Each Slips row needs a named owner before launch.
