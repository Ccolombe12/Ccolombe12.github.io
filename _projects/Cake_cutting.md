---
layout: page
title: Envy-Free Cake Cutting 
description: An overview of envy-free cake cutting algorithms
img: assets/img/project_images/CakeCuttingThumb.png
published: true
importance: 1
category: work
---

In Fall 2024 I took an algorithmic game theory course offered by professor [Shuchi Chawla](https://www.cs.utexas.edu/~shuchi/) and one of the topics we covered was the field of fair-division. In my course project, I collaborated with [Alejandro Leos](https://alexgomezleos.github.io) to create an in-depth research overview of the field of [envy-free cake cutting](https://en.wikipedia.org/wiki/Envy-free_cake-cutting).

The core concept behind envy-free cake cutting revolves around dividing a single, potentially heterogeneous resource (the “cake”) among  $$n$$  agents in a manner that satisfies the following criteria:

1.	**Disjoint Allocations**: Each agent receives a distinct portion of the cake, and no two agents share any part of the resource.
2.	**Completeness**: The entire cake is allocated, leaving no portion unassigned.
3.	**Envy-Freeness** : No agent prefers another agent’s allocation over their own. In other words, each agent perceives their allocation as the "best" of the realized allocations.

This final condition, envy-freeness, ensures a "fair" allocation and is particularly challenging to achieve when the cake is heterogeneous and agents have different preferences over the segments of the cake.

Developing algorithms and protocols that guarantee envy-free allocations is a complex and active area of research in algorithmic game theory. In our work, Alejandro and I conducted a comprehensive literature review of envy-free cake cutting algorithms, analyzing the state-of-the-art protocols and their strengths and limitations. We also explored promising future research directions and presented preliminary ideas for advancing the field.

<iframe src="/assets/pdf/CakeCutting.pdf" style="width: 100%; height: 600px;" frameborder="0">
    Your browser does not support PDFs. 
    <a href="/assets/pdf/CakeCutting.pdf">Download the PDF</a>
</iframe>
