# Task 5: From Ground Truth to LLM Judgment

Testing whether a large language model can reason correctly about a
small, real dataset — 2025 Syracuse Women's Lacrosse season stats —
by checking its answers against ground truth I computed myself.

## Dataset

**Source:** Syracuse University Athletics, Women's Lacrosse statistics.
Official page: https://cuse.com/sports/womens-lacrosse/stats/2025/
(PDF version also available:
https://s3.us-east-2.amazonaws.com/sidearm.nextgen.sites/suathletics.com/stats/wlacrosse/2025/pdf/cume.pdf)

The source publishes stats as HTML tables on the page above (with a
PDF as an alternate format), but does not offer a direct CSV download.
For this project, the player-season and game-by-game tables were
manually transcribed into two CSV files so they could be processed by
the ground truth notebook and pasted into an LLM chat. This is not
included in this repo per instructions — download/transcribe it
yourself from the source above, or contact me for the exact files
used, and place them in the project root as:

- `game_log_2025.csv` — 19 rows, one per game (date, opponent, result,
  score, and team stats for that game)
- `player_season_stats_2025.csv` — 32 rows, one per player (season
  totals: goals, assists, points, shots, ground balls, etc.)

### A known data quirk

The official page's player table sums to 234 total goals, but the
team total row on the same page reports 235. This is a discrepancy
in the source data itself (likely an unlisted own-goal or
team-attributed goal), not an error introduced here. It's flagged
directly in the ground truth script's output.

## How to reproduce ground truth

Open `Ground truth stats.ipynb` in Jupyter, make sure `game_log_2025.csv`
and `player_season_stats_2025.csv` are in the same folder, and run all
cells.

This prints the verified answer key: record, top scorer, average
margin of victory, highest/lowest combined score games, and a defined
"game changer" metric (see below).

## Phase B metric definition

**Game changer score** = goals + assists + (2 x game-winning goals)

Game-winning goals are weighted double because they have a direct,
provable link to a win, not just raw scoring volume.

## Experiment narrative

I tested Claude in a brand new chat — separate from the one I used to
build my ground truth script, so it had no way of already knowing the
answers. I pasted both CSV files as raw text and just started asking
it questions, starting easy and working up to the harder stuff. Full
transcript and scoring is in `Prompt and Response.pdf`.

## Reflections

Claude got every factual question right, including one I threw in
myself specifically to try to trip it up (checking whether player
goal totals actually add up to the team total — they don't, by one
goal, and it caught that on its own). It also stuck to my own metric
definition instead of quietly swapping in its own idea of "best
player," which I honestly wasn't sure it would do.

The one place it stumbled a bit was the open-ended "coach" question.
Every individual number it gave me checked out when I went back and
verified it myself — draw controls, caused turnovers, Meghan Rode's
stats, all correct. But the overall case it built leaned on its single
best example (a close loss to Clemson) while quietly not mentioning
two other close losses in the same dataset that didn't fit the story
as neatly. Nothing it said was technically false, it just wasn't the
whole picture.

That's basically my main takeaway: I'd trust this thing to look
something up or do the math for me without checking every time. I
would not trust it to hand me a full recommendation without going
back and making sure it actually looked at all the relevant data,
not just the part that made its answer sound good.

## Repo contents

- `Ground truth stats.ipynb` — the answer key notebook
- `Prompt and Response.pdf` — full log of prompts, model responses,
  and verdicts for both Phase A (factual) and Phase B (judgment)
  questions
- `README.md` — this file
- `game_log_2025.csv`, `player_season_stats_2025.csv` — NOT uploaded to
  GitHub (see instructions above); listed here for local reference only
