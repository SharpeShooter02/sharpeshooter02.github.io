---
title: "Congressional Trading ELO: Scoring Congress' Stock Trades Against the S&P 500"
date: 2026-07-10
permalink: /projects/congressional-trading-elo/
categories: [projects, finance]
tags: [python, data-viz, github-actions, elo, congress, yfinance]
excerpt: "A self-updating leaderboard that treats each member of Congress' disclosed stock trades as head-to-head matches against the S&P 500 and ranks them with a market-anchored ELO rating."
toc: true
toc_label: "Contents"
toc_sticky: true
---

[View the live leaderboard](https://sharpeshooter02.github.io/congress-elo/){: .btn .btn--primary}
[Source on GitHub](https://github.com/SharpeShooter02/congress-elo){: .btn .btn--inverse}

## The idea

Members of Congress disclose their stock trades under the STOCK Act, and there's
endless debate about whether those trades beat the market. This project turns
that question into a game: each disclosed trade is treated as a **head-to-head
match against the S&P 500**, and every member earns a **market-anchored ELO
rating** based on how their trades actually performed relative to the index.

## How it works

A Python builder (`build_leaderboard.py`) does the whole pipeline:

1. Pulls disclosed House, Senate, and executive-branch trades — current and
   former officials — from the free, daily-updated **Congress Trading Monitor**
   open dataset (itself aggregated from the House Clerk, Senate eFD, and OGE
   filings), no paid API keys.
2. Scores each trade on a **time-decayed excess return versus the S&P 500**: an
   edge that shows up within weeks counts fully, one that takes a year counts
   less, and one that only appears after years counts least (but never zero). The
   idea is that trading on non-public information reveals itself as being right
   *fast* — so well-timed trades rise, and lucky buy-and-holds don't dominate.
3. Replays a **market-anchored ELO** across every filer's full trade history in
   chronological order, with the market fixed at 1500, so a rating above 1500
   means a member's trades have consistently beaten the market on timing.
4. Writes the data the front end reads, and a static HTML board renders it with
   sortable, filterable rankings.

## The math behind the score

Every member starts at **1500**, the same fixed rating assigned to the S&P
500. Each disclosed trade is replayed, in chronological order, as one match
between the member and the market.

**Turning a trade into a result.** A trade doesn't get scored on its raw
return — it gets scored on *excess return*: the trade's return minus what the
S&P did over the same window. Because kadoa's data gives three return
snapshots per trade (~30 days, ~1 year, and since-the-trade), those three
excess figures are blended into one number using a **time-decay weighting**
(1.00 / 0.30 / 0.05): an edge that shows up within a month counts almost
fully, one that only shows up a year later counts less, and one that only
shows up after several years barely moves the needle. Each horizon is also
clamped to ±50 percentage points first, so one freak multi-year gain can't
single-handedly swing a rating. For a sell, the sign is flipped — a member
"wins" a sell if the stock lags the market after they got out.

That blended number, `eff`, becomes the match result: a win (`S = 1`) if
`eff` beats the market by more than the 0.5% tie band, a loss (`S = 0`) if it
trails by more than that, and a tie (`S = 0.5`) in between.

**The ELO update.** This is standard chess-style ELO, just with the market
sitting in one seat of every match. The member's expected score against the
market is

```
E = 1 / (1 + 10^((1500 − member_elo) / 700))
```

using a rating divisor of 700 rather than chess's 400, which spreads the
leaderboard out more — 700 points of edge over the market roughly corresponds
to the kind of gap a 400-point divisor would compress into something less
visually distinct. The rating then moves by

```
elo += K × mov × (S − E)
```

where `K = 32` is the base sensitivity (identical to standard chess ELO), and
`mov` is a **margin-of-victory multiplier** — `min(1 + ln(1 + |eff|), 4.5)` —
so a trade that beat the market by 40% moves the rating more than one that
beat it by 2%, but the log keeps a single absurd outlier trade from dominating
a member's entire history (capped at 4.5x a normal win).

Because every match is played against a market pinned at exactly 1500, a
rating above 1500 has one reading: across their disclosed trades, weighted
toward how quickly the edge showed up, this member has beaten the S&P more
often, and by more, than a coin flip would predict.

**Tools:** Python (requests), GitHub Actions, vanilla HTML/CSS/JS.

*Methodology note: disclosures report trade sizes only as ranges, so trades are
equal-weighted by default; ratings reflect timing and direction versus the index,
not dollar-weighted P&L.*
