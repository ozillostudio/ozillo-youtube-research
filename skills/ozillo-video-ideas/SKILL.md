---
name: ozillo-video-ideas
description: Generate YouTube video ideas that are backed by proof - each idea is derived from real outlier videos in the niche (videos that got several times their channel's normal views), with a working title, angle and the evidence behind it. Uses Ozillo Studio outliers, title patterns and viewer comments. Use when the user wants video ideas or topics for a niche or channel. For writing or fixing titles themselves, use ozillo-title-formulas. Triggers - "video fikri", "ne çekeyim", "içerik fikirleri", "konu önerisi", "video ideas", "what should I make next".
---

# Video ideation

Turns outlier evidence into video ideas. Load the `ozillo-data` skill first and follow its rules.

## Inputs

- The niche, or the user's channel. If they give a channel (@handle or UC... id), call `get_channel`, turn its niche metadata into a short niche description (a few words, in the channel's language), and use its language as the market.
- Format: long-form, Shorts or both. Default long-form unless the channel is Shorts-first.
- How many ideas. Default 10.

## Steps

1. **Collect outliers.** Call `search_outliers` with the niche query, the format and `limit: 25`.
   - If `match_quality` is `"weak"`, rewrite the query and retry once.
   - If the user gave a channel under 50K subscribers, call it a second time with `max_subscribers: 100000` so ideas come from channels of a similar size, and merge the two lists by video, dropping duplicates. Without a channel, ask nothing and skip this.
   - If fewer than 8 outliers come back, widen with `published_within_days: 730` or `min_ratio: 1.5`, and say you did.

2. **Group them.** Cluster the outliers by the idea behind them, not by wording: the topic, the promise to the viewer, and the angle (for example: price or cost reveal, "I tried X for 30 days", beginner mistakes, comparison, insider secret). One group needs at least 2 videos from different channels to count as a pattern; a single video is a weaker "one-off" signal. Count distinct channels per group, and do not let one channel's several hits pass as a pattern.

3. **Check title formulas.** For the 3 strongest outlier titles, call `search_title_patterns` with `title` set, one after another. Use a match only if it fits the title; its stats already show how widely it works (judge by median views). If titles get no useful match, take the title structure from the outliers themselves.

4. **Listen to viewers (optional, recommended).** For the top 2 or 3 outliers, call `get_video_comments` with `sort: "popular"`, `limit: 100`. Many popular comments are just thanks; skip those and keep repeated questions, requests ("bunun devamı gelsin", "X'i de yapar mısın"), disagreements and confusion. Each is a candidate idea that the audience already asked for. If a video yields nothing useful, move on rather than fetching more.

5. **Write the ideas.** For each idea:
   - It must be traceable to at least one proof line: a group from step 2, a title pattern, or a comment theme.
   - Adapt the idea to the user's channel and audience. Do not copy a title; borrow the structure and the promise.
   - Prefer ideas whose evidence comes from several channels over ones that depend on one creator's personality.

## Output

Write in the user's language. For each idea:

- **Working title** (plus one alternative)
- **Angle**: one sentence on what makes it click-worthy
- **Why it should work**: 1 to 3 proof lines (`Channel · "Title" · views · ×ratio · date`), or the comment theme with a short paraphrase
- **Format and hook**: long-form or Short; one line on the first 10 seconds

Order the ideas by strength of evidence. Then add:

- **Patterns seen**: the groups from step 2, in one line each
- **Data note**: the as_of range of the outliers (earliest to latest `as_of` among the videos used), and how many outliers and channels the ideas rest on

Do not claim an idea will get a certain number of views.
