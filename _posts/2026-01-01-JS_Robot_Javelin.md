---
layout: post
title: "Jane Street Puzzle: Robot Javelin"
description: My solution to the 2025 December Puzzle
giscus_comments: true
tags: 
date: 2025-12-30 00:00:01
published: false
math: true
---

# Problem Statement


>It’s coming to the end of the year, which can only mean one thing: time for this year’s Robot Javelin finals! Whoa wait, you’ve never heard of Robot Javelin? Well then! Allow me to explain the rules:
* It’s head-to-head. Each of two robots makes their first throw, whose distance is a real number drawn uniformly from $[0, 1]$.
* Then, without knowledge of their competitor’s result, each robot decides whether to keep their current distance or erase it and go for a second throw, whose distance they must keep (it is also drawn uniformly from $[0, 1]$).
* The robot with the larger final distance wins.

>This year’s finals pits your robot, Java-lin ($J$), against the challenger, Spears Robot ($S$). Now, robots have been competing honorably for years and have settled into the Nash equilibrium for this game. However, you have just learned that Spears Robot has found and exploited a leak in the protocol of the game. They can receive a single bit of information telling them whether their opponent’s first throw (distance) was above or below some threshold $d$ of their choosing before deciding whether to go for a second throw. Spears has presumably chosen d to maximize their chance of winning — no wonder they made it to the finals!

>Spears Robot isn’t aware that you’ve learned this fact; they are assuming Java-lin is using the Nash equilibrium. **If you were to adjust Java-lin’s strategy to maximize its odds of winning given this, what would be Java-lin’s updated probability of winning?**

---
# Solution

This problem naturally has three components:

* What is the Nash Equillibrium strategy of the vanilla game?
* Given that $S$ can see if $J$'s throw was more or less than $d$, what is the their optimal $d^*$?
* Knowing that $S$ will play optimally with their new information, but doesn't know $J$ is aware of $S$'s strategy, what is the optimal strategy for $J$ now? 

## Part 1: Vanilla Nash Equillibrium

We will assume that both robots are using a threshold strategy and, since the game is symmetric for both robots here, they both adopt the threshold of $\tau$. That is, both robots throw their first javelin. If they throw it further than distance $\tau$ then they keep their first throw, otherwise they throw again and must keep their second throw. For this first part we would like to find the value of $\tau$.

Recall that in a Nash Equillibrium, no player can unilatteraly change their strategy and improve their position. So  WLOG let's assume that $J$ plays $\tau$ and $S$ plays $t$. We would like to find the value of $\tau$ such that for all $t \in [0, 1]$, $S$'s probability of winning is maximized at $t = \tau$.

To do this, we first need an expression for the probablility that $S$ wins given they use strategy $t$ and $J$ uses strategy $\tau$. Suppose the final value $S$ rolls is the random variable $X$ and the final value $J$ rolls is the random variable $Y$. We want to determine $\Pr[X > Y]$. We can write down the CDF of $X$ as a function of $x$ and $t$:

$$\begin{equation}
F_X(x, t) = \begin{cases}
 t x & 0\leq x\leq t \\
 (x-t) + x t & t<x<1.
\end{cases}
\end{equation}
$$
This follows from the fact that the only way to roll at most $x$ for $x \leq t$ is to roll under $t$ the first roll, then under $x$ the second. And for $x \geq t$ we can roll between it and $t$ on our first roll with probability $x - t$ or below $t$ on the first roll and then under $x$ the second. By the same logic, it follows.

$$\begin{equation}

F_Y(y) = \begin{cases}
 \tau y & 0\leq y\leq \tau \\
 (y-\tau) + y \tau & \tau<y<1.
\end{cases}
\end{equation}
$$

With these, we can write the $\Pr[X > Y]$ as a function of $t$. 

$$
\begin{equation}
\begin{aligned}
\Pr[X > Y] & = \int_{0}^1 F_Y(x) f_x(x, t) \; dx \\
& = \begin{cases}
 \frac{1}{2} \left(\tau ^2-\tau -\tau  t^2+\left(\tau ^2-\tau +1\right) t+1\right) & 0\leq t \leq \tau \\
\frac{1}{2} \left(-\tau -\left((\tau +1) t^2\right)+\left(\tau ^2+\tau +1\right) t+1\right) & \tau<t<1.
\end{cases}
\end{aligned}\label{eq:van_nash}
\end{equation}
$$


Now, we would like to find the value of $\tau$ such that for all $t \in [0,1]$ the probability of winning is maximized at $t = \tau$. If we vary $\tau$ and plot the probability of winning as a function of $t$, we can see that there is some $\tau$ for which the function is strictly decreasing in $t$ on either side of $t = \tau$.

<figure>
  <center>
  <img src="/assets/img/blog_images/2026-01-01-robot-javelin/P0_tau_animation.gif" width="60%" style="display: block; margin: auto;">
     <figcaption style="text-align: center;"> Figure 1.  </figcaption>
    </center>
</figure>

Taking the derivative of (\ref{eq:van_nash}) in $t$, setting $t = \tau$, and then solving for the root in $\tau$ yields a value of 

$$
\begin{equation}
\boxed{\tau = (\sqrt{5} - 1)/2}
\end{equation}$$
which is the unique Nash-equillibrium threshold strategy in the vanilla game and aligns with the value we would expect from Figure 1.

## Part 2: Optimal Cheating

The next step is to determine what value of $d$ player $S$ should ask for if they thought $J$ was using the vanilla strategy of $\tau$. Here $S$ essentially has two pieces of information: their initial roll value and whether or not $J$ rolled higher or lower than $d$. From those pieces of information, they must decide to reroll or stay. Similar to the previous part, we will develop an expression for the probability $S$ winning when $d \leq \tau$ and $d > \tau$ and using those, determine the optimal value of $d$.
### Case 1: $d < \tau$
Let us first suppose that $d \leq \tau$ and that $S$'s initial throw was $y$. We can break up the probability of $S$ winning by first considering if $S$ observes that $J$'s first throw $X_1$ was more or less than $d$. If $X_1 < d \leq \tau$, then $S$ knows that $J$ will rethrow. In that case, $S$ will `keep` if their initial throw was at least $1/2$ and rethrow otherwise. Therefore the probability of $S$ winning if they initially roll $y$ and see that $X < d$ is $P_{lo} = \max(y, 1/2)$ where the first term represents keeping their initial throw of $y$ and the second is the event they both rethrow. On the other hand, if $X_1 > d$ then since $d \leq \tau$, $J$ may rethrow or keep. We $S$ `keeps` here, then they win with probability 

$$
\begin{equation}P_{keep}(y) = \underbrace{\max \left(0,\frac{y -\tau }{1-\tau }\right)\frac{1-\tau }{1-d} }_{\text{$S$ keeps $| X_1 > d$}} + \underbrace{ y\frac{(\tau -d)}{1-d}}_{\text{$S$ rethrows $| X_1 > d$}}
\end{equation}
$$

By the same conditioning logic, we find that if $S$ `throws` then they win with probability

$$
\begin{equation}P_{throw} = \underbrace{\frac{1-\tau }{2}\frac{1-\tau}{1-d}}_{\text{$S$ keeps $| X_1 > d$}} + \underbrace{\frac{1}{2}\frac{\tau  - d}{1-d}}_{\text{$S$ rethrows $| X_1 > d$}}
\end{equation}
$$

Depending on the value of $y$, $S$ will choose the larger of the two previous options. Putting it together, integrating these cases over values of $y$, the probability of $S$ winning for a given $d \leq \tau$ is 


$$
\begin{equation}
\begin{aligned}
P_{Win (d < \tau)} & = \int_{0}^1 d \cdot P_{lo} + (1 - d)\max\bigg(P_{keep}(y), P_{throw}(y) \bigg)\\
                   & = \frac{\left(19-5 \sqrt{5}\right) d^2-\left(11 \sqrt{5}+1\right) d+4 \left(\sqrt{5}+3\right)}{4 \left(-2 d+\sqrt{5}+1\right)^2}
\end{aligned}
\end{equation}
$$
#### Case 2: $d > \tau$
Now, we must find the probability of winning for $d > \tau$. We can develop similar expressions as before. If $S$ observes that $X_1 \geq d \geq \tau$, then they will know $J$ will `keep`. If $S$ `throws` they win with probability $(1 - d)/2$, and if they `keep`, they win with probability $\max(0, (y - d)/(1 - d))$. Putting these together, in the event that $S$ observes that $J$ throws more than $d$, $S$ wins with probability 

$$
\begin{equation}
P_{hi} = \max\bigg((1 - d)/2, (y - d)/(1 - d)\bigg)
\end{equation}
$$

Next, in the event $S$ observes $X_1 < d$, then $J$ may throw again or keep, so we need to use the same conditioning logic as in the previous case. Doing so, we arrive at 


$$
\begin{equation}
P_{keep}(y) = \begin{cases}
\frac{\tau}{d} y & 0 \leq y \leq \tau \\
\frac{\tau}{d} y  + \frac{y - \tau}{d} & \tau < y \leq d \\
\frac{\tau }{d} y + 1 -\frac{\tau }{d} & y \geq d \\
\end{cases} 
\end{equation}
$$


$$
\begin{equation}P_{throw} = \underbrace{\frac{1-\tau }{2}\frac{1-\tau}{1-d}}_{\text{$S$ keeps $| X_1 > d$}} + \underbrace{\frac{1}{2}\frac{\tau  - d}{1-d}}_{\text{$S$ rethrows $| X_1 > d$}}
\end{equation}
$$







<!-- We can solve this problem by modeling it as finite-state Markov chain. Let the states be given by the set $$ S = \left\{(b, s) : s \in \{0,\ldots,3\}, b \in \{0,\ldots,4\}\right\}$$ where $(b, s)$ represents the state with $b$ balls and $s$ strikes. Depending on the decisions made by the pair of robots at a given state (`swing` or `wait` for the Batter and `strike` and `ball` for the Pitcher respectively), we transition to one of several possible states, each with their own transition probability.

If we knew the transition probabilities between all states as a function of $p$, then we can tune $p$ such that the probability of reaching a full count from the initial state $(0,0)$ is maximized. Thus we will approach the remainder of the problem in three steps:

* For a given state $(b, s)$ determine the optimal strategies for each player. 
* Given the optimal strategies, determine the transition probabilities between states in the induced Markov chain.
* Determine the probability of reaching a full count and then select $p$ as to maximize it.

### Optimal Strategies
Let $V(b, s)$ be the expected score of the Batter at state $(b, s)$. For a given state, we have a subgame where the Batter is trying to maximize the  expected score and the Pitcher to minimize it. What are the optimal strategies for this subgame? 

We can describe the strategies of the players using the concept of mixed-strategies. Let $x \in [0,1]$ and $y \in [0, 1]$ be the probabilities that the Batter  plays `swing` and the Pitcher plays `strike` at state $(b,s)$ respectively. How can we determine what these two ought to be in an equilibrium strategy? Recall that in a [Nash equilibrium](https://en.wikipedia.org/wiki/Nash_equilibrium), neither player can increase their objective by altering their current strategy. By the nature of our game, we can reason that neither player should use a pure strategy, as the opponent (or the player) could then deviate and improve their payout (Think rock, paper, scissors here -- If your opponent always plays `rock` you should drop whatever strategy you are using and play `paper`). So what does a mixed strategy equilibrium look like in this case, does one even exist?

It turns out that, yes, one does exist! Since the subgame has a finite number of players (two) and a finite number of pure strategies (two each), it famously follows that the game will indeed have [at least one Nash equillibrium in mixed-strategies](https://en.wikipedia.org/wiki/Nash_equilibrium#Existence). Now, can we find it?

Suppose that $(x, y)$ forms a mixed-strategy Nash equilibrium and that the Batter was considering changing $x$ in an attempt to increase their score. The expected payoff to the Batter using strategy $x$, for fixed $y$, is given by:

$$
\begin{equation}
\begin{aligned}
V(b, s | x) & = x \underbrace{\bigg(y(4 p  + (1 - p) V(b, s + 1) ) + (1 - y) V(b, s + 1)\bigg)}_{\text{Expected score from SWING}} \\
& \qquad + (1 - x)\underbrace{\bigg(y V(b, s + 1) + (1 - y) V(b + 1, s)\bigg)}_{\text{Expected score from WAIT}}.
\end{aligned}\label{eq:payoff1}
\end{equation}
$$ 

To determine their strategy $y$, the Pitcher should choose $y$ such that the Batter cannot do better by deviating from $x$. For *fixed* $y$, we can write (\ref{eq:payoff1}) as 

$$
\begin{equation}
V(b, s | x)  = x C_1(y) + (1 - x) C_2(y) \label{eq:payoff2}
\end{equation}
$$ 

where $C_1(y), C_2(y)$ are constants that depend on $y$. Observe that (\ref{eq:payoff2}) is maximized at $x = 0$ when $C_1(y) < C_2(y)$, $x = 1$ when $C_1(y) > C_2(y)$, and any $x \in [0, 1]$ when $C_1 = C_2$. From this observation, it follows that if the Pitcher were to set $y$ such that $C_1(y) = C_2(y)$, then the Batter cannot improve their payoff by deviating from $x$.

We can solve for $y$ such that $C_1(y) = C_2(y)$. Doing so, we find that 

$$
\begin{equation}
\boxed{y^*(b, s) = \frac{V(b + 1,s)-V(b,s + 1)}{V(b + 1,s) - V(b,s + 1) + p(4 - V(b,s+1))}}. \label{eq:y_strat}
\end{equation}
$$

From (\ref{eq:y_strat}), since for any $(b, s) \in S$, we have that $V(b, s) \leq 4$ and $V(b + 1,s) \geq V(b,s + 1)$, it follows that $y(b, s) \in [0,1]$, which implies $y$ is a well-defined probability value.

Now, since the Batter and Pitcher share the same objective function, but different optimization directions, we can apply the same logic to ensure that the Pitcher cannot improve their payoff by deviating from $y(b, s)$, to find the corresponding Nash equilibrium strategy of

$$
\begin{equation}
\boxed{x^*(b, s) = y^*(b, s) }. \label{eq:x_strat}
\end{equation}
$$

Now that we have found the unique Nash equilibrium strategy pair in this game [^1] . We need to apply backwards induction using our state payoffs $V$ to find the exact value of each $x(b, s)$.

As mentioned in the problem statement, the payout to the Batter for striking out is $0$, for getting a walk is $1$, and for hitting a Homerun is $4$. Therefore the payouts are $V(b, 3) = 0$, $V(4, s) = 1$. We can solve for the Batter score function under the equilibrium strategies using the Mathematica code below.

```mathematica
B = 4;
S = 3;
(* Base Cases *)
V[b_ /; b == B, s_] := 1 
V[b_, s_ /; s == S] := 0

(*Define equilibrium strategies*)
x[b_, s_] := 
 x[b, s] = 
  Simplify[(V[b + 1, s] - V[b, s + 1])/(
   4 p - (1 + p) V[b, s + 1] + V[b + 1, s])]

(*Define Payoff recurrence under optimal strategy*)
V[b_, s_] := V[b, s] = x[b, s]^2 (4  p + (1 - p) V[b, s + 1]) + 
    2 x[b, s] (1 - x[b, s]) V[b, s + 1] + (1 - x[b, s])^2 V[b + 1, s]
```

Note $V(b, s)$ and $x(b, s)$ only rely on states that are further along in the count (namely $V(b +1, s)$ and $V(b, s + 1)$) so we can use backwards induction to solve for them.

### Transition Probabilities

Given that we now have a system to compute the optimal strategies and valuations of each state, to complete our Markov chain, we need to determine the transition probabilities between states.

We will have the absorption states: $(b, 3)$ and $(4, s)$ for $b \in[3]$ and $s \in [2]$ which represent the outcomes of `strikeout` and `walk` respectively, and an auxiliary state that represents a `Homerun`, which can be reached from any non-terminal state in the game. We visualize the Markov chain below in Figure 1 (omitting the `Homerun` state for clarity).

<figure>
    <img src="{{ '/assets/img/blog_images/2025-11-01-robot-baseball/MC.png' | relative_url }}" width="70%" style="display: block; margin: auto;">
    <figcaption style="text-align: center;">Figure 1: Plot of the states of the Markov chain and their possible transitions. The Homerun state has been omitted but every state has the probability to end the game in a Homerun. The full-count state has been highlighted in <span style="color:green">green</span>. </figcaption>
</figure>

Given that we are at the non-absorbing state $(b, s)$, we can either transition to $(b + 1, s)$, $(b, s + 1)$ or the `Homerun` state. Let $Q(b, s)$ be the probability of reaching a full count given we are at state $(b, s)$. We can write $Q(b,s)$ as

$$Q(b, s) = {x}^2 \bigg(p \cdot 0 + (1-p) Q(b, s + 1)\bigg) + 2x(1-x) Q(b, s + 1) + (1 - x)^2 Q(b + 1, s)  $$

where $x$ is the Nash-equilibrium strategy given in (\ref{eq:y_strat}).  The base cases for our function $Q$ are  $Q(3, 2) = 1$ and $Q(b, s) = 0$ for any terminal state. For a given $p$, we can now use dynamic programming to compute $Q(0, 0)$, using the snippet of Mathematica code below. We want to know the value of $p \in [0,1]$ that maximizes the value of $Q(0, 0)$.

```mathematica
(* Base Cases *)
Q[b_ /; b == B - 1, s_ /; s == S - 1] := 1 
Q[b_ /; b == B, s_] := 0
Q[b_, s_ /; s == S ] := 0

(*General recurrence *)
Q[b_, s_] := Q[b, s] = x[b, s]^2 (1 - p) Q[b, s + 1] + 
   2 x[b, s] (1 - x[b, s]) Q[b, s + 1] + (1 - x[b, s]) ^2 Q[b + 1, s]
```

### Optimizing for a Full-Count.

We can plot $Q(0,0)$ as a function of $p$.

<figure>
    <img src="{{ '/assets/img/blog_images/2025_08_01-robot-roadtrip/image.png' | relative_url }}" width="70%" style="display: block; margin: auto;">
     <figcaption style="text-align: center;">Figure 2: The probability of reaching a full-count as a function of Homerun probability $p$.</figcaption>
</figure>

Based on the figure, we can see the probability is unimodal in $p$ and we can use our favorite `Binary Search` method to determine the $p$ that maximizes our function. Alternatively, we can ask Mathematica to do this for us!

```mathematica
(* Maximize Q[0,0] over the interval [0,1] *)
NMaximize[{Q[0, 0], p >= 0, p <= 1}, p, WorkingPrecision -> 20]
```

Which produces a value of $\boxed{p^* \approx 0.2269732325}$ and the probability of reaching a full count in this case $\boxed{q \approx 0.2959679934}$, which is the answer to this month's puzzle!

For fun, I experimented with changing the value of the `Homerun` state. In the given description, the value of a `Homerun` is 4 which implies the Batter has the bases loaded! What if this was instead a lower stakes scenario for the pitcher, and there were instead fewer points on the line if the Batter hit a `Homerun`. Below are the probabilities of reaching a full count, for various numbers of runners on base, as a function of $p$. We can see that with more players on base, the lower the value of $p$ needs to be to maximize the odds of reaching a full count!

<figure>
  <center>
  <img src="/assets/img/blog_images/2025-11-01-robot-baseball/q_evolution.gif" width="60%" style="display: block; margin: auto;">
     <figcaption style="text-align: center;"> Figure 3. The probability of reaching a full-count given different numbers of players on base. </figcaption>
    </center>
</figure>

[^1]: Can you prove it? -->


