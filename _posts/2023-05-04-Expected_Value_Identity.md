---
layout: distill
title: An Expected Value Identity
description: 
giscus_comments: true
date: 2023-04-14 00:00:01
published: true
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
<style type="text/css">
    ol { list-style-type: lower-alpha; }
</style>
I recently decided to brush up on my stochastic processes knowledge and started reading *Stochastic Processes* by Sheldon Ross, 1995. In the problems section of the first chapter, I came across an expected value identity that I knew was true for non-negative discrete random variables, but I had never used for continuous random variables. Alongside it was a further generalization for higher order moments. Having worked out the problem, I think it was interesting enough to share!
## Problem Statement
Let $$N$$ denote a non-negative random variable.  
1. ) Show that $$\begin{equation}\mathbb{E}[N] = \sum_{k=0}^\infty \mathbb{P}[N > k]\end{equation}$$

2. ) In general, show that if $$X$$ is a nonnegative random variable with distribution $$F$$, then  $$\begin{equation}
  \mathbb{E}[X] = \int_0^{\infty} \overline{F}(x)\; dx
\end{equation} $$

where $$\overline{F}(x) = 1 - F(x)$$.

3. ) and $$ \begin{equation}
  \mathbb{E}\left[X^n\right] = \int_0^{\infty}n x^{n-1} \overline{F}(x)\; dx \label{eq: janky expected val}
\end{equation}$$

<hr>

## Solution

a. ) Recall the equation for the expected value of a non-negative random variable $$N$$, 
$$\begin{equation}
\mathbb{E}[N] = \sum_{k=0}^\infty k \cdot \mathbb{P}[N = k] = \sum_{k=1}^\infty k \cdot \mathbb{P}[N = k] \label{eq:discrete EV}
\end{equation}
$$

Note that for a given $$k$$, we are adding up $$ k $$ copies of $$\mathbb{P}[N =k]$$. We can visualize this as a grid

$$\begin{align*}
  & \mathbb{P}[N = 1] + \\ 
  & \mathbb{P}[N = 2] + \mathbb{P}[N = 2] + \\ 
  & \mathbb{P}[N = 3] + \mathbb{P}[N = 3] + \mathbb{P}[N = 3] + \\ 
  & \quad \vdots \qquad \qquad \qquad \vdots \qquad \qquad \quad
    \vdots 
  \end{align*}$$

  The standard expected value equation (\ref{eq:discrete EV}) assumes that we sum the rows of the above grid. The $$k$$-th row sum being $$k \cdot \mathbb{P}[N = k]$$. However, if we consider the column sums, the $$k$$-th column sum is given by 

  $$ \begin{equation*} \mathbb{P}[N = k] + \mathbb{P}[N = k + 1] + \cdots  = \mathbb{P}[N > k - 1]
  \end{equation*} $$

  which implies that we can write (\ref{eq:discrete EV}) as 

  $$\begin{equation*}
    \mathbb{E}[N] = \sum_{k=0}^\infty\mathbb{P}[N > k]
  \end{equation*}$$
  as desired. Note that this proof this proof relied on using the grid of probabilities and summing it in two different ways. This method will not work when $$N$$ can take negative values since we can no longer construct the grid.

  c.) Note that b.) follows from c.) for $$ n = 1$$. Let $$ n $$ be some positive integer and consider the standard equation for $$\mathbb{E}[X^n]$$ for a non-negative RV $$ X $$

  $$
  \begin{equation}
    \mathbb{E}[X^n] = \int_0^\infty x^n f(x)\; dx \label{eq:cont ev}
  \end{equation}
  $$\
  where $$f(x) = \frac{d F(x)}{dx}$$ is the probability density function for the random variable $$X$$. We will show using integration by parts that (\ref{eq: janky expected val}) reduces to the standard (\ref{eq:cont ev}).

  Consider
  $$ \int_0^{\infty}n x^{n-1} \overline{F}(x)\; dx$$ and using the standard integration by parts notation let $$u = \overline{F}(x) = 1 - F(x)$$ and $$dv = n x^{n-1}$$, which implies $$du = - f(x)\; dx $$ and $$V = x^n$$. Using the standard integration by parts formula $$\int_{a}^b u \;dv  = [uv]^b_a - \int_{a}^b v \; du$$, we obtain

  $$
  \begin{align*}
    \int_0^{\infty}n x^{n-1} \overline{F}(x)\; dx & = \lim_{b \to \infty}\left[x^n \left(1 - F(x)\right)\right]_0^b + \int_0^{\infty}x^n f(x)\\ 
    & = (0 - 0) + \int_0^{\infty}x^n f(x)\\ 
    & = \mathbb{E}[X^n]
  \end{align*}
  $$
  
  as desired!








