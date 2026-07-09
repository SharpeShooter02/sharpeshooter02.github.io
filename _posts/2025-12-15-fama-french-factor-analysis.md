---
title: "Which Fama–French Factors Matter? Multiple Testing and Out-of-Sample Prediction"
date: 2025-12-15
permalink: /projects/fama-french-factor-analysis/
categories: [research, finance]
tags: [asset-pricing, fama-french, lasso, multiple-testing, r]
excerpt: "An empirical asset-pricing study asking whether the standard Fama–French factors carry any robust one-month-ahead predictive power for the 25 size–value portfolios — using multiple-testing corrections and cross-validated LASSO to guard against overfitting."
toc: true
toc_label: "Contents"
toc_sticky: true
---

[Download the full report (PDF)](/assets/docs/fama-french-factor-analysis.pdf){: .btn .btn--primary}
[View the R source (.Rmd)](/assets/docs/fama-french-factor-analysis.Rmd){: .btn .btn--inverse}

## The question

A central question in empirical asset pricing is *which* risk factors actually
matter for explaining stock returns — and whether apparent "significance"
survives once you account for the fact that testing many factors across many
assets will inevitably turn up false positives. This project asks a focused
version of it: **do the standard Fama–French factors carry any robust
one-month-ahead predictive power for US stock returns, once we correct for
multiple testing and judge models by out-of-sample prediction rather than
in-sample fit?**

## Data

All data come from the Kenneth R. French Data Library, the canonical source in
academic finance. The analysis combines two monthly datasets from July 1963
onward: the **Fama–French five factors** (market, size, value, profitability,
investment, plus the risk-free rate) and the **25 portfolios formed on size and
book-to-market** — a 5×5 grid of value-weighted portfolios that is a standard
test-asset set. Returns are converted to excess returns over the risk-free rate,
and the modeling target is *next month's* excess return, predicted from the
*current* month's factors.

## Approach

The study deliberately looks at the factors through two complementary lenses:

- **An inference lens.** Separate linear factor regressions are fit for each of
  the 25 portfolios, producing 125 factor loadings in total. Significance is then
  assessed both raw and after **Bonferroni** and **false-discovery-rate (FDR)**
  corrections, to see which loadings survive the multiple-testing problem.
- **A prediction lens.** All portfolios are pooled and a standard OLS model is
  compared against a **LASSO** model whose penalty is chosen by 10-fold
  cross-validation — letting the regularizer decide, on its own, whether the
  factors are worth keeping at all.

Because the data are time series, the train/test split is **time-based**, not
random: models are trained on the earliest 80% of months (through April 2013)
and evaluated on the held-out remainder (May 2013 onward), so no future
information leaks into training.

## Findings

The three analyses point to the same conclusion:

- **Single portfolio.** For a representative portfolio (SMALL LoBM), the factor
  regression has an R² of roughly 0.001 and no significant slopes — essentially
  no ability to forecast next-month returns.
- **Cross-section.** Across all 125 loadings, the estimates cluster near zero and
  **none are significant at the 5% level even before correction**; Bonferroni and
  FDR leave the significant count at zero for every factor.
- **Prediction.** The cross-validated LASSO shrinks **all five factor slopes to
  zero**, keeping only an intercept, and matches the pooled OLS model's
  out-of-sample RMSE. Given the freedom to use the factors or not, the optimal
  choice is to ignore them and predict a constant mean.

## Interpretation

This does not contradict the large literature showing Fama–French factors help
explain *average* returns over long horizons. Rather, at a short monthly horizon
and for these diversified portfolios, the factor signal is tiny relative to the
noise in returns — one-month-ahead excess returns are dominated by noise rather
than persistent factor-driven predictability. The project is a compact
demonstration of how regression, multiple-testing correction, and regularization
can be combined to reach a disciplined, overfitting-resistant conclusion.

**Tools:** R, tidyverse, `glmnet`, `broom`, `ggplot2`.

*Full methodology, figures, and results are in the linked PDF; the complete R
Markdown source is available above.*
