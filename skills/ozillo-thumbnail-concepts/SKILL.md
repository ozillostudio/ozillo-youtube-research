---
name: ozillo-thumbnail-concepts
description: Develop YouTube thumbnail concepts from what visually works in the niche - Ozillo Studio's thumbnail search (search millions of thumbnails by what they show), outlier videos and visually similar videos - and pair each concept with a title. Use when the user needs thumbnail ideas, a thumbnail brief for a designer, or feedback on a thumbnail direction. Triggers - "kapak görseli", "thumbnail fikri", "küçük resim", "thumbnail tasarımı", "thumbnail ideas", "thumbnail brief".
---

# Thumbnail concepts

Thumbnail directions backed by what gets clicked in the niche. Load the `ozillo-data` skill first and follow its rules.

## Inputs

- The video topic and its working title, or a niche. If there is no title yet, draft working titles with `ozillo-title-formulas` first, or write concepts without a title pairing and say so.
- The channel's style constraints: face or faceless, brand colors, recurring elements.
- A visual idea the user already has ("şaşkın yüz + kırmızı ok"), if any.
- A reference video the user likes, if any.

## Steps

1. **Collect winners.** Call `search_outliers` for the niche (`limit: 25`, the user's format). Keep at most 2 per channel. These show which videos broke out; their thumbnails are the first evidence.

2. **Search thumbnails by look.** This is the main step. Write 2 to 4 short **English** visual captions, even when the user writes in Turkish:
   - one for the user's own idea, if they gave one,
   - one or two for the video topic shown the way the niche's outliers seem to show it,
   - one deliberately different direction.

   Start each with "a photo of" or "an illustration of", describe only visible things (subject, expression, action, objects, colors, composition, style), and mention on-image text only as: a thumbnail with the text "WORD" written on it.

   Call `search_thumbnails` for each caption, one after another:
   - `min_views: 10000`, `uploaded_within_days: 365`, `limit: 15`,
   - `duration: "no_shorts"` for long-form, `"any"` for Shorts.

   A look that returns several channels with high views is a proven direction. `outlier_ratio` here is views ÷ subscribers, not the channel-median ratio from step 1; label it that way.

3. **Visual siblings (optional).** If the user gave a reference video, or one outlier clearly stands out, call `search_similar_videos` with `mode: "visual"` (`limit: 10`) on it. Similar is not the same as successful: ignore results under 10K views.

4. **Look at the thumbnails.** Use the `thumbnail_url` from the tool responses (fallback: `https://i.ytimg.com/vi/<video_id>/hqdefault.jpg`).
   - If you have a way to view images, open the top 6 to 8 and describe them: composition, faces and expressions, text (word count, size, color), contrast, objects, arrows or circles, before/after splits, and background.
   - If you cannot view images, say so. Work from the captions that matched, the titles and the similarity scores, call each pattern a "similarity cluster" rather than proven, and give the user the links to check.

5. **Extract patterns.** List 3 to 5 visual patterns, each seen in at least 2 videos from different channels. Note how each thumbnail works with its title: thumbnail and title should add to each other, not repeat each other.

6. **Write concepts.** Write 4 to 6 concepts for the user's video. For each:
   - the pattern it uses,
   - the layout: what sits where,
   - the text on the image: at most 3 to 4 words, in the video's language, not repeating the title,
   - the subject and expression,
   - the colors and contrast,
   - the matching title.

## Output

Write in the user's language:

1. **Visual patterns in the niche**, each with 2 proof lines and thumbnail links. For `search_thumbnails` results, write the ratio as "views/subs ×N".
2. **Concepts**: a designer-ready brief for each, plus the title pairing.
3. **Recommendation**: which 2 to A/B test first and why.
4. **Data note**: as_of dates of outliers, the English captions you searched with, and whether the thumbnails were viewed directly or inferred.

Never claim you saw an image you did not open.
