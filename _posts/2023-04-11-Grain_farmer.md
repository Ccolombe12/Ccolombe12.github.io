---
layout: distill
title: Optimal Weights
description: A fun problem about a grain farmer and their balance.
giscus_comments: true
date: 2023-04-11 00:00:01
tags: math 
authors:
  - name: Connor Colombe
    url: "https://ccolombe12.github.io/"
    affiliations:
      name: ORIE, UT Austin
  

bibliography: 

# Optionally, you can add a table of contents to your post.
# NOTES:
#   - make sure that TOC names match the actual section names
#     for hyperlinks within the post to work correctly.
#   - we may want to automate TOC generation in the future using
#     jekyll-toc plugin (https://github.com/toshimaru/jekyll-toc).
toc:
  - name: Problem Statement
    # if a section has subsections, you can add them as follows:
    # subsections:
    #   - name: Example Child Subsection 1
    #   - name: Example Child Subsection 2
  - name: Solution
  

# Below is an example of injecting additional post-specific styles.
# If you use this post as a template, delete this _styles block.
_styles: >
  .fake-img {
    background: #bbb;
    border: 1px solid rgba(0, 0, 0, 0.1);
    box-shadow: 0 0px 4px rgba(0, 0, 0, 0.1);
    margin-bottom: 12px;
  }
  .fake-img p {
    font-family: monospace;
    color: white;
    text-align: left;
    margin: 12px 0;
    text-align: center;
    font-size: 16px;
  }

---
## Problem Statement
A farmer, who sells grain, has a set of four weights that each weigh an integer number of pounds and a fair balance. She claims that she can weigh any integer number of pounds of grain up to a maximum of $$N$$ using just these weights. What is the maximum value of $$N$$ and what should the weights be? *Challenge: Given $$n$$ weights what is the maximum value of $$N(n)$$ and what should the weights be?*
<hr>
## Solution

Let us consider the generalized case and then use it to answer the question for when $$n = 4 $$. Given $$n$$ weights, we would like to pick the value of their individual weights $$w_1,w_2,\ldots, w_n$$ such that using the balance we can weigh items of weights $$1,2, \ldots, N(n)$$ such that $$N(n)$$ is maximized. In this problem we will say that you can *weigh* an item of weight $$x$$ if by placing some our weights on the left side of the balance and potentially some weights on the right side of the balance, the two sides have an absolute weight difference of exactly $$x$$. So, how can can we go about determining what the values should be?

First off, we can place an upper bound on what $$N(n)$$ by counting the number of distinct weight configurations on the balance. For a given weight, it can be either: on the left side of the balance, on the right side of the balance, or not on the balance. Since this is true for any of our $$n$$ weights, there are $$3^n$$ configurations of our weights on the balance. How many of these produce a unique weight difference? By symmetry, if we swap the  weights on either side, we produce the same net weight difference so we have over-counted by a factor of 2. The only case where this symmetry argument does not apply is when there are no weights on either side. By this logic, if each weight configuration produces a unique weight difference, then we have shown the upper bound $$\begin{equation}
  N(n) \leq \frac{3^n - 1}{2}.
\end{equation}$$

Great! Now what should our weights be? Suppose we have found that with $$n$$ weights we can measure weights up to $$N(n)$$. This maximum value must be achieved when all weights are on one side of the balance. Now suppose we can add an additional weight $$w_{n+1}$$ to our set. What should it be to maximize our new weighing potential? Well given our current weights, we can't weigh $$N(n) + 1$$ so our new weight ought to allow us to measure this. One such weight that satisfies this can be found 

$$\begin{equation}
  w_{n+1} - N(n) = N(n) + 1 \implies w_{n+1} = 2 N(n) + 1.
\end{equation}$$
Using this new weight and the previous weight set, we can achieve all integral weights up to $$N(n) + w_{n+1} = 3 N(n) + 1.$$ If we adopt this strategy of choosing the next weight to add to our set, starting at zero weights, we get the recurrence relation $$\begin{align}
  N(n + 1) & = 3 N(n) + 1\\ 
  N(0) & = 0
\end{align}$$

which can we solved to find $$\boxed{N(n) = \frac{3^n - 1}{2}}$$, the theoretical upper-bound! It then follows that $$w_{n} =  3^{n-1}$$. That is, if we have $$n$$ weights the optimal assignment of their weight values are the first $$n$$ powers of $$3$$ starting at $$0$$. Based on this, the solution to our original problem is $$\boxed{N(4) = 81}$$ and using weights $$\boxed{\{1,3,9,27\}}$$. 
Our colleague Brent Austgen cleverly pointed out that your can actually **Double** the value of $$N(n)$$ if you double the values of all the weights. Using this strategy, only even numbers are achievable but to measure an odd number you simply determine which two adjacent even numbers the weight falls between! This strategy allows us to reach up to $$3^n - 1$$ with $$n$$ weights!




