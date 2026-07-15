---
layout: single
title: "Overnight Gaps and the 30-Minute ORB: A Post-Gap Momentum-Continuation Study in Leveraged ETFs"
date: 2026-06-23
permalink: /research/gap-orb-momentum-continuation/
categories: [trading, research]
tags: [orb, gap-trading, leveraged-etfs, momentum, breakout, backtesting]
excerpt: "A study of how overnight gaps and opening-range behavior in leveraged ETFs combine into a systematic post-gap momentum-continuation signal — a breakout trade in the gap direction, gated by a crowding filter, on the instruments where gaps carry the loudest signal."
toc: true
toc_label: "Contents"
toc_sticky: true
---

## Overview

Leveraged ETFs amplify their underlying's overnight move by their stated
multiple — a 2x fund gaps roughly twice as hard as its index, a 3x fund roughly
three times as hard. A large amplified gap represents a real overnight
imbalance, and the first 30 minutes of trading is where the market negotiates
whether that imbalance is finished or still has room to run. This research
studies whether the **opening range breakout (ORB)** — the extremes of that
first 30 minutes — can serve as a timing trigger for **continuing** the gap
direction, and which conditioning indicators separate the days when the
breakout carries follow-through from the days it doesn't.

The trade is directional and short-horizon: buy the ORB high when the gap is
up, sell the ORB low when the gap is down, take a fixed target a couple of
range multiples away, and flatten by the close. The edge is not in the
breakout mechanic itself — most breakouts don't pay — but in the *pre-trade
conditioning* that decides which gaps are worth trading in the first place.

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
  comparable. Bigger gaps carry a stronger imbalance signal.
- **Opening-range-breakout trigger** — whether price breaches the extreme of
  the opening 30-minute range in the gap direction. This is the pure timing
  mechanic: without follow-through past the range, the imbalance has already
  been priced in and there is no trade to take.
- **Prior-session context (crowding filter)** — how the underlying behaved in
  the session leading into the gap. When the prior session ran hard *in the
  same direction* as today's gap, the pool of participants who wanted to buy
  (or sell) has already acted, and the follow-through fizzles. The strongest
  and most persistent filter in the study skips exactly these days.
- **Market-wide gap breadth** — how many distinct, unrelated products are
  dislocating on the same morning, used to distinguish an idiosyncratic
  single-name event from a broader, systemic dislocation.

---

## Headline finding

Each of the conditioning indicators shows a **positive relationship with
risk-adjusted performance**: as gap size grows, as the prior-session crowding
filter removes exhausted setups, and as market-wide breadth widens, both
returns and the Sharpe ratio of the conditioned trades improve. The effects
are directionally consistent across the separate validation period, and they
are largely complementary — combining the indicators produces a stronger and
more stable signal than any one of them alone.

The opening-range breakout, by contrast, works best as a pure timing trigger.
Attempts to add "quality" filters on top of the breakout itself — momentum
confirmations, moving-average alignment, close-past-boundary rules — did not
improve risk-adjusted performance. The edge lives in the conditioning
indicators applied *before* the trigger fires, not in second-guessing which
breakouts look prettier once they do.

In one line: this is a **post-gap momentum-continuation breakout with a
crowding filter**. It buys strength, not weakness; the leveraged-ETF universe
is chosen because gaps in those instruments carry the loudest signal, not
because the ETFs themselves mean-revert.

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
