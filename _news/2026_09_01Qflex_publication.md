---
layout: post
title: The QFlex Distribution now published in Decision Analysis!
date: 2026-09-01 00:00:01
inline: false
related_posts: false
thumbnail: "assets/img/news_images/quantile_plot.png"
description: "Our QFlex paper was published in Decision Analysis"
---

One of my dissertation chapters, ["The QFlex Distribution"](https://pubsonline.informs.org/doi/10.1287/deca.2025.0517), coauthored with my advisors Eric Bickel and Ben Leibowicz, was recently published in *Decision Analysis*! 

Quantile-parameterized distributions (QPDs) allow us to produce continuous probability distributions that directly match a handful of quantile assessments, such as an expert's 10th, 50th, and 90th percentiles about an unknown quantity. QPDs' ability to go from sparse expert estimates to a plausible probability distribution makes them a natural tool for decision analysis. In the paper, we introduce QFlex, a new QPD built entirely from monotone transformations of valid quantile functions. Unlike the widely used [Metalog](https://en.wikipedia.org/wiki/Metalog_distribution) distribution, QFlex guarantees a valid distribution through simple coefficient constraints, so fits never need numerical repair afterwards. This guarantee doesn't come at the cost of accuracy: across roughly 3,500 benchmark distributions from the Pearson system[^pearson], QFlex generally matches or exceeds the Metalog's accuracy at moderate orders.

For further reading, a free preprint of the paper is available on [SSRN](https://papers.ssrn.com/sol3/papers.cfm?abstract_id=5930859).

[^pearson]: The [Pearson system](https://en.wikipedia.org/wiki/Pearson_distribution) is a family of continuous distributions that spans a wide range of shapes, including the normal, beta, gamma, and Student's *t*. It contains a distribution for every feasible combination of skewness and kurtosis, which makes it a natural benchmark for testing how flexible a distribution is.

<hr>
