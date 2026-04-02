---
layout: post
title: "Jane Street Puzzle: Planetary Parade"
description: My solution to the 2026 March Puzzle
giscus_comments: true
tags: math
thumbnail: assets/img/blog_images/2026-04-01-planet-parade/pp-3d-case.png
date: 2026-04-01 00:00:01
published: true
math: true
---

# Problem Statement



>We are enjoying a planetary parade during the beginning of this month here on Earth, but my alien friend from the distant planet Pyrknot (well modeled as a perfect sphere) isn't too impressed. Their single-star system has $n$ planets other than Pyrknot with such chaotic orbits that each night these planets independently appear in uniformly random locations in the Pyrknothian sky. Note that *the planets aren't visible in daylight*. If at a given moment there exists somewhere on Pyrknot that all $n$ planets are visible, then from my friend's position on the surface they have an $\alpha$ probability of also seeing all of the planets.

>My friend is considering building a tower to improve their chances of seeing these planetary parades (parade). Assume the tower allows the viewing of every celestial body that could be seen from at least one location on the surface of Pyrknot less than a distance $r$ from the base of the tower. In the limit of $r$ being small compared to the radius $R$ of Pyrknot, the new probability of seeing the planetary parade from the top of the tower is linearly approximated by $\alpha + \beta\cdot(r/R)$. **Find $\alpha$ and $\beta$ in exact terms**.


---
# Solution

We will solve this problem in a few parts, looking at the 2D and 3D versions of the problem. First, we will solve for $\alpha$, which turns out to be dimension-agnostic. Next, we will solve the problem in 2D (that is, for a circular planet whose universe is the plane) to build some intuition. Then, we will use our findings from the 2D case to construct the answer to the 3D version. 

--- 

## Part 1: Finding $\alpha$ — a dimension-agnostic result
The key quantity throughout is $\alpha$, the probability of seeing the parade without a tower, given that a parade is taking place somewhere on the planet. 

To start, let's assume that our friend's location on the planet is at the North Pole (NP). Assume that for our modeling purposes the celestial bodies in the system (not including Pyrknot) lie on some other sphere that is very far from the planet. For our purposes, we can say that the position of a planet in the sky can be mapped to a position on the surface of Pyrknot by drawing a line between the planet and the center of Pyrknot. That way, we can model the positions of celestial bodies around Pyrknot as positions on the surface of Pyrknot. 

Now, given that a parade exists, what must be true for it to be visible from the NP? We have two conditions that must be satisfied:

* It is night time at the NP (otherwise the parade is not visible)
* All $n$ planets must be in the hemisphere centered at the NP. 

We need to find the probability that, at a given moment, we can see the parade from the NP given that one exists. Using Bayes' theorem, we can write this as

$$
\begin{equation}\label{eq:Bayes}
\begin{aligned}
\alpha & := \Pr[\text{Parade visible from NP}|\text{Parade exists}]\\
 & = \frac{\Pr[\text{Parade exists}|\text{Parade visible from NP}] \cdot\Pr\left[\text{Parade visible from NP}\right]}{\Pr\left[\text{Parade exists}\right]}    \\ 
& = \frac{1 \cdot \Pr\left[\text{Parade visible from NP}\right]}{\Pr\left[\text{Parade exists}\right]}.  \\
\end{aligned}
\end{equation}
$$

The middle factor simplifies to 1 because if the parade is visible from the NP, it certainly exists somewhere on the planet.

Thus, it suffices for us to find $\Pr\left[\text{Parade visible from NP}\right]$ and $\Pr\left[\text{Parade exists}\right]$.

To find the probability that in a given moment a parade on $n$ planets is visible from the north pole, we need all the planets to be in the northern hemisphere and the star to be in the *opposite hemisphere*. Since the positions of all $n$ planets and the star are independent and uniform, each planet lies in the northern hemisphere with probability $\frac{1}{2}$ and the star lies in the southern hemisphere (making it night at the NP) with probability $\frac{1}{2}$, giving
$$
\begin{equation}
\Pr\left[\text{Parade visible from NP}\right] = \frac{1}{2^{n + 1}}. \label{eq:NP_visibility}
\end{equation}
$$

It is a bit more nuanced to find the probability that a parade exists somewhere on the surface of the planet since there could be many places where the parade is visible. However, there exists a well-known result that can help us out: [Wendel's theorem](https://en.wikipedia.org/wiki/Wendel%27s_theorem), which gives us the probability that $n+1$ points randomly placed on the surface of a $d$-dimensional sphere are contained within some shared hemisphere. To use it within the context of our problem, we create a pseudo-planet that is placed antipodal to the location of the star. That way, we can find the probability that the $n$ planets and the pseudo-planet all exist in some shared hemisphere, which is equivalent to all $n$ planets being visible from some point while the star is not.

Wendel's theorem states that in dimension $d$ with $n + 1$ points, the probability those points all lie in some shared hemisphere is: 
$$
\begin{equation}
\label{eq:parade_exists}
\begin{aligned}
\Pr\left[\text{Parade exists}\right] = \frac{2 \sum_{k = 0}^{d - 1} \binom{n}{k}}{2^{n + 1}}. 
\end{aligned}
\end{equation}
$$

If we combine equations (\ref{eq:Bayes}), (\ref{eq:NP_visibility}), and (\ref{eq:parade_exists}), we find that

$$
\begin{equation}
\label{eq:alpha}
\begin{aligned} 
\alpha & = \frac{ \Pr\left[\text{Parade visible from NP}\right]}{\Pr\left[\text{Parade exists}\right]}\\ 
& = \frac{\frac{1}{2^{n + 1}}}{\frac{2 \sum_{k = 0}^{d - 1} \binom{n}{k}}{2^{n + 1}}}\\ 
& = \boxed{\frac{1}{2 \sum_{k = 0}^{d - 1} \binom{n}{k}}},
\end{aligned}
\end{equation}
$$
which is the reciprocal of the number of regions created by $n + 1$ non-degenerate great circles on a sphere in $d$ dimensions!

Next, we will introduce the tower and solve for $\beta$ in the case of $d = 2$.


--- 

## Part 2: A tower on a planet in the plane-universe 

We now construct a tower at the north pole that allows us to see any body that would otherwise be visible from some point within distance $r$ from the base of the tower. In the limit of $r \ll R$, this is essentially a small arc on the circular planet of arclength $2r$. How does this affect our chances of seeing a parade? On one hand, it gives us a greater likelihood of seeing more planets in the sky at a given moment, but on the other hand, it also gives us a greater chance of seeing the star during that moment, which would ruin the parade. 

Suppose we are just isolating the effect for a single planet $P$. Given that we have our tower of effective radius $r$, what is the probability of seeing the planet from the tower? 

Note that *without* the tower, the position of $P$ must lie above the equator. With the tower, we can now see all planets whose position dips slightly below the equator. Let $\theta$ be the angular diameter of the area that the tower can effectively view from. If we draw the diameters of the semicircles that are centered on the two edge points of our viewing region (red and blue points in Figure 1, respectively), we can see that the effective region in which planets are visible (yellow region in Figure 1) includes the northern hemisphere and two sectors of angle $\omega$ that dip below the equator. 

<figure>
  <center>
  <img src="/assets/img/blog_images/2026-04-01-planet-parade/pp-2d-case.png" width="60%" style="display: block; margin: auto;">
     <figcaption style="text-align: center; font-style: italic; font-size: 0.9em; color: #666;"> Figure 1. The tower's effective viewing region in the 2D (circular planet) case. Without the tower, a planet must lie in the northern hemisphere to be visible. The tower extends this to the full yellow region, adding two sectors of angle $\omega = \theta/2$ below the equator, centered on the edge points of the base arc (shown in red and blue). </figcaption>
    </center>
</figure>

Some elementary geometry yields that $\omega = \theta / 2$. This follows from the inscribed angle theorem: the angle $\omega$ subtended at a point on the circle by the base arc is half the central angle $\theta$ subtending the same arc. Therefore, the fraction of the circle covered by the tower is $\frac{\pi + \theta}{2 \pi}$, and for a given planet, the probability that it is visible from the tower is:

$$
\begin{equation}
\Pr\left[\text{Planet $P$ visible from tower}\right] = \frac{1 + \theta/\pi}{2}. \label{eq:eff_area_2}
\end{equation}
$$

Now, for a parade to be visible from the tower, we need all $n$ planets to be visible and the star not to be. 

Using the independence of the planets and the star along with (\ref{eq:eff_area_2}), we find

$$
\begin{equation*}
\begin{aligned}
\Pr\left[\text{Parade visible from tower}\right] & = \left(\frac{1 + \theta/\pi}{2}\right)^n \left(\frac{1 - \theta/\pi}{2}\right)\\ 
& = \frac{1}{2^{n+ 1}}(1 + \theta/\pi)^n (1 - \theta/\pi)\\
& \approx \frac{1}{2^{n+ 1}}(1 + n\theta/\pi) (1 - \theta/\pi) && (\text{apply binomial approx. for small $\theta$})\\
& = \frac{1}{2^{n+ 1}}\left(1 + (n - 1)\theta/\pi - \mathcal{O}(\theta^2)\right)\\
& \approx \frac{1}{2^{n+ 1}}(1 + (n - 1)\theta/\pi). && (\text{drop higher order dependence})
\end{aligned}
\end{equation*}
$$

If we note that $\theta = 2r/R$ and use our previous expressions for the conditional probability (\ref{eq:Bayes}) and existence of a parade (\ref{eq:parade_exists}), we find:

$$
\begin{equation*}
\begin{aligned}
\Pr[\text{Parade visible from tower}|\text{Parade exists}] & = \frac{\Pr\left[\text{Parade visible from tower}\right]}{\Pr\left[\text{Parade exists}\right]}\\
& \approx \frac{\frac{1}{2^{n+ 1}}(1 + (n - 1)\theta/\pi)}{\frac{2 \sum_{k = 0}^{1} \binom{n}{k}}{2^{n + 1}}} \\
& = \frac{1 + (n - 1)\theta/\pi}{2(n + 1)} && \left(\text{since } \sum_{k=0}^{1}\tbinom{n}{k} = 1 + n\right)\\
& = \frac{1}{2 (n + 1)} + \frac{n - 1}{2\pi(n + 1)} \cdot \theta\\
& = \frac{1}{2 (n + 1)} + \frac{n - 1}{2\pi(n + 1)} \cdot \left(\frac{2r}{R}\right)\\ 
& = \frac{1}{2 (n + 1)} + \frac{n - 1}{\pi(n + 1)} \cdot \left(\frac{r}{R}\right), \\ 
\end{aligned}
\end{equation*}
$$

which implies for $d = 2$, we find
$$
\begin{equation*}
\boxed{
\begin{aligned}
\alpha & = \frac{1}{2 (n + 1)}\\
\beta & = \frac{n - 1}{\pi(n + 1)}.\\
\end{aligned}}
\end{equation*}
$$

---

## Part 3: A Parade on the sphere

Since (\ref{eq:alpha}) was derived for arbitrary $d$, it applies directly to the 3D case — we need only find $\beta$. In 3D the effective viewing area for the tower is now a small cap of the sphere of angular diameter $\theta$, which is again given by $\theta = 2r / R$. Now, the tower, instead of extending the total observable area by two arcs of angle $\theta/2$, adds an additional ring of area below the equator, shown between the thick black equator and the green circle in Figure 2.

<figure>
  <center>
  <img src="/assets/img/blog_images/2026-04-01-planet-parade/pp-3d-case.png" width="60%" style="display: block; margin: auto;">
     <figcaption style="text-align: center; font-style: italic; font-size: 0.9em; color: #666;"> Figure 2. The tower's effective viewing region in the 3D (sphere) case. Rather than two sectors, the tower adds an annular band of surface area $2\pi R^2 \sin(\theta/2)$ below the equator (yellow region, bounded below by the green circle), which increases the probability that any given planet is visible from the tower. </figcaption>
    </center>
</figure>

To get the total observable area of the planet from the tower, we need to find the area $A(\theta)$ of the added ring. 

Using spherical coordinates (with $\phi$ the polar angle measured from the north pole and $\lambda$ the azimuthal angle), we can write this surface area as:

$$
\begin{equation*}
\begin{aligned}
A(\theta) & =\int_{0}^{2 \pi}\int^{\pi/2}_{\pi/2 - \theta/2} R^2\sin \phi \, d \phi \, d\lambda \\
& = 2 \pi R^2 \sin(\theta/2).
\end{aligned}
\end{equation*}
$$

Therefore, the total area of the planet viewable from the tower is $2 \pi R^2 + 2 \pi R^2 \sin(\theta/2)$ and thus the probability of viewing any given planet is

$$
\Pr\left[\text{Planet $P$ visible from tower}\right] = \frac{2 \pi R^2 + 2 \pi R^2 \sin(\theta/2)}{4 \pi R^2} = \frac{1 + \sin(\theta/2)}{2}.
$$

Now all that remains is to find the probability that all planets are visible from the tower and the star is not. Using the same approach as in the 2D case, we find:

$$
\begin{equation}
\label{eq:tower_prob_3d}
\begin{aligned}
\Pr\left[\text{Parade visible from tower}\right] & = \left(\frac{1 + \sin(\theta/2)}{2}\right)^n \left(\frac{1 - \sin(\theta/2)}{2}\right)\\ 
& = \frac{1}{2^{n + 1}}\bigl(1 + \sin(\theta/2)\bigr)^n \bigl(1 - \sin(\theta/2)\bigr)\\
& \approx \frac{1}{2^{n+ 1}}\bigl(1 + n\sin(\theta/2)\bigr) \bigl(1 - \sin(\theta/2)\bigr) && (\text{binomial approx. for small $\theta$})\\
& \approx \frac{1}{2^{n+ 1}}\bigl(1 + n\theta/2\bigr) \bigl(1 - \theta/2\bigr) && (\text{small angle approx. for $\sin$})\\
& = \frac{1}{2^{n+ 1}}\left(1 + (n - 1)\frac{\theta}{2} - \mathcal{O}(\theta ^2)\right)\\
& \approx \frac{1}{2^{n+ 1}}\left(1 + (n - 1)\frac{\theta}{2}\right). && (\text{drop higher order dependence})\\
\end{aligned}
\end{equation}
$$

Once again noting that $\theta = 2r/R$, and plugging (\ref{eq:parade_exists}) and (\ref{eq:tower_prob_3d}) into (\ref{eq:Bayes}), we find:

$$
\begin{equation*}
\begin{aligned}
\Pr[\text{Parade visible from tower}|\text{Parade exists}] & = \frac{\Pr\left[\text{Parade visible from tower}\right]}{\Pr\left[\text{Parade exists}\right]}\\
& \approx \frac{\frac{1}{2^{n+ 1}}\left(1 + (n - 1)\frac{\theta}{2}\right) }{\frac{2 \sum_{k = 0}^{2} \binom{n}{k}}{2^{n + 1}}} \\
& = \frac{1 + (n - 1)\frac{\theta}{2}}{n^2 + n + 2} && \left( 2\sum_{k=0}^{2}\tbinom{n}{k} = n^2+n+2\right)\\
& =\frac{1 + (n - 1)\frac{r}{R}}{n^2 + n + 2} \\
& = \frac{1}{n^2 + n + 2} + \frac{n - 1}{n^2 + n + 2}\cdot \left(\frac{r}{R}\right),
\end{aligned}
\end{equation*}
$$


which gives us our final answer:

$$
\begin{equation*}
\boxed{
\begin{aligned}
\alpha & = \frac{1}{n^2 + n + 2}\\
\beta & = \frac{n - 1}{n^2 + n + 2}.\\
\end{aligned}}
\end{equation*}
$$

---

## Remarks

**The tower is useless for $n = 1$.** In both 2D and 3D, $\beta = 0$ when $n = 1$. The intuition is symmetric: for every configuration where the tower's wider field of view lets you catch the single planet below the equator, there is an equally likely configuration where it causes you to see the star instead. These two effects cancel exactly at first order, so the tower provides no benefit.

**The tower always helps when $n > 1$.** For $n > 1$, $\beta > 0$ in both dimensions, meaning the tower strictly improves your odds. Intuitively, the tower's extended range creates $n$ new opportunities to gain (one per planet) but only one opportunity to lose (the star). When $n > 1$, the planets win on average.

**$\alpha$ decays much faster in 3D.** For large $n$, 

$$\alpha_{2D} \approx \frac{1}{2n}, \qquad \alpha_{3D} \approx \frac{1}{n^2}.$$

In 3D, there is so much more room in the sky that simultaneously fitting all $n$ planets into a hemisphere becomes quadratically harder. This reflects the general Wendel's theorem result: higher-dimensional spheres make hemispheric coincidences exponentially rarer.

**The tower's marginal benefit $\beta$ has different large-$n$ behavior by dimension.** As $n \to \infty$,

$$\beta_{2D} \to \frac{1}{\pi}, \qquad \beta_{3D} \to 0.$$

In 2D, the marginal gain per unit $r/R$ converges to a positive constant. Even with very many planets, the tower always helps at first order. In 3D, the parade itself becomes so rare that even the tower's extended range can't keep up.

