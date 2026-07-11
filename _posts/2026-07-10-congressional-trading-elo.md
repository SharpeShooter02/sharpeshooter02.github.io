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

1. Pulls disclosed House and Senate trades from the free **Stock Watcher**
   datasets — no paid API keys.
2. Fetches end-of-day prices for each ticker and the S&P 500 via **yfinance**.
3. Scores each trade's return over a 30-trading-day window against the S&P — the
   trade's *excess return*.
4. Replays a **market-anchored ELO** in chronological order, with the S&P fixed
   at 1500, so a rating above 1500 means a member's trades have beaten the market
   on a risk-of-being-wrong-adjusted basis.
5. Writes the data the front end reads, and a static HTML board renders it with
   sortable, filterable rankings.

## Staying current, for free

The whole thing is built to run at zero cost. A **GitHub Actions** workflow
rebuilds the data every weekday and commits it back to the repo, so the board
stays fresh without a server, a database, or paid infrastructure — it's a static
site that quietly updates itself.

**Tools:** Python (pandas, yfinance, requests), GitHub Actions, vanilla
HTML/CSS/JS.

*Methodology note: disclosures report trade sizes only as ranges, so trades are
equal-weighted by default; ratings reflect timing and direction versus the index,
not dollar-weighted P&L.*
