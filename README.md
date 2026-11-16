# PR Lens on Martian's Code Review Bench

Two runs of [PR Lens](https://prlens.dev) on the offline set of [Martian's Code Review Bench](https://github.com/withmartian/code-review-benchmark): 50 pull requests, 158 known defects.

These are our own runs, not Martian's. Martian scores every tool on its board itself.

## Results

| Run | Hits of 158 | Wrong | Precision | Recall | F1 | F2 |
|---|---|---|---|---|---|---|
| 1 | 100 | 58 | .633 | .633 | .633 | .633 |
| 2 | 105 | 59 | .640 | .665 | .652 | .660 |

Both runs are reported. Nothing was re-run to get a better number.

## How the runs were made

- Date: 16 November 2026.
- Reviewer: PR Lens in production, with the settings customers had that day. The bot account is `coldtea-pr-lens[bot]`.
- Pull requests: fresh ones opened on public forks in this org, one fork per benchmark pull request. Every comment the bot left is readable there. Each run opened its own pull requests, so a fork holds one per run.
- Scoring: Martian's pipeline at commit `e616e84` (extract, dedup and judge steps), with Claude Opus 4.5 as the judge, on the Core set.

## Files

Each of `run-1/` and `run-2/` holds:

| File | What it is |
|---|---|
| `benchmark_data.json` | The 50 pull requests, their golden comments, and the comments PR Lens posted. Other tools' reviews are left out. |
| `candidates.json` | The issues the pipeline extracted from those comments. |
| `dedup_groups.json` | Which extracted issues the pipeline treated as the same. |
| `evaluations.json` | The judge's verdict for each pull request: hits, wrong comments and misses. |

## Checking the numbers

The raw totals in `evaluations.json` cover all 173 golden comments: 104 hits in run 1 and 109 in run 2. The board's default view is the Core set, which leaves out 15 of them and has 158. The table above is the Core set, computed with `_compute_profile_metrics` in Martian's `offline/analysis/benchmark_dashboard.py`, the same function the board uses.

## One thing to know

Both runs had a verification step switched on. It was switched off in production later the same day, so a run made after that may differ slightly.
