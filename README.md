# NFL outcome model

Probabilistic prediction of NFL game outcomes from historical results, evaluated
with proper scoring rules against simple baselines and bookmaker odds.

**Status:** work in progress. Results will be reported as they are, including
where a model fails to beat its baseline.

## Approach

- **Data:** game results from public sources (ESPN scoreboard API, nflverse),
  stored in SQLite.
- **Features:** built in SQL with window functions over each team's previous
  games: rolling points scored and conceded, recent win rate, rest days and
  home field. A game's features use only games completed before its kickoff.
- **Models:** logistic regression as the baseline, then gradient-boosted trees
  (LightGBM). Both output P(home win).
- **Validation:** time-ordered split. Train on earlier seasons, tune on a
  validation season, test on later seasons. No shuffling across time.

## Evaluation

Metrics are log loss, Brier score and a calibration plot (reliability diagram)
on the held-out seasons.

| Baseline | Prediction |
|---|---|
| Home rate | Constant P(home win) from the training seasons |
| Logistic regression | Fitted on the SQL features |
| Bookmaker | Implied probability from the moneyline, overround removed |

A more complex model is kept only if it beats the simpler one on the
validation season.

## Testing

`pytest` covers the data pipeline and feature code:

- **Leakage:** a game's features are unchanged when all later games are removed.
- **Validation:** no duplicate games, at most one game per team per week, no
  overlap between training and test seasons.

## Results

To be added.

## Usage

To be added.
