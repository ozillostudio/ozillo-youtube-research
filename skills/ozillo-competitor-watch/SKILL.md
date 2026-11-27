---
name: ozillo-competitor-watch
description: A digest of what the user's competitors published recently - new videos, which ones beat their channel's normal, topics and formats they are pushing, and what the user should respond with - using the competitor list saved in the user's Ozillo Studio collections. Also helps set up the competitor list. Use for regular competitor monitoring. Triggers - "rakiplerim ne paylaştı", "rakip takibi", "rakip özeti", "bu hafta rakipler", "competitor digest", "what did my competitors post".
---

# Competitor watch

A recurring briefing on the user's competitors. Load the `ozillo-data` skill first and follow its rules.

## Inputs

- The time window. Default: the last 7 days; use 30 days for a monthly digest.
- Optionally a collection (`collection_name`) or specific channels to narrow the set.

## Steps

1. **Who the competitors are.** Call `list_competitors` (`limit: 25`) and page on while `returned` is less than `total`. Note each competitor's collections.
   - **If the list is empty**: offer to set it up. Find candidates with `find_similar_channels` on the user's channel (ask for it; with no channel, use `search_channels` on their niche), then ask the user which ones to track. Only after their approval, call `list_collections` (or `create_collection`) and `set_channel_competitor` for each chosen channel. These tools change the user's account.
   - **If competitors span several collections** (often different niches), work collection by collection: pass `collection_name` in the next steps and write the digest per collection. If there are more than 25 competitors in one set, ask which collection to start with.

2. **What they published.** Call `get_competitor_latest_videos` with `since_days` set to the window, `type: "video"` and `limit: 25`. Page on while `returned` is less than `total`.
   - Content exists only for channels tracked by the Ozillo Studio browser extension. Compare the channels that returned items with the competitor list; for each one with nothing, try `get_channel_recent_videos` with `since_days`.
   - If both return nothing, list the channel as "no data (not tracked)", not as "posted nothing".

3. **What worked.** Call `get_competitor_overview` (`limit: 25`, `videos_per_channel: 3`, and `collection_name` when working per collection). Its `avg_views_per_video` is all-time and its top videos are all-time hits, so treat them as a rough bar only:
   - Judge Shorts and long-form separately (`duration` of 180 seconds or less, or a Shorts `video_type`, is Shorts).
   - Videos a few days old understate their final views: give each video's age, and call a video a standout only when it is clearly ahead of that channel's other recent videos of the same format.
   - Note titles marked as sponsored ("#reklam", "#işbirliği", "sponsor"); for deeper sponsor research suggest `ozillo-sponsorship-scout`.

4. **Themes.** Within each collection, group the new videos by topic and format. Are several competitors pushing the same topic? That is a trend worth answering. Do not force themes across unrelated collections; if there is none, say so.

5. **Response.** For the 2 or 3 strongest moves, suggest how the user can respond: a better angle, a gap the competitor left, or a different format. If you do not know the user's own channel or niche, ask before suggesting. Use `ozillo-video-ideas` for deeper ideation.

## Output

Write in the user's language, in digest form:

1. **Headline**: the biggest competitor move of the period.
2. **Per competitor** (grouped by collection when there are several): the new videos, with age, views and format, flagging standouts.
3. **Themes across competitors** in the same collection.
4. **Suggested responses**: 3 actions.
5. **Data note**: the window, competitors with no data, `freshness.newest_uploaded_at` when present, and that view counts are stored snapshots.

To run this regularly, the user can schedule it in their own tool, for example a weekly task in Claude.
