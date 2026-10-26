# NFL outcome model

Predicting the probability that the home side wins an NFL game, from historical
results only, and checking whether those probabilities are any good.

**Status: in progress.** No results yet. This README will carry them, whatever
they turn out to be, including if the model doesn't beat the bookmaker.

## What it does

- Pulls public NFL results (ESPN's scoreboard endpoint or nflverse).
- Loads them into SQLite and builds the features in SQL: rolling form, points
  for and against, rest days, home advantage. Every feature uses only games
  that finished before kickoff.
- Fits a logistic regression baseline, then a gradient-boosted model.
- Trains on earlier seasons and tests on later ones. Nothing is shuffled across
  time.

## How it's measured

The measure was fixed before any model was fitted.

- Log loss and Brier score on held-out seasons.
- A calibration plot: when the model says 70%, does the home side win about
  70% of the time?
- Baselines to beat, in order of difficulty: always pick the home side, the
  logistic regression, and the bookmaker's implied probability (from the
  moneyline, with the margin removed) where historical lines can be found.
  A model that doesn't beat a baseline gets dropped, and the README says so.

## Tests

The data pipeline and feature code have tests, mainly leakage checks: a
feature for a game must not change if every game after it is deleted.

## Running it

To follow once the pipeline is in.
