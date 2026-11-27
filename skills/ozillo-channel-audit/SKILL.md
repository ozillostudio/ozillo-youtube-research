---
name: ozillo-channel-audit
description: An honest audit of the user's own YouTube channel - what works, what stopped working, title and format problems, how it compares with peers, policy and script risks, and a prioritized list of quick wins - with Ozillo Studio data. Can be blunt ("roast mode") when the user asks. For someone else's channel (a competitor or a reference), use ozillo-channel-breakdown instead. Triggers - "kanalımı denetle", "kanalımı incele", "kanalımı analiz et", "kanalımı değerlendir", "kanalımı eleştir", "neden büyümüyorum", "kanalım neden izlenmiyor", "acımasızca yorumla", "audit my channel", "roast my channel", "why am I not growing".
---

# Channel audit

A diagnosis with a to-do list. Load the `ozillo-data` skill first and follow its rules.

If it is unclear whether the channel is the user's own, ask. For a channel that is not theirs, switch to `ozillo-channel-breakdown`.

## Inputs

- The channel: @handle, UC... id or URL.
- The tone: default **honest and constructive**. If the user asks for a roast ("acımasız ol", "roast"), be blunt and funny about the work, never about the person, and every jab must still rest on data.
- The user's goal, if known: more views, more subscribers, monetization or a niche change.

## Steps

1. **Profile and peers.**
   - Call `get_channel` for the profile, score and RPM range.
   - Call `find_similar_channels` (`limit: 8`). Pick up to 4 peers within roughly 0.3× to 3× the channel's subscribers and call `compare_channels` with them. If none are in that band, say there is no close peer and compare with the nearest ones, labelled as such.
   - Note where the channel trails: average views per video, video count.

2. **Check data freshness.** Call `get_channel_recent_videos` (`limit: 25`). Read `freshness` and `notes` first. If the newest stored upload is more than 30 days old, or `notes` says the data may be stale, tell the user, and do not diagnose the upload rhythm or "recent" performance from stored uploads alone.

3. **Hits and misses.** Also call `get_channel_top_videos` (`per_channel: 10`).
   - Split every video by format: `duration` of 180 seconds or less is Shorts, longer is long-form. Measure the two separately.
   - Define "recent" as the last 12 months (`since_days: 365`); if that leaves fewer than 8 videos per format, use the latest 10 of that format and give their date range.
   - Take the median views of the recent videos per format. A **hit** is a recent video at 2× that median or more, a **miss** below 0.5×. Do not compare against the all-time average, which old hits inflate.
   - Look for these problems:
     - topics that drifted away from the hits,
     - a weaker title style,
     - a format mix (Shorts vs long-form) that changed,
     - an upload rhythm that slowed (only if step 2 found the data fresh).

4. **Titles.** Run `search_title_patterns` with `title` on the top 3 hits and the 3 weakest recent videos, one after another. If titles get no match or only a generic one, compare hits and misses by hand instead (question vs statement, number, conflict, named person or place, length) and say the pattern library had no match.

5. **Niche context.** Turn the channel's niche from `get_channel` into a short niche description (for example "Türk tarihi belgesel"), then call `get_niche_stats` with the channel's language. Compare the channel's hit rate (step 3: share of recent videos at 2× or more, per format) with the niche's `share_over_2x` for the same format. Is the niche itself slow?

6. **Audience.** Call `get_video_comments` (`sort: "newest"`, `limit: 100`) on the latest 2 long-form videos (fewer if they have few comments). Count recurring themes rather than quoting single comments. Look for complaints and drop-off signals in the channel's language, for example:
   - Turkish: "çok uzun", "boş konuşma", "clickbait", "ses kötü", "yanlış bilgi", "eskisi gibi değil", "neden video gelmiyor".
   - English: "too long", "clickbait", "sound", "you changed", "wrong info".

7. **Script risk (optional, paid).** If the user wants it, review the script of the weakest recent video:
   - fetch its transcript,
   - call `estimate_transcript_analysis`,
   - tell the user the token cost and get their approval,
   - call `start_transcript_analysis` with that cost as `max_token_cost` and poll `get_transcript_analysis`.

   This gives a watchability and YouTube policy risk review.

## Output

Write in the user's language:

1. **Verdict in 3 sentences**: the main problem and the main strength.
2. **Scorecard**: a table of the channel vs its peers vs the niche (average views per video, recent median views, hit rate, uploads, format mix), long-form and Shorts in separate rows.
3. **What works**: hits with proof lines.
4. **What is not working**: misses, with a likely reason backed by data, not guesses.
5. **Quick wins**: 5 prioritized actions. Each has the evidence behind it and the expected effect, stated as a direction, not a number.
6. **Risks**: policy or script risks, if analyzed.
7. **Data note**: the niche's as_of dates, the date range of the channel videos used, any freshness warning, and the steps skipped.

Do not speculate about private analytics (CTR, retention, revenue). Ozillo only sees public data.
