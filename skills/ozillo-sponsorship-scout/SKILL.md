---
name: ozillo-sponsorship-scout
description: Research sponsorship and monetization around a channel or niche - how common sponsors and stores are, which comparable channels work with sponsors, which brands appear in their videos, and how to position for deals - using Ozillo Studio data. Use when the user wants sponsors, brand deals or other income beyond AdSense. Triggers - "sponsor bulmak istiyorum", "marka işbirliği", "hangi markalar sponsor oluyor", "kanalımdan nasıl para kazanırım", "sponsorship", "brand deals", "find sponsors".
---

# Sponsorship scout

Finds where sponsorship money already flows near the user. Load the `ozillo-data` skill first and follow its rules.

## Inputs

- The user's channel (preferred) or niche. If only a niche is given, ask once for the channel; without one, run in niche mode (see Readiness below).
- The market (default Turkish).

This skill covers sponsors and stores. For other income (memberships, affiliate links, own products), mention them briefly but do not claim data on them.

## Steps

1. **Baseline.**
   - If a channel is given, call `get_channel`: niche, subscribers, `has_sponsorships`, `has_store`.
   - Call `get_niche_stats` for the niche: `sponsorship_share`, `store_share`, the subscriber bands and the examples.
   - A high sponsorship share means brands already budget for this audience. These flags and shares come from detection: `false` or 0 means "not detected", not "none". Say so, especially when a share is 0 across the whole niche.

2. **Comparable channels.** Call `find_similar_channels` (`limit: 15`) for the user's channel, or `search_channels` (semantic, `limit: 15`) for the niche. Then call `get_channel` on the 8 to 10 closest, a few at a time, and record `has_sponsorships`, `has_store` and subscribers.
   - `avg_views_per_video` is all-time and old hits inflate it; for a fairer number take the median views of each peer's recent videos (step 3 fetches them anyway) or say the average is all-time.
   - Note the **smallest** sponsored channel: it shows how early sponsors appear in this niche.

3. **Find sponsored videos (free).** For the 4 to 6 most relevant peers, call `get_channel_recent_videos` (`since_days: 180`, `limit: 25`). Look for titles marked as sponsored: "reklam", "#reklam", "*reklam", "işbirliği", "sponsor", "#ad", "sponsored". This often finds sponsored videos, and sometimes the brand, without spending transcript quota.
   - Read the words in context: "sponsorsuz", "reklamsız" or "sponsorluk bulunmamaktadır" mean the opposite. Count only titles that mark a video as sponsored.
   - Treat a peer with marked titles as sponsored even if its `has_sponsorships` is `false`, and say so.
   - A channel description with a business contact line ("reklam ve işbirliği için", "for business inquiries") shows the channel is open to deals, not that it has sponsors; note it separately.

4. **Which brands (limited, uses transcript quota).** Only when step 3 did not name the brands: for 2 or 3 sponsored peers, fetch the transcript of a marked video (or, if none is marked, their most-viewed video from step 3's recent window), one at a time.
   - Check `quota.remaining` first and tell the user how many fetches this uses.
   - Look for sponsor reads, usually in the first 2 minutes or the middle: "bu videonun sponsoru", "sponsored by", "indirim kodu", "linki açıklamada".
   - Record the brand, its category and how the read is placed.
   - Tell the user this is a sample, not a full list.

5. **Positioning.** Compare the user with the sponsored peers: size, recent views per video, niche fit. What do the sponsored channels have that the user lacks? In niche mode, compare the niche's median subscribers with the smallest sponsored peer instead, as the threshold a new channel needs to reach.

## Output

Write in the user's language:

1. **Sponsor climate**: the niche's sponsorship and store shares, and what they mean.
2. **Sponsored peers**: a table of channel, subscribers, views per video (say whether recent median or all-time average), sponsors (detected / marked titles / not detected), store yes/no.
3. **Brands seen**: brand, category, channel and how they were integrated, and whether found in titles or transcripts. Present it clearly as a sample.
4. **Readiness**: where the user stands against the smallest sponsored peer, or in niche mode the threshold to reach.
5. **Plan**: brand categories to pitch, a pitch angle built on the user's audience (use `ozillo-comment-mining` for audience proof), and 3 integration ideas.
6. **Data note**: sample size, the niche's as_of dates, the transcripts used, and that sponsor flags are detection-based. Small differences in subscriber counts between tools are normal: they are stored at different times.

Do not estimate deal prices or promise income. RPM is not a sponsorship signal (see `ozillo-data`).
