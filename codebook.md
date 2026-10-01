# Benchmark 01 Codebook v1.0 (draft, dated before collection)

Written before any data is collected. Amendments are versioned and dated. Ships with the dataset.

## 1. Units
- One **observation** = one query, one surface, one run.
- Surfaces: ChatGPT (search on), Gemini, Google AI Mode. Google organic top 10 is the control, not a surface.
- Runs: 3 per query per surface, all inside one 7-day window. We report frequency across runs, never a single answer.

## 2. Brand mention
A brand is **mentioned** when its name (or a listed alias) appears anywhere in the answer text, tables included. Aliases per brand are fixed in `brands.csv` before collection (e.g. "monday", "monday.com", "Monday.com" = one brand).

Position = order of first appearance in the answer, counting from 1.

## 3. Recommendation vs passing mention
A mention is coded **recommendation** when at least one of these is true:
- The brand appears in a ranked or "best for" list produced by the answer.
- The answer uses a choosing verb about it: "choose", "pick", "go with", "I'd start with", "best overall", "best value", "recommended".
- The brand is the direct answer to a "which should I use" query.

A mention is coded **passing** when it appears only as a comparison anchor, an example of what to avoid, or inside a cited source title without being adopted in the answer body.

A brand named in the user's query (e.g. "Asana vs ClickUp") is coded on the same rules; being in the query does not make it a recommendation.

## 4. Sentiment
Per mention, one of: **positive**, **neutral**, **negative**.
- Positive: framed as a strength or the pick for a stated case.
- Negative: framed by a drawback with no offsetting pick ("overkill", "gets pricey", "limited", "avoid").
- Neutral: listed without evaluation.
A "watch-out" column beside a "best for" column is coded positive with a caveat flag, not negative.

## 5. Citations
- Every cited URL logged as-is, then deduplicated to the canonical registrable domain (www stripped, utm stripped).
- A domain is **first-party** when it belongs to a brand in `brands.csv` for that category. Everything else is **third-party**.
- Third-party subtypes: publisher/media, review or comparison site, UGC (Reddit, Quora, forums), vendor-adjacent (a competitor's blog), reference (Wikipedia), other.

## 6. Failures
Refusals, errors, empty answers, and answers that ask a clarifying question instead of naming brands are logged with a failure code and counted in the denominator.

## 7. Control
Google organic top 10 for the same query, same window, US English. "Does not rank" = not in the top 10 organic results on the collection date.

## 8. Reporting rules
- Every number is attributed to the collection window, location, language and surface version.
- Absence in our sample is reported as "not observed in N runs", never as "zero visibility".
- Competitor and vendor domains appear as plain text.
- We do not rank ourselves or clients in the published tables.

## 9. Quality check
A random 5% of observations are hand-coded independently against this codebook. Agreement rate is published with the results. If agreement is under 90% on any field, the rule is clarified in v1.1 and that field is recoded.

---

## Amendment v1.1 (September 8, 2026, before benchmark collection; after the project-management teardown test run)

Section 3 is replaced. The two-level scheme (recommendation / passing) collapsed on test data: a comparison table naming ten brands coded all ten as recommendations. Three levels are used instead.

**Recommended.** The answer selects the brand for a stated case. Cues: "best overall", "best value", "best for [case]", "my pick", "top pick", "I'd choose / pick / go with / start with", "recommend", "winner", or the brand is the direct answer to a "which should I use" query. A bolded brand on a line that also carries one of those cues is recommended.

**Listed.** The brand appears in a comparison table, a bulleted or numbered list of options, or a bolded lead-in, without a selecting cue on the same line. Listed brands are visible but not chosen.

**Passing.** The brand appears only in running text as a comparison anchor, an example, a caveat, or inside a cited source title.

Reported metrics use "recommended" for recommendation share, and "recommended + listed" for presence share. Both are published.

## Amendment v1.2 (September 8, 2026, before collection)

Brand dictionary source: `brands_v1.csv`, built from the brand entities DataForSEO's LLM Mentions corpus surfaced per category on 9/8/2026 plus manual curation of the obvious market leaders (8 per category). Aliases and canonical domains fixed before collection. An answer that recommends only brands outside the dictionary is coded "left the competitive set" and reported separately, not as a failure.

Query universe: `queries_v1.csv`, 20 categories x 50 queries, generated from fixed templates so every category gets the same intent structure. Runs: 3 per query per surface. Surfaces: ChatGPT (scraper, web search forced), Gemini (scraper), Google AI Mode; Google organic top 10 as control. Standard queue, United States, English.

Collection note (9/8 smoke test): the Gemini scraper returns no `sources` unless `expand_citations: true` is set; the benchmark sets it. Google organic control is pulled at depth 20 and truncated to the first 10 organic results, because depth 10 returns only 6 to 9 organic rows once ads, videos and People Also Ask take slots. Google AI Mode results arrive as an `ai_overview` item with `references`.

## Amendment v1.3 (September 8, 2026, before collection): triangulation sources

The primary dataset is our own collection (ChatGPT scraper, Gemini scraper, Google AI Mode, Google organic control) coded under this rulebook. Two vendor tools are used only to triangulate, never to replace it, and their figures are always attributed by name and kept in their own units.

1. **Ahrefs Brand Radar, topic table** for category-level share: the count of AI responses mentioning each brand within the category topic. This is the closest vendor analogue to our recommendation share, because it is scoped to the category rather than to every prompt about the brand.
2. **Semrush AI Visibility, domain overview** for the surface split (ChatGPT / AI Overview / AI Mode / Gemini), citation counts and top cited sources per brand domain. Semrush's mention counts span every prompt that names the brand, including support questions, so they are reported as brand presence, not recommendation.
3. The Semrush visibility score out of 100 is relative to an auto-selected competitor set that changes per domain and is not recorded.

Agreement between our coded results and either tool is reported as agreement in direction (same brand order), not as equivalence of numbers, because the tools sample different prompt sets over different windows. Snapshots are read in the same week as our collection and saved with the dataset.

**Approval record:** queries_v1.csv (1,000 queries) and brands_v1.csv (160 brands) approved without changes by Bike Hawley on September 8, 2026, before collection. Frozen copies: queries_v1_APPROVED_2026-09-08.csv, brands_v1_APPROVED_2026-09-08.csv. Collection waits for account funding so that all runs fall inside one seven-day window as the methodology requires; no partial run will be started on the remaining starter balance.

## Amendment v1.4 (September 17, 2026, after the CRM teardown collection, before its publication)

Two coding fixes, applied to every dataset published from this point and re-applied to the project-management teardown so the two categories are coded identically. Both teardowns publish their v1.1 and v1.4 figures.

1. **URLs are not answer text.** The address inside a markdown link is removed before brand matching; the visible link text is kept. Under v1.1 a citation to a page whose address contained a brand name ("alternatives-to-salesforce") counted as a mention of that brand on that line, and a selecting cue elsewhere on the line then made it a recommendation.

2. **Selecting cues extended.** The v1.1 list (best overall, best value, best for, top pick, my pick, I'd choose / pick / go with / start with, recommend, winner, easiest, best choice) missed the hedged verbs ChatGPT now uses to select a brand for a stated case. Added: I'd lean, I'd look at, look closely at, I'd shortlist / shortlist, narrow it (down) to, narrow the field to, my default, worth considering / worth serious consideration / worth evaluating / worth looking at / worth investigating, I'd (start by) evaluate / evaluating, investigate first, makes (more) sense if, tends to fit, fits best, may fit better, near the top of the shortlist, strongest option. The rule itself is unchanged: a brand is recommended when the answer selects it for a stated case, listed when it appears in a table or list without being selected, passing otherwise.

3. **The brand being replaced is not a recommendation.** On alternative-intent queries ("Salesforce alternatives", "cheaper alternative to HubSpot") the brand named in the query is coded **anchor**, a fourth mention type, and is excluded from both recommendation share and presence share. Under v1.1 the anchor was coded like any other brand, so "if you are replacing Salesforce, I'd shortlist these" counted as a Salesforce recommendation. Anchor rows stay in mentions.csv so the effect can be recomputed.

Hand-check (section 9) on the CRM teardown: five answers drawn with a fixed seed, 22 coded mentions, 21 agreed with the reader under v1.4. The one disagreement is a contrast mention ("rather than bolted on via Mailchimp, I'd shortlist these") that a selecting verb on the same line turned into a recommendation. That failure mode is left in and reported rather than patched by hand.

Definition note for the comparison finding: a head-to-head query is coded "fork" when every brand named in the query is recommended, "one side" when at least one but not all are, and "no pick" when none of the query brands is recommended (the answer describes differences and asks for context, or recommends only brands outside the query).

## Amendment v1.5 (September 17, 2026, after the email marketing teardown hand-check, before its publication)

**Conditional assignment counts as a recommendation.** The email marketing hand-check (five seeded answers, 22 coded mentions) agreed with the reader on 19, under the 90 percent floor in section 9, so the rule is clarified and every dataset is recoded. The three misses were lines of the form "**B2B startup with a sales team:** HubSpot" and "**SaaS/product-led startup → Customer.io**": the answer assigns a brand to a stated case without using a selecting verb. That is a selection for a stated case, which is what "recommended" has meant since v1.0. Under v1.5 a bolded lead-in ending in a colon or question mark, or an arrow, followed within three words by a brand name codes that brand as recommended. Lead-ins that are labels rather than cases (caveat, note, important, tip, warning, however, bottom line, summary) are excluded, and table rows are not affected. A random sample of 40 lines caught by the new rule across the three datasets was read; all 40 were genuine assignments.

Effect: the rule catches many more picks in every category, and the gap between categories reported under v1.4 narrows. Recommendation counts for the most-recommended brand move from 60 to 68 (Asana), 39 to 62 (HubSpot, CRM) and 25 to 40 (Mailchimp). Answers with no recommended dictionary brand move from 8 to 5, 36 to 13 and 29 to 8. Every published teardown page carries v1.5 figures from this date; the project management page keeps its original v1.1 figures with an update note, and its v1.5 recode ships with the later datasets.

Re-check under v1.5: email marketing 22 of 22 agree; CRM 21 of 22 (the Mailchimp contrast mention from v1.4 remains). Known conservative bias left in: a line naming a brand in a comparison table with no cue is listed, never recommended, even when the table's "best fit" column reads as a pick.

### Implementation note, September 17 to 21, 2026 (coder fixes under v1.5, no rule change)

Three defects in the shared coder were found by a second analyst running the rulebook on a sixth category (payment processing) and were fixed in the shared coder; every published dataset was recoded and every affected page regenerated. (1) The test for "brand named in the query" was case-sensitive for the aliases that must match capitalised only (Close, Front, Kit and the like), while the query list is lowercase, so those brands were never treated as anchors on alternative queries and were ignored by the head-to-head classification. (2) The conditional-assignment pattern missed a brand written in bold after the intervening words ("marketplace: investigate **Stripe** first"). (3) Two written variants of existing cues were not matched ("I'd also look at", "the first one I'd investigate"). Effect across the five datasets coded here: recommendation counts moved by at most four mentions per dataset and by at most one answer per brand; one accounting head-to-head (Xero versus Sage) moved from fork to single winner; one email marketing answer gained a pick. Reported rulebook gaps not yet fixed: the reverse conditional ("Stripe if you are primarily online"), "simplest starting point", and "I'd start here" under a brand heading are still coded as listed.

## Amendment v2.0 (September 27, 2026): reader coding and two recommendation levels

Every answer in teardowns 1 to 21 was recoded by two independent AI readers (Claude agents, not people) working blind from a written protocol, published with each dataset as reader-protocol.md. A third reader settled the 204 codes on which they disagreed (2.2 percent of 9,252 brand codes); none were left unresolved. The two readers gave the same one of six codes 97.8 percent of the time, and agreed 99.2 percent of the time at the recommended level. The rule-based coder used for the earlier figures agreed with the final codes on 50 to 77 percent of brand codes, depending on the category, so every published figure was replaced.

Two levels are now reported. Recommended: the answer assigns the brand to a stated case or fit ("best for small teams"); this is the level the earlier pages called recommended, now applied as the rule intended, and it includes picked brands. Picked: the answer itself chooses the brand as its verdict, overall or for a case ("my pick", "choose X if", a shortlist it tells the buyer to act on, the first entry of its own ranking, or the direct answer to "which should I use"). Listed, passing and anchor keep their v1.5 meanings. Link addresses are not answer text; visible link text is.

The rule-based coder (teardown_code.py, with the v1.5.1 fix that strips link addresses) is kept as a first pass only. Brand dictionaries were not changed in this pass.
