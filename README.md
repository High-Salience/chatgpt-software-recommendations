# ChatGPT Software Recommendations: 21 Categories, 2,100 Answers

This dataset records which software brands ChatGPT names, recommends and picks when buyers ask about 21 software categories, and which websites it cites. It covers 2,100 ChatGPT answers (100 buyer questions in each of 21 categories), coded brand by brand, with Google's organic top 10 for the same questions as the control.

It is published by [High Salience](https://highsalience.com/), an AI search and SEO agency. Every file here is also published on the matching research page at [highsalience.com/research](https://highsalience.com/research/), where each category has a written teardown.

## What is in the dataset

| Measure | Count |
|---|---|
| Software categories | 21 |
| Buyer questions (one ChatGPT answer each) | 2,100 |
| Brand appearances (recommended or listed) | 8,218 |
| Recommended (the answer assigns the brand to a case or fit) | 7,997 of 8,218 |
| Picked (the answer itself chooses the brand) | 6,475 of 7,997 |
| Cited source links | 6,443 |
| Brands in the category dictionaries | 1,074 |

## How the data was collected

- **Surface:** ChatGPT with web search on, United States, English, one run per question, collected through the DataForSEO scraper.
- **Dates:** September 8 to September 26, 2026. The collection date of each category is in `teardowns.csv`.
- **Questions:** 100 per category in four intents: category ("best CRM software"), comparison ("salesforce vs hubspot"), alternative ("salesforce alternatives") and recommendation ("which CRM should I use for a 5 person sales team").
- **Control:** Google's organic top 10 for the same question on the same date, except where Google served a swapped results page that day: for those questions, Google's page for the same question on October 23, 2026 (see Corrections).
- **Brands:** only brands in each category's dictionary (`brands.csv`) are coded. Aliases and domains are listed per brand.

## How the answers were coded

Two independent AI readers (Claude agents, not people) coded every answer blind from a written protocol, `reader-protocol.md`. A third reader settled the 204 codes on which they disagreed (2.2 percent of 9,252 brand codes). The two readers gave the same one of six codes 97.8 percent of the time, and agreed 99.2 percent of the time at the recommended level. The full rulebook and its dated amendments are in `codebook.md`.

The codes, strongest first:

- **picked:** the answer itself chooses the brand, overall or for a case ("my pick", "choose X if").
- **recommended:** the answer assigns the brand to a stated case or fit ("best for small teams"). In `mentions.csv` a picked brand has `mention_type` = recommended and `picked` = 1.
- **listed:** presented as an option with no case or fit.
- **passing:** named, but not offered to the buyer as an option.
- **anchor:** the brand named in an "alternatives to X" question. It is excluded from every share.

## Files

| File | Rows | What one row is |
|---|---|---|
| `teardowns.csv` | 21 | One category: collection date, counts and the teardown page |
| `queries.csv` | 2,100 | One buyer question |
| `mentions.csv` | 9,211 | One brand named in one answer |
| `citations.csv` | 6,443 | One source link cited in one answer |
| `observations.csv` | 2,100 | One answer: counts of brands, recommendations and sources |
| `brands.csv` | 1,074 | One brand in one category dictionary |
| `reddit-presence.csv` | 700 | One question from categories 1 to 7: Reddit in Google, in the AI Overview and in the ChatGPT answer |
| `youtube-presence.csv` | 700 | One question from categories 1 to 7: YouTube in Google, in the AI Overview and in ChatGPT's citations |
| `industry-queries.csv` | 100 | One buyer question in fintech, healthcare, legal or ecommerce (25 each), collected September 26, 2026 |
| `industry-per-question.csv` | 100 | Google top 10, AI Overview sources and ChatGPT cited domains for each industry question |
| `industry-chatgpt-citations.csv` | 554 | One ChatGPT citation on an industry question |
| `google-control.csv` | 1,400 | One question from categories 8 to 21: the Google top 10 used as the control, and for a swapped page, the September 26 page it replaced |
| `industry-google-control.csv` | 100 | The same for the industry questions |
| `codebook.md` | | Coding rulebook, version 1.0 through amendment 2.0 |
| `reader-protocol.md` | | The protocol the AI readers followed |

Every teardown table starts with `teardown` (1 to 21) and `category`. The `id` column is the question number inside that category (q001 to q100), so join tables on `teardown` plus `id`.

### mentions.csv

| Column | Meaning |
|---|---|
| `intent` | category, comparison, alternative or recommendation |
| `query` | The buyer question |
| `brand` | Brand name from the dictionary |
| `position` | Order of first appearance in the answer, counting from 1 |
| `mention_type` | recommended, listed, passing or anchor |
| `sentiment` | positive, neutral or negative |
| `in_query` | 1 if the question names the brand |
| `brand_domain_in_google_top10` | 1 if the brand's own site was in Google's organic top 10 for the same question |
| `bolded` | 1 if the answer bolds the brand name |
| `in_table` | 1 if the brand appears in a table |
| `picked` | 1 if the answer itself chooses the brand |

### citations.csv

| Column | Meaning |
|---|---|
| `domain` | Registrable domain of the cited page |
| `class` | first-party (the site of a brand in that category's dictionary), review-or-media, or other-third-party |
| `url` | The cited address as ChatGPT returned it |
| `domain_in_google_top10` | 1 if that domain was in Google's organic top 10 for the same question |

### observations.csv

| Column | Meaning |
|---|---|
| `status` | ok for every answer in this release |
| `n_brands` | Dictionary brands named in the answer |
| `n_recs` | Brands coded recommended |
| `n_listed` | Brands coded listed |
| `n_sources` | Sources cited |
| `google_top10_brand_sites` | Google top 10 results that are sites of dictionary brands |
| `google_top10_third_party` | Google top 10 results that are any other site |

## Load it

```python
import pandas as pd

base = "https://raw.githubusercontent.com/High-Salience/chatgpt-software-recommendations/main/"
mentions = pd.read_csv(base + "mentions.csv")
citations = pd.read_csv(base + "citations.csv")

# most-picked brands in one category
crm = mentions[mentions.category == "CRM software"]
print(crm.groupby("brand").picked.sum().sort_values(ascending=False).head(10))
```

The same files are on Hugging Face at [highsalience/chatgpt-software-recommendations](https://huggingface.co/datasets/highsalience/chatgpt-software-recommendations).

## Limits to keep in mind

- One run per question. ChatGPT's answers vary between runs, so a brand missing from one answer is "not observed in this run", not proof that it is never named.
- The readers are AI models. Their agreement with each other is high, but it is not the same as agreement with a human panel.
- Only dictionary brands are coded. An answer can recommend a brand outside the dictionary, and that brand does not appear in `mentions.csv`.
- United States, English, September 2026. Answers change over time and by location.
- For the questions listed under Corrections, the Google control is four weeks newer than the ChatGPT answer.

## Corrections

**October 23, 2026: the Google control for categories 8 to 21 and the industry questions.** Since late September 2026, Google has answered many automated searches with swapped results pages full of the wrong sites. We checked every Google page collected on September 26, 2026 (categories 8 to 21 and the 100 industry questions) against Google's page for the same question on October 23, 2026 (DataForSEO, Web results only). 481 of the 1,400 category pages and 28 of the 100 industry pages shared fewer than half of their sites with it, so for those questions the control is now the October 23 page. Categories 1 to 7, collected September 8 to 21, were checked the same way and are unchanged.

Columns that changed: `brand_domain_in_google_top10` in `mentions.csv`, `domain_in_google_top10` in `citations.csv`, `google_top10_brand_sites` and `google_top10_third_party` in `observations.csv`, `google_top10` in `industry-per-question.csv` and `in_google_top10` in `industry-chatgpt-citations.csv`. Every ChatGPT answer, brand code and citation is unchanged. Across the 21 categories, the share of recommended mentions going to brands whose own site was not in Google's top 10 moves from 78 to 74 percent. `google-control.csv` and `industry-google-control.csv` list every question with the page used and the page it replaced. The AI Overview columns of `industry-per-question.csv` are as collected on September 26 and include the swapped pages.

## Citation and license

Please cite as: High Salience (2026). *ChatGPT Software Recommendations: 21 Categories, 2,100 Answers.* https://highsalience.com/research/

The data is released under [Creative Commons Attribution 4.0](https://creativecommons.org/licenses/by/4.0/). You may use and adapt it, including commercially, with a credit and a link to High Salience.

Questions and corrections: hello@highsalience.com
