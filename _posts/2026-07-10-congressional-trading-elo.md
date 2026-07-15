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

## Staying current, for free

The whole thing is built to run at zero cost. A **GitHub Actions** workflow
rebuilds the data every weekday and commits it back to the repo, so the board
stays fresh without a server, a database, or paid infrastructure — it's a static
site that quietly updates itself.

**Tools:** Python (requests), GitHub Actions, vanilla HTML/CSS/JS.

*Methodology note: disclosures report trade sizes only as ranges, so trades are
equal-weighted by default; ratings reflect timing and direction versus the index,
not dollar-weighted P&L.*
