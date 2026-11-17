# PR Lens on Martian's Code Review Bench

Five runs of [PR Lens](https://prlens.dev) on the offline set of [Martian's Code Review Bench](https://github.com/withmartian/code-review-benchmark): 50 pull requests, 158 known defects.

These are our own runs, not Martian's. Martian scores every tool on its board itself.

## Results

| Run | Date (2026) | Hits of 158 | Wrong | Precision | Recall | F1 | F2 |
|---|---|---|---|---|---|---|---|
| 1 | 16 Nov | 100 | 58 | .633 | .633 | .633 | .633 |
| 2 | 16 Nov | 105 | 59 | .640 | .665 | .652 | .660 |
| 3 | 16 Nov | 100 | 71 | .585 | .633 | .608 | .623 |
| 4 | 16-17 Nov | 101 | 68 | .598 | .639 | .618 | .630 |
| 5 | 17 Nov | 105 | 69 | .603 | .665 | .633 | .651 |
| **Mean** | | **102.2** | **65.0** | **.612** | **.647** | **.629** | **.639** |

Every run we made is reported. Nothing was re-run to get a better number. The best single run is run 2, at F1 .652 and F2 .660.

## How the runs were made

- Reviewer: PR Lens in production, with the settings customers had at the time of each run. The bot account is `coldtea-pr-lens[bot]`.
- Pull requests: fresh ones opened on public forks in this org, one fork per benchmark pull request. Every comment the bot left is readable there. Each run opened its own pull requests, so a fork holds one per run.
- Scoring: Martian's pipeline at commit `e616e84` (extract, dedup and judge steps), with Claude Opus 4.5 as the judge, on the Core set.

## Files

Each `run-N/` folder holds:

| File | What it is |
|---|---|
| `benchmark_data.json` | The 50 pull requests, their golden comments, and the comments PR Lens posted. Other tools' reviews are left out. |
| `candidates.json` | The issues the pipeline extracted from those comments. |
| `dedup_groups.json` | Which extracted issues the pipeline treated as the same. |
| `evaluations.json` | The judge's verdict for each pull request: hits, wrong comments and misses. |

## Checking the numbers

The raw totals in `evaluations.json` cover all 173 golden comments: 104 in run 1, 109 in run 2, 104 in run 3, 105 in run 4, 109 in run 5. The board's default view is the Core set, which leaves out 15 of them and has 158. The table above is the Core set, computed with `_compute_profile_metrics` in Martian's `offline/analysis/benchmark_dashboard.py`, the same function the board uses.
