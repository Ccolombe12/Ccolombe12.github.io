---
layout: distill
title: Generalizing the "Drunk Pasenger" Problem
description: 
giscus_comments: true
date: 2023-08-16 00:00:01
published: true
tags: math
output: 
  pdf_document:
    extra_dependencies: ["bbm", "threeparttable","Bookdown"] 
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
  .theorem {
    display: block;
    font-style: italic;
      }
  .theorem:before {
    content: "Theorem. ";
    font-weight: bold;
    font-style: normal;
    }
  .theorem[text]:before {
    content: "Theorem (" attr(text) ") ";
  } 
  .proposition {
    display: block;
    font-style: italic;
      }
  .proposition:before {
    content: "Proposition. ";
    font-weight: bold;
    font-style: normal;
    }
  .proposition[text]:before {
    content: "Proposition (" attr(text) ") ";
  } 


  

---
<style type="text/css">
    ol { list-style-type: lower-alpha; }
</style>
I recently stumbled across a problem that asked for a generalization of the "Drunk Passenger" problem. The classic problem is as follows: 
## Problem Statement
There are $$n$$ seats on an airplane and $$n$$ passengers. The passengers are in a line to board the plane and the $$i$$-th passenger in line is assigned to seat number $$i$$ on the plane. However, the first passenger in line is drunk and takes a seat on the plane uniformly at random. The rest of the passengers take a on the plane seat one by one. Passenger $$i \geq 2 $$ will sit in their assigned seat if it is available, otherwise they will sit in an open seat uniformly at random. What is the probability the $$n$$-th passenger in line sits in their assigned seat? 

The version of the problem we will solve is: find the probability that the $$k$$-th passenger in line sits in their assigned seat for $$k = 1,\ldots,n$$.
<hr>

## Solution

Let $$P(n, k)$$ be the probability that the $$k$$-th passenger sits in their assigned seat. Let's do a little exploration. We can immediately reason that because the first passenger ($$k=1$$) always picks their seat uniformly at random, we have that $$P(n,1) = 1/n $$ for all $$n$$. But what about for $$k=2$$ ? The second passenger will sit in their assigned seat only if the first passenger does not steal their seat. This implies $$P(n,2) = \frac{n-1}{n}$$. For $$ k \geq 3 $$, we rely on an important observation:

>If the first passenger sits in seat $$ 3 \leq  i \leq n$$, then the passengers $$2,\ldots, i - 1 $$ will all sit in their assigned seats. 

We can now find $$P(n,k)$$ by conditioning on three events:

1. The first passenger sits in seat $$1$$.
2. The first passenger sits in seat $$ i $$ where $$i > k$$.
3. The first passenger sits in seat $$ i $$ where $$ 2 \leq i \leq k -1$$.


In events a) and c) passenger $$k$$ sits in their assigned seat with probability 1. Event b) requires some more careful analysis. 

Say passenger $$1$$ sits in seat $$ i $$ such that $$ 2 \leq i \leq k -1 $$. In this case, passengers $$ 2,\ldots,i-1$$ all get their assigned seats. Passenger $$i$$, now without their assigned seat, will sit uniformly at random in one of the seats numbered $$1, i+1, i+2, \ldots, n$$. But this is the same problem as before with passenger $$i$$ as the new "drunk passenger" whose assigned seat is $$1$$. In this smaller problem, there are $$n-i+1$$ passengers and passenger $$k$$ in the larger problem is now passenger $$k - i + 1$$ in the smaller problem. Therefore in the event that passenger $$1$$ sits in seat $$i$$, the probability that passenger $$k > i$$ sits in their assigned seat is $$P(n+1 - i, k + 1 - i)$$.

To put this all into a recurrence relation, we have

$$\begin{equation}
\begin{split}
P(n,k) & = \Pr\left[\text{$k$ sits in seat $k$}|\text{1 sits in seat 1} \right]\cdot \Pr[\text{1 sits in seat 1} ]\\ 
&  + \Pr\left[\text{$k$ sits in seat $k$}|\text{1 sits in seat $i > k$} \right]\cdot \Pr[\text{1 sits in seat $i > k$} ]\\ 
& + \sum_{i=1}^{k-1} \Pr\left[\text{$k$ sits in seat $k$}|\text{1 sits in seat $i$} \right]\cdot \Pr[\text{1 sits in seat $i$} ]\\ 
& = 1 \cdot \frac{1}{n} + 1 \cdot \frac{n-k}{n} + \sum_{i=2}^{k-1}P(n+1 - i, k + 1 - i)\cdot \frac{1}{n}\\ 
& = \frac{n+1 -k}{n} + \frac{1}{n} \sum_{i=2}^{k-1}P(n+1 - i, k + 1 - i)\\ 
& = \frac{n+1 -k}{n} + \frac{1}{n} \sum_{i=1}^{k-2}P(n - i, k - i)\\
\end{split}\label{eq:recurrence}
\end{equation}$$

with base cases $$P(n,1) = \frac{1}{n}$$ and $$P(n,2) = \frac{n-1}{n}$$ for all $$n$$. But how might we go about evaluating (\ref{eq:recurrence})?

Well, by solving for a few small values of $$k$$ by hand, we find that $$P(n,3) = \frac{n-2}{n-1}$$ and $$P(n,4) = \frac{n-3}{n-2}$$. This leads us to the following proposition which we will then prove by induction on $$k$$.


>For $$n \in \mathbb{Z}_+ $$ and integer $$k \leq n$$ the probability that passenger $$k$$ sits in their assigned seat in the drunk passenger problem is 
$$ \begin{equation}
    \boxed{P(n,k) = \begin{cases}
      \frac{n + 1 - k}{n + 2 - k} & k\geq 2\\ 
      \frac{1}{n} & k = 1.
      \end{cases}}
  \end{equation}$$.


We will prove this by induction on $$k$$. First note the base cases for $$P(n,1)$$ holds. Then using (\ref{eq:recurrence}) we have $$P(n,2) = \frac{n-1}{n}$$ which agrees with our previous results. These two base cases hold for all $$n \geq 2$$. We will now proceed with the inductive hypothesis and assume the result holds for all $$k' < k$$. It now suffices to show $$P(n,k) = \frac{n + 1 - k}{n + 2 - k}$$.

From (\ref{eq:recurrence}) we have 
$$\begin{equation}  
\begin{split}
P(n,k) & = \frac{n + 1 - k}{n} + \frac{1}{n} \sum_{i=1}^{k-2}P(n - i, k - i)\\ 
       & = \frac{n + 1 - k}{n} + \frac{1}{n} \sum_{i=1}^{k-2}\frac{(n-i) + 1 - (k-i)}{(n-i) + 2 - (k-i)}\\ 
       & = \frac{n + 1 - k}{n} + \frac{1}{n} \sum_{i=1}^{k-2}\frac{n + 1 - k}{n + 2 - k}\\ 
       & = \frac{n + 1 - k}{n} + \frac{k-2}{n}\cdot\frac{n + 1 - k}{n + 2 - k}\\
       & = \frac{n + 1 - k}{n}\left(1  + \frac{k-2}{n + 2 - k}\right)\\
       &  = \frac{n + 1 - k}{n}\left(\frac{n}{n + 2 - k}\right)\\
       & = \frac{n + 1 - k}{n + 2 - k}
\end{split}
\end{equation}  
$$
as desired.

Based on this we do recover the classic result that $$P(n,n) = 1/2$$. Taking the derivative of $$P(n,k)$$ with respect to $$k$$ we can see that it is a decreasing function in $$k$$ and that the later in the line you are, the less likely it is you will sit in your assigned seat with a worst case probability of $$1/2$$.

















