---
name: ozillo-channel-breakdown
description: Break down how a YouTube channel works - what it publishes, which videos beat its own normal and why, its title formulas, how its best videos are structured, what its audience asks for, and how it compares with similar channels - using Ozillo Studio data. Use for someone else's channel (a competitor or a reference) when the user wants to understand, copy the playbook of, or compete with it. For the user's own channel, use ozillo-channel-audit. Triggers - "bu kanalı analiz et", "bu kanal nasıl büyüdü", "rakip kanal analizi", "kanalı incele", "reverse engineer this channel", "how does this channel do it".
---

# Reverse-engineer a channel

Explains a channel's playbook. Load the `ozillo-data` skill first and follow its rules.

## Inputs

- The channel: @handle, UC... id, channel URL or name. A URL gives the @handle or id. For a bare name, call `search_channels` with `mode: "keyword"` and confirm the match with the user if there are several.
- The user's goal, if known (learn from it, compete with it, find gaps). It decides what the summary emphasizes.

## Steps

1. **Profile.** Call `get_channel`. Note subscribers, video count, average views per video, type (long, Shorts, mixed), niche metadata, RPM range and channel score. If the channel is not found, say Ozillo does not have it yet and stop.

2. **What works for this channel.**
   - Call `get_channel_top_videos` with the channel and `per_channel: 10`.
   - Call `get_channel_recent_videos` with `limit: 25` (newest first). Read `freshness` and `notes` first: if the stored data is older than 30 days, say so and do not describe the "current" rhythm from it.
   - Split videos by format (`duration` of 180 seconds or less is Shorts). Per format, take the median views of the recent videos; recent videos at 2× that median or more are this channel's recent outliers, videos well below show what stopped working. Do not use the all-time average from step 1 as the bar: old hits inflate it.
   - Note the channel's current rhythm: how often it uploads and which format it focuses on now.

3. **Title formulas.** For the 5 best videos (top and recent outliers), call `search_title_patterns` with `title` set, one after another. Record the patterns that match and how often the channel reuses them. If most titles get no match or only a generic one, describe the channel's title habits by hand (question, number, conflict, named person or event, length) and say the pattern library had no match.

4. **Structure of the best videos.** Pick the 2 strongest **long-form** videos (highest views relative to the channel's normal, longer than 180 seconds). For a Shorts-only channel, skip transcripts and describe the opening line and format from titles instead. Fetch the transcripts one at a time with `get_video_transcript` and check `quota.remaining` in the response. From each transcript, read:
   - the hook (first 30 seconds): what promise or tension it opens with,
   - the structure: sections, where the payoff comes, how the video keeps viewers watching,
   - the call to action, sponsor placement and length.

   If the quota is used up, say so and continue without transcripts.

5. **Audience.** Call `get_video_comments` on the strongest video with `sort: "popular"`, `limit: 100`. If it returns fewer than 30 comments, also read the second strongest. Summarize what viewers praise, what they ask for next and what they complain about.

6. **Context.** Call `find_similar_channels` with the channel's UC id, `limit: 8`. Then call `compare_channels` with the channel and the 3 or 4 closest peers to show where it stands. Peers that are smaller but have a high average views per video are worth naming.

## Output

Write in the user's language:

1. **One-paragraph summary** of the playbook: what this channel does that works.
2. **Profile**: a short table from step 1. Show RPM only as a rough estimate for this channel, with the caveat from `ozillo-data`.
3. **What beats its normal**: 3 to 5 proof lines with ratios against the channel's recent median for the same format, and the common pattern behind them.
4. **Title formulas**: the patterns and an example title for each.
5. **Inside the best videos**: hook, structure, CTA. Quote briefly; do not reproduce the transcript.
6. **What the audience wants**: the comment themes.
7. **Peers**: the comparison table and one line on its position.
8. **Takeaways for the user**: 3 to 5 concrete things to copy, adapt or avoid, plus gaps the channel leaves open.

Add a data note: channel video tools return no as_of, so give the upload date range of the videos used and any freshness warning, and say which steps were skipped (for example a transcript quota).

Do not speculate about the channel's private analytics, revenue or growth history. Ozillo stores no subscriber history.
