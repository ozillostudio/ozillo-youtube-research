---
name: ozillo-script-breakdown
description: Break down the script of a YouTube video (or the top outliers of a niche) from its transcript - hook, structure, pacing, retention devices, payoff, sponsor placement and call to action - and turn it into a reusable script template. Optionally runs Ozillo's paid watchability and policy-risk analysis on the user's own script. Triggers - "bu videonun senaryosunu çözümle", "kurgusu nasıl", "hook nasıl", "senaryo şablonu", "script breakdown", "analyze this video's structure", "review my script".
---

# Script breakdown

Learns how winning videos are built, from their transcripts. Load the `ozillo-data` skill first and follow its rules, especially the transcript quota.

## Inputs

One of:

- **a video** (URL or id) to break down;
- **a niche**: break down its top outliers;
- **the user's own script** to review.

## Steps

### A. Breaking down existing videos

1. **Pick the videos.** For a niche, call `search_outliers` (`limit: 25`) and choose 2 or 3 top outliers that are:
   - from different channels,
   - about different subjects (not three videos on the same case or event),
   - long enough to have a structure: prefer 8 minutes or more for long-form. For Shorts, say the structure is compressed.

   If the user wants a hook bank or template for their own topic, ask for the topic now; otherwise build it on a typical topic of the niche and say so.

2. **Fetch the transcripts, one at a time.** Call `get_video_transcript` for one video, wait for the result, then the next. Never fetch in parallel: the server allows one fetch at a time per user, and a parallel call can return an error while still spending quota.
   - Check `quota.remaining` after the first call. Before fetching more than 3 in total, tell the user how many remain. If `quota.unlimited` is true, there is no limit to report.
   - If a fetch fails, check `get_account_status` before retrying; the fetch may already have counted.
   - If a video has no transcript, pick the next outlier.
   - Note whether the transcript is auto-generated (see `available_languages`). Auto-generated text garbles names and places, and gaps in the timestamps are usually music or b-roll, not sponsor reads.

3. **Analyze each transcript.** The timestamps make timing measurable.
   - **Hook (0 to 30 s)**: the opening line, the promise, the tension or question, and how fast it gets to the point.
   - **Setup**: the context, stakes and credibility.
   - **Structure**: sections with start times. Name the pattern: list, story, problem→solution, test or experiment, or countdown.
   - **Retention devices**: open loops ("birazdan göstereceğim"), pattern interrupts, mini payoffs, and the rhythm of new information.
   - **Payoff**: when the main promise is delivered, and whether it lives up to the title.
   - **Sponsor and CTA**: where they appear and how they are introduced.
   - **Length and pacing**: section lengths from the timestamps, and a rough sense of density (how much happens per minute). Give words per minute only if you can count words reliably, and mark it approximate.

4. **Compare.** Look across the videos: what do they share? That shared part is the template.

### B. Reviewing the user's own script (optional)

1. Ask for the script text.
2. Compare it with the template from part A: the hook, the structure, the payoff timing.
3. The deeper review is a paid analysis: section-by-section watchability plus a YouTube policy risk check. To run it:
   - call `estimate_transcript_analysis` with the character count;
   - tell the user the token cost and get explicit approval;
   - call `start_transcript_analysis` with `max_token_cost` set to the approved amount;
   - poll `get_transcript_analysis` until it completes.

## Output

Write in the user's language:

1. **Per video**: a proof line and a timeline table (time, section, what happens, device used).
2. **Shared template**: a fill-in-the-blanks script skeleton with target timings.
3. **Hook bank**: 5 hook lines for the user's topic, built on the hooks you found.
4. **Script review**, if one was requested: concrete edits, and the analysis summary if it was run.
5. **Data note**: the transcripts used (and whether auto-generated), the quota remaining, and the views as_of.

Quote transcripts briefly. Do not reproduce a full script.
