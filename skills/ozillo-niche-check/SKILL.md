---
name: ozillo-niche-check
description: Decide whether a YouTube niche is worth entering, with a clear verdict (go / go with an angle / avoid) backed by Ozillo Studio data - competition, room for small channels, outlier rate, activity and monetization. Use when the user asks if a niche or channel idea is good, saturated or profitable. Triggers - "bu niş mantıklı mı", "bu konuda kanal açsam", "niş doğrulama", "is this niche worth it", "validate niche", "should I start a channel about".
---

# Niche validator

Gives a verdict on one niche. Load the `ozillo-data` skill first and follow its rules (query phrasing, ratios, as_of dates, proof lines).

## Inputs

- The niche idea. If it is vague ("para kazanma"), ask one short question to narrow it, or pick the most likely reading and say so.
- Market: the language of the audience. Default Turkish (`language: "tr"`); use the user's stated market otherwise.
- Format preference (long-form, Shorts or both) if the user gave one.

## Steps

1. **Phrase the niche.** Turn the idea into a short niche description in the target language, for example "evde pratik yemek tarifleri". If the user's idea mixes two niches, validate the main one and mention the other.

2. **Niche statistics.** Call `get_niche_stats` with the query and language.
   - If `match_quality` is `"weak"`, rewrite the query once (more concrete, or in English) and call again. If it stays weak, tell the user the niche barely exists in the data, which is itself a finding.
   - If `low_sample` is true, keep the verdict cautious.

3. **Room for small channels.** Call `get_niche_stats` again with `max_subscribers: 50000`. Compare its outlier rate (`performance.long.share_over_2x` and `performance.short.share_over_2x`) with the full niche. If small channels break out about as often as everyone else, newcomers have room.

   Run the calls in steps 2 to 4 one or two at a time, not all at once.

   If the small-channel block has fewer than 10 `channels_measured`, say the comparison is rough.

4. **Proof.** Call `search_outliers` for the same query with the format that matters most (default long), `limit: 25`, and keep at most 2 per channel. Optionally repeat with `max_subscribers: 50000` to show small-channel breakouts. Drop outliers that are clearly outside the niche.

5. **Judge the niche** using these signals and the "Reading niche stats" rules in `ozillo-data`. Base each judgement on the numbers you actually got, for the format the verdict is about.

| Signal | Field | Reads as |
|---|---|---|
| Crowding at the top | `channels.subscriber_bands` | 100K+ channels above 30% of the niche → top-heavy; under 15% → open field |
| New channels succeeding | `new_channels`, `new_channels_over_10k_subscribers` | New channels passing 10K → newcomers can win (after dropping off-niche examples) |
| Outlier rate | `share_over_2x` | Above 25% → unusually open; 12–25% → typical; below 10% → views are predictable and hard to beat |
| Activity | `activity.active_channels` / `channels_with_videos` | Low share of active channels → abandoned or seasonal niche, a gap or a warning |
| Monetization | `sponsorship_share`, `store_share` | A high share of channels with sponsors or a store → brands and products already pay here. Do not use RPM for this: it ignores the topic (see `ozillo-data`) |

These thresholds are rules of thumb, not hard cut-offs. Weigh them together.

6. **Verdict.** State which format it is for. If long-form and Shorts point different ways, give the verdict for the stronger format and one line on the other (for example "Go for long-form; Shorts is crowded"). Choose one of:
   - **Go**: open enough and videos break out regularly.
   - **Go with an angle**: crowded or slow at the top, but a specific sub-angle, format or audience shows breakouts. Name that angle using the outlier titles.
   - **Avoid**: crowded, low outlier rate, no sign of sponsors, or too little data. Suggest one or two adjacent niches to check instead.

## Output

Write in the user's language. Keep it scannable:

1. **Verdict** in one sentence, with the single strongest reason.
2. **The numbers**: a short table with channels, median subscribers, subscriber bands, new channels and how many passed 10K, active share, outlier rate (long and Shorts), sponsorship share. Add a note on the as_of range and on low_sample if set.
3. **What breaks out**: 3 to 5 proof lines from `search_outliers`, and one sentence on the common pattern (topic, angle, format).
4. **Where a newcomer fits**: the small-channel comparison, and the largest new channels from `examples.largest_new` as role models.
5. **Risks** and **next step**. The next step is usually: run `ozillo-video-ideas` for this niche.

Do not promise income, views or subscribers. Leave RPM out of the verdict; mention it only if the user asks, with the caveat from `ozillo-data`.
