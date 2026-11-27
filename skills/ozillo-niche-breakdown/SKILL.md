---
name: ozillo-niche-breakdown
description: Map how a whole YouTube niche works - who the players are, which formats and topics break out, the title formulas that repeat, what audiences ask for, and the gaps - using Ozillo Studio data across many channels. Use when the user wants to understand a niche deeply before or after entering it, not just a go/no-go verdict. Triggers - "bu nişi çözümle", "bu nişte neler işe yarıyor", "niş haritası", "niş analizi detaylı", "reverse engineer this niche", "how does this niche work".
---

# Niche breakdown

A playbook for a whole niche. `ozillo-niche-check` decides whether to enter; this skill explains how the niche works. Load the `ozillo-data` skill first and follow its rules.

## Inputs

- The niche, and the market (default Turkish).
- The format of interest, if any. Default: both, reported separately.

## Steps

1. **Shape of the niche.** Call `get_niche_stats`. Note the subscriber bands, new channels, active share, the outlier rate per format, the sponsorship share, and the example channels.

2. **The players.** Take `examples.largest` and `examples.largest_new`, drop off-niche channels, and call `compare_channels` with up to 5: the 3 largest incumbents and the 2 largest new channels. Call `search_channels` (semantic, `limit: 10`) only if that leaves too few on-niche channels. Sort the players into groups:
   - incumbents,
   - rising new channels,
   - small channels with high average views per video.

3. **What breaks out.** Call `search_outliers` twice, `format: "long"` and `format: "short"`, each with `limit: 25`, one after the other. Drop off-niche results and keep at most 2 per channel. Cluster the outliers by topic, by promise to the viewer and by format. A cluster needs at least 2 channels.

4. **Title formulas.** For the 5 strongest outlier titles, call `search_title_patterns` with `title`, one after another. Call `get_title_pattern` only for formulas that match at least 2 titles and fit them. If titles get no match or only a generic one, describe the repeating title shapes by hand and say the pattern library had no match.

5. **Audience demand.** Call `get_video_comments` (`sort: "popular"`, `limit: 100`) on the 2 strongest long-form outliers from different channels. Extract recurring questions, requests and complaints; skip debates about the video's subject that say nothing about what viewers want to watch.

6. **Content gaps.** Compare demand (step 5) with supply (step 3). Requests that no outlier answers are gaps. So are clusters where only one channel succeeds.

## Output

Write in the user's language:

1. **Niche in one paragraph**: how it works and who wins.
2. **Shape**: a stats table and the player groups.
3. **What breaks out**: the clusters, each with 2 to 3 proof lines, for long-form and Shorts separately.
4. **Title formulas**: the formula, an example, and how widely it works.
5. **What the audience wants**: comment themes.
6. **Gaps**: 3 to 5 opportunities with the evidence for each.
7. **Data note**: as_of range, `low_sample`, and the number of channels and videos analyzed (`channels_measured` and `videos` per format from `get_niche_stats`).

In niches about real people (true crime, news, scandals), do not put victims' names forward in summaries or ideas; refer to the case instead.
