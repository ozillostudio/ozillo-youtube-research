---
name: ozillo-niche-finder
description: Discover promising YouTube niches starting from a broad interest, skill or audience, then rank them with Ozillo Studio data (room for newcomers, outlier rate, activity, sponsorship). Use when the user does not know which niche to pick yet. Triggers - "hangi nişe girsem", "kanal açmak istiyorum ama konu bulamadım", "bana niş öner", "niş bul", "find me a niche", "what niche should I start".
---

# Niche finder

Goes from a broad interest to a short, ranked list of niches. Load the `ozillo-data` skill first and follow its rules.

## Inputs

- A broad interest, skill or audience ("cooking", "I'm a software engineer", "content for students"). If the user gives nothing, ask for one interest and one constraint (time, budget, face or faceless).
- Market. Default Turkish (`language: "tr"`).
- Constraints worth respecting: faceless only, Shorts only, low budget, no travel, and similar.

## Steps

1. **Brainstorm candidates.** Write 8 to 12 candidate niches inside the interest. Each one is a short niche description in the target language. Vary them on three axes:
   - **audience**: beginners, parents, students, professionals;
   - **format**: tutorial, vlog, review, commentary, documentary;
   - **angle**: budget, speed, challenge, local or regional.

   Respect the user's constraints.

2. **Check they exist.** For each candidate, call `search_channels` (semantic, `limit: 5`), one or two at a time.
   - If a candidate comes back `match_quality: "weak"`, rewrite it once (more concrete, or in English). If it stays weak, drop it and note it as "too new or too small to measure".
   - Drop candidates whose channels are clearly about something else, and near-duplicates that return the same channels.

3. **Measure the survivors.** Call `get_niche_stats` for the best 5 to 6 candidates, one or two at a time. Record for each, per format (long and Shorts):
   - outlier rate (`share_over_2x`, long and Shorts),
   - subscriber bands,
   - new channels and how many passed 10K,
   - active share,
   - sponsorship share,
   - `low_sample` and `channels_measured`.

   Drop off-niche channels from `examples` before counting role models (see "Reading niche stats" in `ozillo-data`).

4. **Score.** Rank on the user's format (or the stronger format, stated) with simple, visible reasoning, not a hidden formula. Use the thresholds in `ozillo-data` ("Reading niche stats"):

   | Signal | Why it matters |
   |---|---|
   | Outlier rate | Videos regularly beat their channel's normal, so a good video can win. |
   | New channels over 10K | Newcomers have succeeded recently. |
   | Few 1M+ channels | Less domination at the top. |
   | Active share | The audience is still being served. A low share can also be a gap. |
   | Sponsorship share | Brands already pay here. |

   Penalize `low_sample` niches: mark them as uncertain rather than ranking them high. Never use RPM to rank (see `ozillo-data`).

   Constraints the data cannot check (faceless, low budget) are your judgement from the niche's typical format; say so. Flag niches built on re-uploaded clips or compilations as a reused-content and copyright risk.

5. **Proof for the top 3.** Call `search_outliers` (`limit: 15`) for each of the top 3 niches, in the scored format. Pick 2 to 3 proof videos from different channels that are clearly on topic; if none are, say the niche has no clean breakouts yet.

## Output

Write in the user's language:

1. **Shortlist table**: niche, outlier rate, new channels over 10K, top-heavy or open, active share, sponsorship share, and a one-word verdict (strong / possible / weak).
2. **Top 3 in detail**: why it ranks there, 2 to 3 proof lines, and the angle a newcomer could take.
3. **Dropped candidates** in one line each, with the reason.
4. **Data note**: as_of ranges and which niches had low samples.
5. **Next step**: run `ozillo-niche-check` on the favorite, then `ozillo-video-ideas`.
