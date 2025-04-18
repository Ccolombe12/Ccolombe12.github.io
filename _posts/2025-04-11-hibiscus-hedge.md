---
layout: post
title: "Can You Grow a Hibiscus Hedge?"
description: My solution to this week's Fiddler on the Proof Puzzle
giscus_comments: true
date: 2025-04-08 00:00:00
published: true
---


This week's [The Fiddler on the Proof](https://thefiddler.substack.com) puzzle is paraphrased as follows:



> **Part 1:**  You have many flowers in three colors: red, orange, and yellow. You want to plant them in a straight row such that the order appears somewhat random, but not truly random. More specifically, you want the following to be true:
* No two adjacent flowers have the same color.
* No ordering of three consecutive flowers appears more than once in the row.

>**What is the maximum number of flowers the row can contain?**

>  **Part 2:**  In addition to red, orange, and yellow flowers, you now have a fourth color: pink! Again, you want to plant a straight row of flowers that appears somewhat random. The new rules are:
* No two adjacent flowers have the same color.
* No ordering of four consecutive flowers appears more than once in the flower row. 
* Among any group of four consecutive flowers, at least three distinct colors are represented.

> **What is the maximum number of flowers the row can contain now?**

<br><br>

## **Solution**


### **Part 1:**

Let's first establish an upper bound on how long our row of flowers *could* be. Since each possible pattern of three flowers can appear only once, the longest imaginable row contains each pattern exactly once. The total number of patterns can be calculated:
* $$3$$ choices for the first color
* $$2$$ for the second
* $$2$$ for the final color

Which gives the grand total of $$12$$ total color patterns. As each pattern overlaps by two flowers, the longest possible row length is $$14$$. But, can we achieve this maximum?

We verify using backtracking with Python

```python
from matplotlib.patches import Rectangle
# lets write a function that, given the current state, sees how many flowers we can place
colors = ['r', 'o', 'y']

# we need to write some helpers that allow us to check if we can place a color 
# given the current sequence of colors so far 

# check the triplet formed is not a repeat 
def no_triplet(state, color):
    if len(state) < 3:
        return True
    else:
        cur_triplet = state[-2:] + color
        for i in range(len(state) - 2):
            if state[i:i+3] == cur_triplet:
                return False
        return True

# check for adjacent color and not a new triplet 
def can_place(state, color):
    if state[-1] != color and no_triplet(state, color):
        return True
    return False

# returns the max len flower array given the current placement
def max_flower(state):
    best = len(state)
    best_flower = state
    for color in colors:
        if can_place(state, color):
            cur_flower, cur_size = max_flower(state + color)
            if cur_size > best:
                best = cur_size
                best_flower = cur_flower
    
    return best_flower, best

# just assume the base flower is 'ro'
best_flower, best_size = max_flower('ro')
print('Best flower:', best_flower)
print('Best size:', best_size)


# Now lets visualize the best flower as a sequence of rectangles 
import matplotlib.pyplot as plt

color_mapping = {'r': 'red', 'o': 'orange', 'y': 'yellow'}

fig, ax = plt.subplots(figsize=(len(best_flower), 2))
for idx, letter in enumerate(best_flower):
    rect = Rectangle((idx, 0), 1, 1, color=color_mapping[letter])
    ax.add_patch(rect)
    
ax.set_xlim(0, len(best_flower))
ax.set_ylim(0, 1)
ax.axis('off')
plt.show()
            
```
Which gives us the output and visualized longest flower:
```plaintext
Best flower: roroyryoyoryro
Best size: 14
```


<figure>
  <center>
  <img src="/assets/img/blog_images/2025-04-11-hibiscus/bush.png" width="80%" style="display: block; margin: auto;">
  <figcaption> An example of an optimal flower row of length 14 
</figcaption> 
</center>
</figure>

Confirming that we can indeed reach the longest possible flower length of $$\boxed{14}$$ under the conditions!

## **Part 2**

For the extra credit, we can still use the backtracking approach but given the increased state space size, it might take a while. We can try another approach to handle the larger state space *integer programming* with the [z3 module](https://www.microsoft.com/en-us/research/project/z3-3/). With this approach, we will answer the [decision problem](https://en.wikipedia.org/wiki/Decision_problem):

> for a given $$n$$, does there exist a feasible flower row of length $$n$$?

Using the answer to this decision problem, we can *binary search* over the space of the possible lengths to find the largest $$n$$ such that there exists a feasible flower row.

Before we implement this, we need to again place an upper bound on the largest possible flower row so that we know the range of sizes we are to search over. Using a similar logic as in Part 1, along with the additional constraint that for any $$4$$ adjacent flowers we need at least $$3$$ distinct colors, we can show that there are $$96$$ such possible color patterns. Therefore the largest possible flower row has size $$99$$. Now it remains to model the decision procedure with the z3 module in python and then binary search over the space of possible lengths $$[4, 96]$$.

```python
from z3 import *
from itertools import combinations

def flower_problem(n, k=4):
    """The decision procedure: returns True if there exists a feasible flower row
    of size n with k colors. """
    # The n decision variables (color of each flower)
    x = [Int(f'x_{i}') for i in range(n)]
    
    # Constraints:
    s = Solver()
    # 1)  Each flower must have a color 
    for i in range(n):
        s.add(And(x[i] >= 0, x[i] <= k - 1))
    
    # 2) No two adjacent colors match
    for i in range(n - 1):
        s.add(x[i] != x[i+1])
    
    # 3) no repeated k-patterns 
    for i in range(n - k + 1):
        for j in range(i + 1, n - k + 1):
            # Build the disjunction: at least one position l in {0,..., k-1} is different.
            diff = [x[i + l] != x[j + l] for l in range(k)]
            s.add(Or(diff))
    
    # 4) Each k-block must contain at least k-1 distinct colors.
    for i in range(n - k + 1):
        block = x[i:i+k]
        # we need to check all combinations of k - 1 colors in the block
        t = []
        for c in combinations(range(k), k - 1):
            t.append(Distinct([block[i] for i in c]))
        s.add(Or(t))
    
    # Try to solve 
    if s.check() == sat:
        m = s.model()
        # Return the solution as a list of integers.
        return [m.evaluate(x[i]).as_long() for i in range(n)]
    else:
        return None
```

Performing a binary search over the range of possible flower values with $$k = 4$$, we find that $$\boxed{99}$$ is the maximal achievable flower row length.


<figure>
  <center>
  <img src="/assets/img/blog_images/2025-04-11-hibiscus/shrub2.png" width="100%"  style="display: block; margin: auto;">
  <figcaption> An example of an optimal flower row with 4 colors. It has length 99.
</figcaption> 
</center>
</figure>

For our two cases (four if you include the trivial $$k = 1$$ and $$k = 2$$ cases), it seems that we can find a way to include *all* possible length $$k$$ flower combinations without violating our constraints. This might lead us to suspect that, in general, it is possible to construct a flower row that includes all the possible valid [^1] length $$k$$ flower combinations. If this were true, what would the maximal length of a flower row consisting of $$k$$ colors, $$M(k)$$ be? 

To calculate this, we need only consider how many possible valid length $$k$$ flower combinations there are. We have two cases, 

* **Case 1:** All the flowers are different: In this case there are exactly $$k!$$ such valid combinations
* **Case 2:** There is a repeat flower color:  In this case there is exactly one flower color excluded. There are $$k$$ ways to choose the excluded color and then $$k - 1$$ ways to choose the color to be repeated of the remaining colors. Now to order the flowers, we place the duplicate color first. There are $$\binom{k}{2} - (k - 1)$$ ways to pick the spots they will be placed without them being adjacent and lastly there are $$(k - 2)!$$ ways to place the remaining flowers.

Combining Cases 1 and 2, we find that the maximum flower length that satisfies our constraints on $$k$$ flowers is:

$$ \begin{align*}
M(k) & = k! + k(k-1) \left(\binom{k}{2} - (k - 1)\right) + k -1\\
&   = \boxed{k!\left(2 +\binom{k}{2} - k \right) + k - 1}
\end{align*}$$

So, is it possible to achieve this upper bound for all $$k$$, as our few experiments suggest might be true? Well, it turns out that if we instead model our problem as a graph where the nodes are the valid length $$k$$ flower orderings and two nodes $$u, v$$ are connected via directed edge  $$u \to v$$ if the last $$k-1$$ colors of u are the first $$k-1$$ colors of $$v$$, then our problem is reduced to finding a hamiltonian path on the nodes! Fortunately, our [post last week](https://ccolombe12.github.io/blog/2025/HS_fav_puzzle/) introduced the [Lovász Conjecture](https://en.wikipedia.org/wiki/Lovász_conjecture) which again, suggests [^2] that this graph (finite, connected, and  vertex-transitive) will have admit a hamiltonian path! Therefore we have some reasonably strong evidence that our value of $$M(k)$$, can be achieved!

We can use our z3 model to generate an example of a maximal flower row with $$k = 5$$ colors (we have made the 5th color blue here)! As predicted it was length $$M(5) = 844$$.

<figure>
  <center>
  <img src="/assets/img/blog_images/2025-04-11-hibiscus/shrub3.png" width="100%"  style="display: block; margin: auto;">
  <figcaption> An example of an optimal flower row with 5 colors. It has length 844.
</figcaption> 
</center>
</figure>

[^1]: Meaning that they have no repeated adjacent colors and at least $$k - 1$$ colors appear in them.
[^2]: This is still an open conjecture, so this is not a proof.