---
layout: page
title: Seceratary Problem Variant
description: A variant of the classical seceratary problem
img: /assets/img/project_images/MDP_thumb.png
published: true
importance: 1
category: work
---

In the Spring of 2023 I was enrolled in a Markov decision processes (MDP) course at UT. In this course we studied the classical [secretary problem](https://en.wikipedia.org/wiki/Secretary_problem). Since my introduction to the problem and the analysis that leads to its beautiful asymptotic results, I have been fascinated with the problem. Fortunately, the MDP course had a broad final project requirement and I used this opportunity to formulate and analyze my own variant of the famous secretary problem!

My variant described as follows: 

> There are $$N$$ candidates applying for a single job. The candidates have some relative ranking to one another and we, as the hiring manager, are tasked with hiring the candidate with the relative lowest rank of of all of the candidates. However, our boss has instructed us that we only have *one offer to give* (for fear of seeming desperate).
>
>  The candidates are interviewed one by one in a uniformly random order. Once the $$k$$-th candidate has been interviewed, we can now observe their rank relative to the previously interviewed $$k-1$$ candidates. We then have two options:
> - Send our single offer to one of the previous $$k$$ candidates interviewed and the game ends whether or not they accept.
> - Wait to send an offer and continue interviewing. 
>
> The final caveat is that if we send the offer to a candidate we interviewed $$r \in \{0,1,\ldots,k-1\}$$ interviews ago, ($$r=0$$ implies the candidate we just interviewed) they will accept our offer with probability $$q(r)$$ where $$q(r)$$ is some known decreasing function in $$r$$.
>
> The goal is: given $$q(r)$$ and $$N$$, determine a strategy to maximize the probability of getting an accepted offer from the candidate with the lowest ranking of all the candidates (i.e. the best candidate).

This ended up being a really interesting problem to think about and mathematically analyze. We ended up finding and proving the form of the optimal policy for a special case where $$q(r) = p^r$$ for $$p \in [0,1]$$. Most strikingly perhaps was the recovery of an $$N/e$$ length "sampling" phase for modestly sized $$p$$ and $$N$$, just like in the the classic secretary problem! For the full details and the other cool results we discovered check out the paper below!

 <head>
    <title></title>
  </head>
  <body>
    <h1></h1>
    <object data="/assets/pdf/Mdp_project.pdf" type="application/pdf" width="100%" height="700px">
      <p>Unable to display PDF file. <a href="/uploads/media/default/0001/01/540cb75550adf33f281f29132dddd14fded85bfc.pdf">Download</a> instead.</p>
    </object>
  </body>
