---
name: ozillo-niche-bending
description: Find a fresher position next to a crowded niche by borrowing a proven format, angle or audience from an adjacent niche ("niche bending"), checked with Ozillo Studio data. Use when a niche looks saturated, or the user wants to stand out within it. Triggers - "bu niş çok kalabalık", "farklılaşmak istiyorum", "nasıl öne çıkarım", "niş bükme", "niche bending", "how do I stand out in a crowded niche".
---

# Niche bending

Combines a crowded niche with something proven elsewhere: a format, an angle or an audience. Load the `ozillo-data` skill first and follow its rules.

## Inputs

- The base niche the user is in or wants to enter.
- Market (default Turkish) and any constraints.

## Steps

1. **Measure the base.** Call `get_niche_stats` for the base niche. Note what makes it hard, for example:
   - a top-heavy subscriber band (many 1M+ channels),
   - a low outlier rate,
   - few new channels over 10K.

2. **Find what already works there.** Call `search_outliers` for the base niche (`limit: 25`), keeping at most 2 per channel. Name the formats and angles behind its outliers. These show the bends the audience already accepts. If one channel dominates, say a single creator's style may be driving it, not the niche.

3. **Generate bends.** Write 6 to 10 candidates. Each one combines the base with one change:
   - **format bend**: the base topic in another niche's winning format (documentary, challenge, "I tried X for 30 days", ranking, reaction);
   - **audience bend**: the base topic for a narrower audience (students, parents, retirees, a city or region, beginners);
   - **angle bend**: the base topic through another lens (budget, history, science, mistakes, behind the scenes);
   - **market bend**: a format that works in another language market, brought to the user's market (`language` set to that market when checking where it comes from).

   Write each bend as a short niche description.

4. **Check each bend.** Call `search_channels` (semantic, `limit: 5`) for each bend, one or two at a time. `match_quality` is almost always "good", because semantic search returns the closest channels it has, so judge by the channels themselves:
   - **Taken**: several on-topic channels, some with 10K+ subscribers. It exists; compete on execution.
   - **Thin**: only 1 or 2 on-topic channels, or only small ones. A likely gap if the parent topics have demand.
   - **Empty**: the results are about something else. Nobody does it yet: a gap or a non-niche.

   Call `get_niche_stats` for the 3 most promising (include at least one thin or empty bend if any) and compare with the base on the same format: outlier rate (`share_over_2x`), share of 100K+ channels, new channels over 10K and `channels_measured`. A bend is better when its outlier rate is at least as high with a less top-heavy field.

   For a thin or empty bend, `get_niche_stats` usually falls back to the closest broad cluster (often the base niche itself). If its examples are mostly not about the bend, do not present the numbers as the bend's; label them "base niche numbers" or skip them, and judge the bend by the parent topics' demand instead.

5. **Find the format source.** For format bends only: the "source" is the niche where that format is native (for example "belgesel" for a documentary bend, "challenge videoları" for a challenge bend), not the bend itself. Call `search_outliers` on that source niche and keep only results that use the format, to show it really works there.

## Output

Write in the user's language:

1. **Why the base is hard**, in 2 to 3 numbers.
2. **Bends table**: bend, type (format / audience / angle / market), taken / thin / empty (with the channels found), and key stats when measured.
3. **Top 3 bends in detail**:
   - the evidence: proof lines from the base niche and from the format's home niche,
   - 3 example video titles,
   - the risk, including brand or copyright risk (games, characters, other creators' work) and made-for-kids rules for bends aimed at children.
4. **Data note**: as_of ranges, and which bends were not measurable.

A bend that nobody does is a hypothesis, not proof. Say that clearly.
