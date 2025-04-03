---
layout: post
title: "Sum One, Somewhere"
description: My solution to last Month's Jane Street Puzzle
giscus_comments: true
date: 2025-05-01 00:00:00
published: false
---





This week's [Jane Street](https://www.janestreet.com/puzzles/) puzzle is paraphrased as follows:

>For a fixed $$p$$, independently label the nodes of an *infinite complete binary* tree $$0$$ with probability $$p$$, and $$1$$ otherwise. **For what $$p$$ is there exactly a $$1/2$$ probability that there exists an infinite path down the tree that sums to at most $$1$$ (that is, all nodes visited, with the possible exception of one, will be labeled $$0$$). Find this value of $$p$$**.

<figure>
  <center>
  <img src="/assets/img/blog_images/2025-05-01-Sum_one/Inf_tree.png" width="80%" style="display: block; margin: auto;">
  <figcaption> Figure 1. An example of a labeling of an infinite complete binary tree. 
</figcaption> 
</center>
</figure>

<br><br>
## Solution

This puzzle is similar to the one we encountered [last August](https://ccolombe12.github.io/blog/2024/JS_tree/)! It turns out we can use a similar strategy here. We will utilize **symmetry** and **recursion**. Let $$x(p)$$ denote the probability that, starting at the root of our tree, 













