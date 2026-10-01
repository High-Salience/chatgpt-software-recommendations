# Reader protocol v2: coding brand mentions in one ChatGPT answer (codebook v2.0)

You are a careful human coder. For ONE ChatGPT answer to ONE buyer question you decide, for every dictionary brand the answer names, which of six codes applies. You have NOT seen any machine coding. Code what the answer means for a buyer reading it.

## What counts as the answer text
- The answer text in the packet, including tables. Link addresses have been removed; visible link text (e.g. "[Zendesk]" or "[zendesk.com]") IS answer text.
- Only brands in the packet's dictionary count. Aliases are listed per brand. A brand's own web domain used as link text in subject position (e.g. a table row that starts "[gusto.com]") names that brand.

## The six codes (one per brand per answer: the strongest that applies anywhere; strength order picked > recommended > listed > passing)

**picked**: the answer ITSELF chooses this brand as its answer, overall or for a case. It is a verdict, not a description. Examples:
- "My pick / my recommendation / I'd choose / I'd go with / I'd start with X", "The best choice for you is X", "Bottom line: X", "Winner: X", "Best overall: X".
- Decision rules phrased as instructions: "Choose X if...", "Pick X if...", "Go with X when...", "If you're on Microsoft 365, use X", "Heavy outbound sourcing → Lever" inside a "which should you choose" / "my recommendation" / "quick picks" / "bottom line" section.
- A shortlist the answer tells the buyer to act on: "I'd shortlist X and Y", "I'd narrow it to X or Y", "Start by trialing X", "I'd put X and Y through a POC", "demo X first".
- The #1 of a ranking the answer produces ("1. X: best overall"), and the direct answer to a "which should I use" question.
- In a head-to-head ("A vs B"), a brand the answer tells the buyer to choose for a case ("Choose A if..., choose B if..." makes both picked).

**recommended**: the brand is assigned to a stated case or fit, but NOT as the answer's own verdict. Typical form: a catalog of options where each option carries a fit label: "X: best for small teams", "Best for: agencies" table column, "X is a strong fit for Microsoft 365 environments", "X is good if you need invoicing", "worth considering for...", "particularly compelling if...". Also items 2..n of a "best X" ranking that each carry a best-for label.

**listed**: presented as an option (in a list, table, heading, bolded name, or a sentence naming options) with no case or fit assigned.

**passing**: named but not offered to the buyer as an option: comparison anchor ("X-style", "like X but cheaper", "rather than X"), what to avoid or move away from, an integration or something the buyer already has ("syncs with X", "if you already use X"), an example inside a question back to the buyer, a source cited as evidence ("[X]" citation label at the end of a sentence, "according to X"), history ("X was acquired by Y"), a payment method or channel rather than a product the buyer would buy (e.g. "customers pay via Venmo").

**anchor**: ONLY on alternative-intent questions ("X alternatives", "cheaper alternative to X"): the brand named in the question, the product being replaced. Always anchor, whatever the answer says about it.

**not_a_mention**: the alias matched text that is not the brand: an ordinary word ("drip campaigns", "Teams" meaning people, "snowflake schema", "Close probability"), a different product sharing a word ("Jitsi Meet" is not Google Meet, "Sage Intacct" is not Sage if the dictionary has them separately, "Talend Data Fabric" is not Microsoft Fabric).

## Rules of judgment
- picked vs recommended: ask "is the answer telling ME what to choose?" (picked) versus "is the answer describing who each option suits?" (recommended). A sentence like "For agencies, choose X" is picked; "X: best for agencies" in a list of eight options is recommended.
- A brand picked in one place and described elsewhere is picked (strongest wins).
- A clarifying question to the buyer does not recommend the brands it mentions.
- A one-sentence answer that only announces a comparison: the brands are listed at most; passing if it only repeats the question.

## Output
Append ONE JSON line per packet to your output file:
{"teardown": "<folder>", "id": "<qid>", "coder": "<your label>", "codes": [{"brand": "<exact dictionary brand name>", "code": "picked|recommended|listed|passing|anchor|not_a_mention", "line": <line number of the deciding evidence>, "why": "<max 10 words>"}], "missing_from_candidates": ["<dictionary brand named but not in the candidate list>"]}
Code every candidate brand in the packet exactly once, plus any dictionary brand the answer names that the candidate list missed (also list it in missing_from_candidates).
