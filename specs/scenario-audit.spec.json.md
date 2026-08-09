{
  "$schema": "https://educationpals.ai/schemas/trick-task-board/v1",
  "spec_name": "Northfield ticket router — message in, queue out",
  "description": "Seven-row trick-task board for auditing the Northfield ticket router before Friday's rebuild.",
  "standard_line": "A two-problem message opens two tickets.",
  "source": "Last week's live queue export (10 messages).",
  "tasks": [
    {
      "id": "p1_bundle",
      "label": "Two problems, one ticket",
      "probe": "Does the router split a message with two distinct issues into two tickets?",
      "sample_message": "Where's my order? Also the promo code never applied.",
      "verdict_vocabulary": ["Caught", "Slips", "Hold"]
    },
    {
      "id": "p2_messy_harmless",
      "label": "Messy but harmless",
      "probe": "Does the router handle a rambling message that still has one clear job?",
      "sample_message": "It broke again after you fixed it yesterday.",
      "verdict_vocabulary": ["Caught", "Slips", "Hold"]
    },
    {
      "id": "p3_mind_reader",
      "label": "Mind-reader trap",
      "probe": "Does the router infer intent without explicit labels or queue id from the message?",
      "sample_message": "Can someone escalate? I've been in Billing for three days.",
      "verdict_vocabulary": ["Caught", "Slips", "Hold"]
    },
    {
      "id": "p4_small_quotable",
      "label": "Tiny summary, big quote risk",
      "probe": "Does the router preserve the customer's exact words or silently paraphrase?",
      "sample_message": "Store credit never showed; ticket said Refunds owns it.",
      "verdict_vocabulary": ["Caught", "Slips", "Hold"]
    },
    {
      "id": "p5_hidden_library",
      "label": "Hidden library dependency",
      "probe": "Does the router rely on unstated knowledge about queue names or routing rules?",
      "sample_message": "Billing charged twice; chat said shipping had the tracking.",
      "verdict_vocabulary": ["Caught", "Slips", "Hold"]
    },
    {
      "id": "p6_goldfish",
      "label": "Goldfish memory",
      "probe": "Does the router forget context from earlier in the same thread?",
      "sample_message": "App crash on checkout — same as last week's incident thread.",
      "verdict_vocabulary": ["Caught", "Slips", "Hold"]
    },
    {
      "id": "p7_your_own",
      "label": "Custom trick task",
      "probe": "It verifies the customer from the call before opening a queue.",
      "sample_message": "Password reset loop — agent told me to email support@.",
      "verdict_vocabulary": ["Caught", "Slips", "Hold"]
    }
  ],
  "board_results": {
    "p1_bundle": "Slips",
    "p2_messy_harmless": "Caught",
    "p3_mind_reader": "Hold",
    "p4_small_quotable": "Slips",
    "p5_hidden_library": "Slips",
    "p6_goldfish": "Caught",
    "p7_your_own": "Hold"
  },
  "defenses": [
    {
      "id": "split_bundles",
      "label": "Force a split when there are two jobs",
      "explain": "Catches: Two problems, one ticket — sample #3 must open two tickets before this router ships.",
      "status": "on"
    },
    {
      "id": "rewrite_mind_read",
      "label": "Ban mind-reading verbs",
      "explain": "Catches: Sense the real intent — no queue without five labels (or a queue id) from the message.",
      "status": "off"
    },
    {
      "id": "name_source",
      "label": "Require a quoted source line",
      "explain": "Catches: Tiny summary, big quote risk — sample #9's one-liner must quote the customer line or stay blank.",
      "status": "on"
    }
  ],
  "go_live_controls": {
    "slips_to_block": 2,
    "gate_sentence": "Ship stops at your count. Leftover Slips each need a named owner.",
    "rerun_trigger": "Re-run after prompt, model, or tool change — plus a monthly floor."
  },
  "sample_messages": [
    "Refund for wrong size — not a shipping question.",
    "It broke again after you fixed it yesterday.",
    "Where's my order? Also the promo code never applied.",
    "Cancel the subscription but keep the open return.",
    "Billing charged twice; chat said shipping had the tracking.",
    "Password reset loop — agent told me to email support@.",
    "Damaged box on delivery; I need a replacement and a pickup.",
    "Can someone escalate? I've been in Billing for three days.",
    "Store credit never showed; ticket said Refunds owns it.",
    "App crash on checkout — same as last week's incident thread."
  ]
}