---
layout: post
title: "Jane Street Puzzle: Andy's Afternoon Amble"
description: My solution to the 2026 August Puzzle
giscus_comments: true
tags: math
thumbnail: /assets/img/blog_images/2026-10-07-andys-amble/andys-amble-thumb.gif
date: 2026-09-01 12:00:00
published: true
math: true
---

# Problem Statement

<figure>

  <img src="/assets/img/blog_images/2026-10-07-andys-amble/andys-afternoon-amble.gif" alt="Truncated tetrahedral sphere and hexagonal floor tiling" width="80%" style="display: block; margin: auto;">
     <figcaption style="text-align: center; font-style: italic; font-size: 0.9em; color: #666;"> The puzzle's figure: Andy's truncated tetrahedral sphere (left) and the hexagonal kitchen floor (right). Image from Jane Street. </figcaption>

</figure>

>Andy the ant has moved on from his classic 'Telstar' soccer ball homeland to live on a simpler spherical surface consisting of four white hexagons that are surrounded by alternating black triangles and white hexagons (three of each), and four black triangles surrounded by three white hexagons. To us this land is a truncated tetrahedron blown up into a sphere we see above on the left. Due to Andy's tiny size and terrible eyesight, he doesn't notice the curvature of the land and avoids the black triangles because he suspects they may be bottomless pits.
>
>Much like his morning routine, every afternoon he wakes up from his nap on a white hexagon, leaves some pheromones to mark it as his special home space, and starts his random amble. Every step on this walk takes him to one of the three neighboring white hexagons with equal probability. He ends his amble as soon as he first returns to his home space, which he recognizes but cannot distinguish the edges of (i.e. he doesn't know if he returned across the same edge as he left). As an example, on exactly 1/3 of afternoons Andy's amble is 2 steps long, as he randomly visits one of the three neighbors, and then has a 1/3 probability of returning immediately to the home hexagon.
>
>This afternoon his truncated tetrahedral homeland bounced through the very same kitchen with an infinite regular hexagonal floor tiling consisting of black and white hexagons, shown above on the right. In this tiling every white hexagon is surrounded by alternating black and white hexagons, and black hexagons are surrounded by six white hexagons. Andy fell off the ball and woke up on a white hexagon. He didn't notice any change in his surroundings, and goes about his normal amble.

>Throughout his walk, Andy remembers the turns he's taken. Let $p$ be the probability that by the end of his afternoon amble on this new land he has discovered that he is no longer on the truncated tetrahedral sphere. **Find $p$ in exact terms.**

[Original puzzle on Jane Street's site](https://www.janestreet.com/puzzles/andys-afternoon-amble-index/)

---
# Solution

The first step is to figure out how to compare the walks in each space. Let the space of the truncated tetrahedron be denoted $T$ and the hexagonal tiling space be denoted $T'$. Let's begin by analyzing the walk on $T$, which is composed of four white hexagons and four black triangles. We can unfurl $T$ to lie in the plane as shown below. 

<figure>

  <img src="/assets/img/blog_images/2026-10-07-andys-amble/andy_amble_tetra2.png" alt="Andy's moves on the truncated tetrahedron" width="60%" style="display: block; margin: auto;">
     <figcaption style="text-align: center; font-style: italic; font-size: 0.9em; color: #666;"> Figure 1. The truncated tetrahedron $T$ unfolded into the plane, with Andy's home hexagon in green. The other three white hexagons all border home, and the lone black triangle at the top right is the one face that doesn't touch home. After Andy's first step to $A$, forward and backward moves (dashed) carry him around the cycle $A \to B \to C \to A$ while a right turn (red) from any of $A$, $B$, or $C$ takes him home. </figcaption>

</figure>

WLOG, assume Andy starts on the green hexagon and then always moves to hexagon $A$ and then faces towards hexagon $B$.  Since we are given that 
> throughout his walk, Andy remembers the turns he's taken

we need to find a way to model his movements throughout the rest of his walk. Given that he starts at $A$ facing $B$, we can model his path in the following way. Andy can make 3 types of "move": 
* Forward: This takes him along one of the black arrows in the diagram (e.g. $A \to B$)
* Backwards: He walks in *reverse* along the black arrow into his current hexagon (e.g. $B \to A$)
* Right: He turns right along one of the red arrows and returns home. 

All three moves are defined from Andy's own perspective. After his first step, Andy keeps track of which way he is facing, so at every hexagon the three exits are: the edge in front of him, the edge on his right, and the remaining edge. When he moves *forward*, he walks through the edge he is facing and then turns to face the exit on his left. A *backward* move undoes a forward move: he steps back through the remaining edge and turns to face the hexagon he just left. Turning *right* takes him through the edge on his right. Since each move depends only on Andy's heading and the order of the edges around him, his memory of his turns tells him exactly which moves he made, and with these three moves he can always work out where he is relative to the first hexagon he entered. 


For Andy to follow a sequence of turns, $S$, in the hexagonal tiling $T'$ and *not* notice that he left $T$, it must be the case that *$S$ returns Andy home in $T$ exactly once, and on the last move.* If Andy ever takes a move that would have returned him home in $T$ (i.e., turning right) but doesn't in $T'$, then he knows he is not on $T$.

What do moves look like in $T'$?

Again assume Andy steps to hexagon $A$ and faces $B$. His possible moves on $T'$ are shown in the figure below. 

<figure>

  <img src="/assets/img/blog_images/2026-10-07-andys-amble/andy_hexa_path.png" alt="Andy's moves on the hexagonal tiling" width="40%" style="display: block; margin: auto;">
     <figcaption style="text-align: center; font-style: italic; font-size: 0.9em; color: #666;"> Figure 2. Andy's moves on the hexagonal tiling $T'$. Forward and backward moves keep him on the ring $A, B, \ldots, F$ around a black hexagon. A right turn (red) leads home only from $A$; from anywhere else it leads off the ring to a hexagon that isn't home. </figcaption>

</figure>

Why does $T'$ look different? Locally, the two lands are identical: every white hexagon has three white neighbors spaced 120 degrees apart, so any sequence of turns can be replayed on either one. The only difference is the black tiles. A black triangle on $T$ is ringed by 3 white hexagons, while a black hexagon on $T'$ is ringed by 6. So the ring $A, B, \ldots, F$ wraps *twice* around the cycle $A, B, C$. For example, at $D$, Andy's memory says he's back at $A$.

This means that, unlike before, in $T'$, $A$ is the *only* position from which Andy can turn right and be in the home hexagon. The first time Andy turns right while not in hexagon $A$, he will notice he is not home and can tell he is in $T'$. Thus it now suffices to find the probability that the first time Andy turns right will be when he is in hexagon $A$. 

We can set this up as a recurrence relationship (this is a random walk on a Markov chain). Let $p_i$ be the probability that Andy finishes his walk without noticing he is in $T'$, given he is currently in hexagon $i$, $i = A, B, \ldots, F$. From any hexagon on the ring, he moves forward, backward, or right, each with probability $\frac{1}{3}$. Turning right from $A$ takes him home undetected, while turning right from anywhere else gives him away. Reflecting the ring across the line through $A$ and $D$ gives the symmetries $p_B = p_F$ and $p_C = p_E$. We get the following system of equations

$$
\begin{aligned}
p_A &= \frac{1}{3} + \frac{1}{3} p_B + \frac{1}{3} p_F = \frac{1}{3} + \frac{2}{3} p_B\\ 
p_B & = \frac{1}{3} p_A + \frac{1}{3}p_C \\ 
p_C & = \frac{1}{3} p_B + \frac{1}{3}p_D \\
p_D & = \frac{1}{3} p_C + \frac{1}{3} p_E = \frac{2}{3}  p_C.
\end{aligned}
$$

A little algebra (or Mathematica 😅) later and we find, 
$$
p_A = \frac{9}{20}.
$$

However, note that this is the probability that Andy has a sequence of moves that *doesn't* result in him finding out he is on $T'$. The puzzle asks for the complement, and thus our answer is 
$$
\begin{equation*}
\boxed{p = 1- p_A = \frac{11}{20}}.
\end{equation*}
$$