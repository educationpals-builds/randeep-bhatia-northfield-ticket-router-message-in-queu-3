# Measurements: Northfield ticket router — message in, queue out

Observable evidence for each of the seven trick tasks. Every mark (Caught / Slips / Hold) must trace to something a reviewer can see in the queue, ticket count, quoted source line, or route assignment.

---

## p1_bundle — Two problems, one ticket

**What to observe:**  
Count the distinct jobs in the incoming message. If the message contains two or more problems, count the tickets the router opened.

**Observable evidence:**  
- Message text names two separate issues  
- Ticket system shows one ticket or two tickets  

**Sample message (learner specimen):**  
> Where's my order? Also the promo code never applied.

**Pass condition:** Router opens two tickets — one for order status, one for promo code.  
**Fail condition:** Router opens one ticket bundling both problems.

---

## p2_messy_harmless — Messy phrasing, clear intent

**What to observe:**  
The message is grammatically rough or emotionally charged, but the routing job is unambiguous.

**Observable evidence:**  
- Message text contains typos, run-ons, or frustrated tone  
- Queue assignment matches the obvious job  

**Sample message (learner specimen):**  
> It broke again after you fixed it yesterday.

**Pass condition:** Router assigns to the correct queue (e.g., Repairs or Support) despite the informal phrasing.  
**Fail condition:** Router misroutes or flags for human review when the job is clear.

---

## p3_mind_reader — Sense the real intent

**What to observe:**  
The message lacks explicit labels or queue identifiers. The router must not guess intent without at least five labels or a queue id from the message.

**Observable evidence:**  
- Message text contains no explicit queue name, ticket type, or category label  
- Router either requests clarification or assigns based on explicit cues only  

**Sample message (learner specimen):**  
> Can someone escalate? I've been in Billing for three days.

**Pass condition:** Router uses the explicit mention of "Billing" as a cue, or requests clarification if no explicit cue exists.  
**Hold condition:** Router infers intent from context without explicit labels — requires defense "Ban mind-reading verbs" to flip.

---

## p4_small_quotable — Tiny summary, big quote risk

**What to observe:**  
The router's summary or ticket title must quote the customer's actual words, not paraphrase.

**Observable evidence:**  
- Ticket summary field  
- Presence or absence of quoted customer language  

**Sample message (learner specimen):**  
> Store credit never showed; ticket said Refunds owns it.

**Pass condition:** Ticket summary quotes "Store credit never showed" or similar verbatim excerpt.  
**Fail condition:** Ticket summary paraphrases (e.g., "Customer inquiring about credit") without quoting the source line.

---

## p5_hidden_library — Undocumented routing logic

**What to observe:**  
The router references a queue, category, or rule that is not visible in the prompt or documented routing table.

**Observable evidence:**  
- Queue assignment to a name not in the documented list  
- Routing decision that cannot be traced to a visible rule  

**Sample message (learner specimen):**  
> Billing charged twice; chat said shipping had the tracking.

**Pass condition:** Router assigns to a documented queue (Billing, Shipping, or both) and the rule is visible.  
**Fail condition:** Router assigns to an undocumented queue or uses a hidden heuristic.

---

## p6_goldfish — Same message, same route

**What to observe:**  
Run the same message twice. The router must return the same queue assignment both times.

**Observable evidence:**  
- Two routing runs on identical input  
- Queue assignment on run 1 vs. run 2  

**Sample message (learner specimen):**  
> Password reset loop — agent told me to email support@.

**Pass condition:** Both runs assign to the same queue.  
**Fail condition:** Runs assign to different queues.

---

## p7_your_own — It verifies the customer from the call before opening a queue.

**What to observe:**  
Before the router opens a queue, it must verify the customer identity from the call or message metadata.

**Observable evidence:**  
- Presence of customer verification step in the routing log  
- Queue opened only after verification completes  

**Sample message (learner specimen):**  
> Damaged box on delivery; I need a replacement and a pickup.

**Pass condition:** Router logs a verification step (e.g., customer ID match, account lookup) before queue assignment.  
**Hold condition:** Router opens a queue without any verification step — requires a defense or process change to flip.

---

## Summary table

| Task | Observable | Source |
|------|------------|--------|
| p1_bundle | Ticket count vs. problem count | Ticket system |
| p2_messy_harmless | Queue assignment despite messy text | Queue log |
| p3_mind_reader | Explicit labels or clarification request | Message text, router output |
| p4_small_quotable | Quoted source line in summary | Ticket summary field |
| p5_hidden_library | Queue name in documented list | Routing table, queue log |
| p6_goldfish | Same route on repeated input | Two routing runs |
| p7_your_own | Verification step before queue open | Routing log |

---

## Standard line

> A two-problem message opens two tickets.

This is the clear bar for the Northfield ticket router. Any task that violates this standard is a Slips row until a defense flips it.
