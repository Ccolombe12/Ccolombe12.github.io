---
layout: distill
title: A Generalization of The Classic Bugs on A Square Problem.
description: 
giscus_comments: true
date: 2023-12-16 00:00:01
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
<meta name="viewport" content="width=device-width, initial-scale=1.0">


## Problem Statement
In the classic bugs on a square problem, 4 bugs start out on the vertices of a square with side length 1. At $$t = 0$$ the bugs all begin moving with speed $$v$$ towards their counter-clockwise neighbor. The question is to determine how long the bugs take to reach the center of the square and how many rotations around the center will each bug make in doing so?
<h3><figure><center>
  <img src="/assets/img/blog_images/Bugs_On_A_Square/square.png">
 
</center></figure></h3>

## Solution

Let's first solve the square case and see if we can then generalize the argument. We will use polar coordinates and focus on the upper right bug. We can represent its position as a function of time as $$(r(t), \phi(t))$$, with initial conditions $r(0) = \sqrt{2}/2$ and $\phi(0) = \pi /4$.

The key here is to use symmetry to reason that at every moment in time, the bugs maintain their relative positions as the vertices of square. In otherwords, at each moment in time, the bugs remain in a square formation. 

With this in mind, we can consider the bug's position after a small change in time $\Delta t$. 
<h3><figure>
  <center>
  <img src="/assets/img/blog_images/Bugs_On_A_Square/square_dt.jpeg"
  style="width: 40vw; min-width: 330px;"
  zoomable=true/>
  <figcaption> Figure 2. A bug's relative position after a small moment in time $\Delta t$.
</figcaption> 
</center>
</figure></h3>
The bug moves a distance $v \Delta t$ towards the it's adjacent bug. We can then do some geometery to and use a small angle approximation to find the change in $r$ and $\phi$. Noting the right trangle we can form with hypotenuse $v \cdot \Delta t$, we have 

$$
\begin{align}
  r(t + \Delta t) - r(t) & = - v \cdot  \Delta t \cos \theta\\ 
  r(t + \Delta t) \cdot \Delta \phi & = v \Delta t \sin \theta
\end{align}
$$
Rearranging these and taking the limit as $\Delta t \to 0$, we obtain


$$
\boxed{
\begin{align}
  r'(t)& = - v \cos \theta\\ 
  \phi'(t) & = \frac{v \sin \theta}{r(t)}.
\end{align}}
$$

We can solve (3) for $$r(t)$$ using our initial condiditon  $\theta = \pi /4 $ and $$r(0) = 1/\sqrt{2}$$ to get

$$
\begin{equation}
  r(t) = - \frac{v}{\sqrt{2}}  t + \frac{1}{\sqrt{2}}.
\end{equation}
$$ 
It then follows that the time it takes to reach the center $T$ is $$T = \frac{1}{v}.$$ Now that we have an equation for $$r(t)$$ we can plug it into (4) and solve for $\phi(t)$ to get

$$
\begin{equation}
\boxed{\phi(t) = \pi/4 - \log\left( 1 - v t\right)}.
\end{equation}
$$
As $$t \to T $$, we have $$\phi \to \infty$$, implying that the bug makes an infinite number of rotations around the center of the circle as the approach the center!


This analysis was for the square but every step we used is actually applicable to the general case of a regular $n$-gon. In the general case $$\theta = 2 \pi / n$$ and we can use the law-of-cosines and then a double angle formula for cosine to determine that $$ r(0) = \frac{1}{\sqrt{2 \left(1 - \cos \left(\frac{2 \pi}{n} \right)\right)}} = \frac{1}{2 \sin \frac{\pi}{n}} $$. Consider one of the $$n$$ wedges from a regular $n$-gon shown below. Here the angle $$\alpha = \frac{\pi - 2\pi/n}{2} = \frac{\pi}{2} - \frac{\pi}{n}$$.


<h3><figure>



  <center>
  <img src="/assets/img/blog_images/Bugs_On_A_Square/n_gon.jpeg"
  style="width: 50vw; min-width: 330px;"
  zoomable=true/>
  <figcaption> Figure 2. A bug's relative position after a small moment in time $\Delta t$.
</figcaption> 
</center>
</figure></h3>

Using our same steps as before, we find the differential equations

$$
\begin{align}
   r'(t)& = - v \cos \alpha = - v \sin \frac{\pi}{n}\\ 
  \phi'(t) & = \frac{v \sin \alpha}{r(t)} = \frac{v \cos \frac{\pi}{n}}{r(t)}.
\end{align}
$$

We can solve these once again with our initial conditions to get 

$$\begin{equation}
  \boxed{r(t) = \frac{1}{2 \sin \left(\frac{\pi}{n}\right) }- v \sin \left(\frac{\pi}{n}\right)\;  t}.
\end{equation}$$
Which implies the time to reach the center for a bug on a regular $n$-gon is $$\boxed{T = \frac{1}{2 v \sin^2\left(\frac{\pi}{n}\right)}}$$. Lastly we solve for $$\phi(t)$$ to get 
$$
\begin{equation}
  \boxed{\phi(t) =  \frac{\pi}{n} - \cot\left(\frac{\pi}{n}\right) \log \left(1 - 2 v \sin^2 \left(\frac{\pi}{n}\right) t\right)}.
\end{equation}
$$
note again we recover that $$\phi(t) \to \infty$$ as $$t \to T$$ meaning the bug will always rotate around the center of the $n$-gon an infinite number of times as it approaches the center.

Can we say anything about these results for large $n$? We can make a rough approximation for large $n$ using the small-angle approximation for sine. For large enough $$n$$, $$
\begin{equation}T = \frac{1}{2 v \sin^2\left(\frac{\pi}{n}\right)} \approx \frac{1}{2 v \left(\frac{\pi}{n}\right)^2} = \frac{n^2}{2 \pi^2 v}
\end{equation}$$ which grows unboundely as $n$ increases.

<h3><figure>
  <center>
  <img src="/assets/img/blog_images/Bugs_On_A_Square/big_n_t.png"
  style="width: 50vw; min-width: 330px;"
  zoomable=true/>
  </center>
  <figcaption> Figure 3. The time it takes for a bug on an $n$-gon to reach the center for $v = 1$. The large $n$ approximation is shown in blue. We can see they agree rather quickly.
</figcaption> 

</figure></h3>

Lastly, it is fun to look at the actual path of the bug as a function of time for different $n$.Here are the paths to the center of the first bug for $$n = 4$$ and $$n=20$$.

<h3><figure>
  <center>
  <img src="/assets/img/blog_images/Bugs_On_A_Square/my_gif2.gif" style="width: 35vw; min-width: 330px;">
  <figcaption> Figure 4. Path a bug take to the center on the unit square.
</figcaption> 
</center>
</figure></h3>

<h3><figure>
  <center>
  <img src="/assets/img/blog_images/Bugs_On_A_Square/my_gif.gif" style="width: 35vw; min-width: 330px;" >
  <figcaption> Figure 5. Path a bug take to the center on the unit 20-gon. </figcaption> 
</center>
</figure></h3>














