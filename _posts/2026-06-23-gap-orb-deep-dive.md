---
layout: single
title: "Overnight Gaps and the 30-Minute ORB: An Intraday Mean-Reversion Study in Leveraged ETFs"
date: 2026-06-23
categories: [trading, research]
tags: [orb, gap-trading, leveraged-etfs, mean-reversion, backtesting]
excerpt: "A study of how overnight gaps and opening-range behavior in leveraged ETFs combine into a systematic intraday mean-reversion signal — and which indicators carry the edge."
toc: true
toc_label: "Contents"
toc_sticky: true
---

## Overview

Leveraged ETFs amplify their underlying's overnight move by their stated
multiple — a 2x fund gaps roughly twice as hard as its index, a 3x fund roughly
three times as hard. When that amplified gap is large, it often reflects order
flow chasing the move into the opening auction rather than a repricing the rest
of the session agrees with. This research studies whether the **opening range
breakout (ORB)** — the first 30 minutes of trading — can serve as a confirmation
signal for fading those over-extensions back toward the open, and which
conditions make the fade reliable.

The work is built on several years of minute-bar data across a broad universe
of leveraged ETFs spanning broad-market, sector, crypto, and single-stock
products. The emphasis throughout is methodological discipline: every signal is
computed *causally* (using only information available at the moment a trade
would fire), results are split across separate discovery and validation periods,
and realistic transaction costs — commissions, spreads, and market-impact — are
modeled on every trade.

---

## The indicators

The strategy's edge comes from *conditioning before the entry trigger*, not from
the breakout itself. Four primary indicators drive the system:

- **Overnight gap size** — the magnitude of the leveraged ETF's overnight gap,
  measured in underlying-equivalent terms so that funds of different leverage are
  comparable.
- **Opening-range-breakout confirmation** — whether price commits to a direction
  within the opening 30-minute range, used as the timing trigger for entry.
- **Prior-session context** — how the underlying behaved in the session leading
  into the gap, used to gauge whether the move still has fuel behind it.
- **Market-wide gap breadth** — how many distinct, unrelated products are
  dislocating on the same morning, used to distinguish an idiosyncratic
  single-name event from a broader, systemic dislocation.

---

## Headline finding

Each of these indicators shows a **positive relationship with risk-adjusted
performance**: as gap size grows, as prior-session context aligns with the
thesis, and as market-wide breadth widens, both returns and the Sharpe ratio of
the conditioned trades improve. The effects are directionally consistent across
the separate validation period, and they are largely complementary — combining
the indicators produces a stronger and more stable signal than any one of them
alone.

The opening-range breakout, by contrast, works best as a pure timing trigger.
Attempts to add "quality" filters on top of the breakout itself did not improve
risk-adjusted performance — the edge lives in the conditioning indicators above,
applied *before* the trigger, rather than in second-guessing which breakouts
look prettier.

---

## Methodology notes

A few principles that the study treats as non-negotiable, since they're what
separate a real edge from a backtest artifact:

- **Causal / look-ahead-free.** Every input is computed from information
  available at decision time. A surprising amount of the apparent edge in naive
  backtests of strategies like this turns out to be look-ahead leakage; removing
  it is the difference between a believable result and an inflated one.
- **Discovery vs validation.** Findings are established on one period and then
  checked, unchanged, on a separate later period.
- **Realistic costs.** Commissions, bid-ask spreads, and market-impact are
  subtracted from every trade rather than assumed away.

---

## Scope and limitations

This is a backtest study, and the usual caveats apply: parameter choices carry
optimization risk, the instrument universe was partly selected with hindsight,
and the sample spans a particular set of market regimes that future conditions
may not replicate. The detailed parameterization, exact thresholds, and full
sizing logic are intentionally not published here.

*Further write-ups will go deeper on individual indicators over time.*
