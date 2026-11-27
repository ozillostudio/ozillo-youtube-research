---
name: ozillo-title-formulas
description: Write and improve YouTube titles using proven title formulas from Ozillo Studio's title-pattern database and from outlier videos in the niche. Scores a draft title, explains which formula it matches, and proposes stronger variants with evidence. Use for title help of any kind. Triggers - "başlık yaz", "başlık önerisi", "bu başlık iyi mi", "başlığımı düzelt", "title ideas", "improve my title", "clickable title".
---

# Title formulas

Titles grounded in formulas that have actually worked. Load the `ozillo-data` skill first and follow its rules.

## Inputs

- One of:
  - a draft title to improve,
  - a video topic to title,
  - a niche to build a title playbook for.
- Market and language. Write titles in the video's language: the draft's language if a draft is given, otherwise the user's market (default Turkish). Use the same language for the `search_outliers` query and its `language` parameter.
- The format: long-form or Short.

If only a draft is given, derive the niche from it as a short niche description (for example "budget meal prep") and say so.

## Steps

1. **Formulas in the niche.**
   - Call `search_outliers` for the niche (`limit: 25`, the user's format). Keep at most 2 per channel and drop off-niche results.
   - For the 8 highest-ratio titles, call `search_title_patterns` with `title`, **one after another, never in parallel** (parallel calls get rate-limited).
   - Count which formulas repeat among the outliers. Ignore generic matches that do not fit the title (for example `number-opener` for a title that does not start with a number).
   - Pattern matching is weak on many titles, especially Turkish ones. Also classify every outlier title by hand on its structure: question, number or list, "I tried / denedim", comparison or "vs", price or cost reveal, mistake or warning, named person or place, stakes or challenge. Count these too. Hand-made formulas are valid evidence when their proof lines come from the outliers.

2. **How strong each formula is.**
   - For each repeating library formula, use the stats `search_title_patterns` already returned (video count, channel count, median views, trend score); call `get_title_pattern` only if you need its structure or a field that was missing. Judge by median views, not the average: a few viral videos inflate averages.
   - For niche evidence, prefer the outliers from step 1. `get_title_pattern_videos` (`limit: 5`) shows global examples without channel ratio or as_of, so present them as "examples of the formula elsewhere", never as proof lines.

3. **Globally strong formulas (optional).** Call `search_title_patterns` without a title, `sort: "trend_score"`, `limit: 10`. Keep only formulas that fit the topic; if none fit, skip this step.

4. **Check the draft (if given).** Call `search_title_patterns` with the draft.
   - Say which formula it matches (library or hand-classified), or that it matches none.
   - Check it against what works: a clear promise, curiosity, specificity (numbers, names, stakes), and length. As a rule of thumb, about 60 characters stay readable on mobile; compare with the length of the niche's outlier titles.

5. **Write variants.**
   - For a topic or draft: 8 to 10 titles, each built on a named formula from steps 1 to 3.
   - Keep the promise honest: no clickbait the video cannot pay off.

## Output

Write in the user's language:

- **Draft review** (if given): which formula it matches, and what to fix.
- **Title options**: title, formula, and the evidence (pattern stats or a proof line). Mark your top 3.
- **Formula cheat sheet** for the niche: each formula's structure, an example, its strength (how many niche outliers use it, and the library median views if it matched), and whether it came from the library or was classified by hand.
- **Data note**: the outlier as_of range, that library pattern stats are global (all languages and channels) while `get_title_pattern_videos` covers the last 365 days, and how many titles got no library match.

Do not copy outlier titles word for word. Reuse the structure, not the text.
