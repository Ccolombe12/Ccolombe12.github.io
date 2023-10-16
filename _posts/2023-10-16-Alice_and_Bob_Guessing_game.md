---
layout: distill
title: Alice and Bob Play a Guessing Game
description: 
giscus_comments: true
date: 2023-10-16 00:00:01
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
    margin: 14px 0;
    text-align: center;
    font-size: 16px;
  }
  .theorem {
    display: block;
    margin: 12px 0;
    font-style: italic;
  }
  .theorem:before {
    content: "Theorem.";
    font-weight: bold;
    font-style: normal;
  }
  .proposition {
    display: block;
    margin: 12px 0;
    font-style: italic;
  }
  .proposition:before {
    content: "Proposition.";
    font-weight: bold;
    font-style: normal;
  }
  .lemma {
    display: block;
    margin: 12px 0;
    font-style: italic;
  }
  .lemma:before {
    content: "Lemma.";
    font-weight: bold;
    font-style: normal;
  }

  .proof {
    display: block;
    margin: 12px 0;
    font-style: normal;
  }
  .proof:before {
    content: "Proof.";
    font-style: italic;
  }
  .proof:after {
    content: "\25FC";
    float:right;
  }

  .definition {
    display: block;
    margin: 12px 0;
    font-style: normal;
  }
  .definition:before {
    content: "Definition.";
    font-weight: bold;
    font-style: normal;
  }
  
# End Proofs with this $$\begin{align*} & \tag*{\(\blacksquare\)} \end{align*}$$


  

---
<style type="text/css">
    ol { list-style-type: lower-alpha; }
</style>
Here is another cool problem I stumbled upon the other day.
## Problem Statement
Bob chooses a integer uniformly at random between 1 and 1000. Alice has the guess the chosen number as quickly as possible. Bib will let Alice know whether her guess is smaller than, larger than, or equal to his number. If Alice's guess is smaller than Bob's number, Bob replaces the number with another integer chosen uniformly at random from $[1,1000]$. Prove that there exists a strategy that Alice can use to finish the game in such a way that the expected number of steps is smaller than 45.
<hr>

## Solution

Let's generalize and suppose Bob picks integers uniformly from the set $$[N] = \{1,2,3,\ldots, N \}$$. I initially set out to try to determine the *optimal* strategy using dynamic programming. Let $$E_k$$ be the expected number of turns to finish the game given that we know that Bob's current number is at most $$k$$.

We can set up the recurrence 

$$
\begin{equation}
E_k = 1 + \min_{j \in [k]}\left(\underbrace{\frac{k-j}{k}E_N}_{\text{guess too low}} + \underbrace{\frac{1}{k} \cdot 0}_{\text{guess correct}} + \underbrace{\frac{j-1}{k}E_j}_{\text{guess too high}}\right) \quad \forall k \in [N].
\end{equation}
$$

But since every $$k$$ relies on $$E_n$$, the quantity we are trying to solve for, this ends up to not be as straightforwards to evaluate as we might hope.

What else can we try? Well this problem is sort of similar to the [egg drop problem](https://brilliant.org/wiki/egg-dropping/#n-eggs-k-floors) with one egg and each time it breaks we start with a new building with a randomly chosen number of floors. In the egg drop problem with two eggs, if the first egg breaks on the first floor tested, $$x$$ then the next drops must necessarily be $$x-1, x-2, \ldots, 1 $$ until the least-breaking floor is found. Let's try to build a heuristic strategy off this intuition. 

We can pick some threshold $$\ell$$ and use the strategy:
* Guess $$\ell$$ repeatedly until either we guess correct or Bob rolls a number less than $$\ell$$.
* Guess down from $$\ell, \ell - 1, \ldots, 1$$ until we hit the target

The number of turns until the first step succeeds is a geometric random variable with parameter $\ell/N$ and thus the expected number of turns until we succeed is $N / \ell$. Now, given that the target number is at most $\ell$, it has a uniform distribution over $1,\ldots,\ell$ and thus the expected number of guesses needed to reach it is:
$$
\begin{align*}
  \mathbb{E}[\text{# steps needed given step 1 succeeds}]& = \underbrace{\frac{1}{\ell}\cdot 0}_{x = \ell} + \underbrace{\frac{\ell-1}{\ell}\left(\frac{1}{\ell - 1}\sum_{k=1}^{\ell-1} k\right)}_{x < \ell}\\ 
  & = \frac{\ell-1}{2}.
\end{align*}
$$

Putting it together, let $$f(\ell)$$ be the expected number of turns to guess Bob's number under our strategy with threshold number $$\ell$$. This is given by 

$$
\begin{equation}
f(\ell) = \frac{N}{\ell} + \frac{\ell-1}{2}. \label{eq: f}
\end{equation}
$$

We would like to minimize this function. Taking the derivative, we find a single critical point at $$\boxed{\ell^* = \sqrt{2 N}}$$, and the second derivative is positive for all $\ell > 0$ which implies we have found a global minimum for our problem. Plugging this value into (\ref{eq: f}), we have that

$$
\begin{equation}
  \boxed{f(\ell^*) = \sqrt{2 N} - \frac{1}{2}}
\end{equation}
$$
which for $$N = 1000$$ gives us an expected number of moves of $$\approx$$ 44.2214 which is less than 45, as desired. This result is quite interesting! For large $N$, on average will will only need to guess $$\mathcal{O}\left(\frac{1}{\sqrt{N}}\right)$$ fraction of the numbers.

















