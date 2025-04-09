---
layout: post
title: "Can You Solve a High Schooler’s Favorite Puzzle?"
description: My solution to this week's Fiddler on the Proof Puzzle
giscus_comments: true
date: 2025-04-08 00:00:00
published: true
---

This week's [The Fiddler on the Proof](https://thefiddler.substack.com) puzzle is paraphrased as follows:

> A teacher is handing out candy to his $$N$$ students, of which $$N \geq 4 $$. He abides by the following rules:

>He hands out candy to groups of three students (i.e., “trios”) at a time. Each member of the trio gets one piece of candy. Each unique trio can ask for candy, but that same trio can’t come back for seconds. If students in the trio want more candy, they must return as part of a different trio.
When a trio gets candy, the next trio can’t contain any students from that previous trio.

>It turns out that *every* possible trio can get a helping of candy. **What is the smallest class size $$N$$ for which this is possible?**

<br><br>
## Solution

Instead of focusing on the case of trios, we will generalize the problem and consider groups of $$k$$ students. If there is some ordering of groups such that every group of $$k$$ students can receive candy, what is the smallest such $$N$$ for which this is possible? Denote this smallest $$N$$ as $$N(k)$$.

Now, what are some initial observations we can make about the problem? Well, first off, we can actually place the lower bound that $$N(k) \geq 2 k + 1$$. This is because if there are any fewer students, then we cannot find a sequence of 3 initial $$k$$-groups. This was the only bound I could find in my initial exploration—the problem turned out to be much harder than I expected! 

Perhaps changing our perspective can help us gain traction on the problem. What if we instead thought of the students as **nodes** in a graph and two nodes share an **edge** only if their representative groups are disjoint. Now, a [path](https://en.wikipedia.org/wiki/Path_(graph_theory)) in this graph would then represent a sequence of groups that could get candy such that every group in the sequence is disjoint from the groups adjacent to it in the sequence. If we could find a path such that every node appeared in it exactly once, then we would have found a sequence of groups that allows every group to get candy! Such a path in a graph where every node is visited exactly once is called a [Hamiltonian path](https://en.wikipedia.org/wiki/Hamiltonian_path). Our problem has now become, for what $$N$$ does a Hamiltonian path exist for a given $$k$$? 

It turns out that determining whether a Hamiltonian path exists in a graph *is a [hard problem](https://en.wikipedia.org/wiki/NP-completeness)*. But perhaps there are some special properties our graph that make it a tractable endeavor without having to perform a brute-force search.

We need to first better describe our graph. Suppose for now, we fix some $$N \geq 2 k + 1$$ and construct our graph as follows:
The set of nodes $$V$$ is the set of subsets of size $$k$$ from the group of $$N$$ students. Nodes $$u, v \in V$$ share an undirected edge if $$u \cap v = \emptyset$$. This graph will have $$\binom{N}{k}$$ nodes. For a given node $$u$$, it will have $$\binom{N - k}{k}$$ edges (one to each of the groups that it shares no members with) and thus by the handshake lemma, the graph has $$\frac{\binom{N}{k}\binom{N - k}{k}}{2}$$ edges. 

Now that we have a clearer picture of the graph, is there a particular aspect of it that allows us to determine if it has a Hamiltonian path? It turns out there is. One especially nice property of our graph is that it is *vertex transitive*. In simple terms, vertex transitive implies that from every node in the graph, the graph "looks the same" from the perspective of that node. It turns out that there is an open conjecture, the [Lovász Conjecture](https://en.wikipedia.org/wiki/Lovász_conjecture) that states: 
>*Lovász conjecture: Every finite, connected, vertex regular graph has a Hamiltonian cycle (and thus a Hamiltonian path).*

While this is an open conjecture, it implies that there is some evidence to suggest: as long as our choice of $$N$$ allows our graph to be connected, since it is vertex transitive, it may indeed contain a Hamiltonian path. This leads us to the natural question, for what $$N$$ is our graph connected?

It turns out that for $$N \geq 2k + 1$$, our graph is connected! To see why, consider any two distinct nodes $$u, v \in V$$. If $$u \cap v = \emptyset$$, then they share an edge by definition. Otherwise, Suppose $$u$$ represents the group of students $$(A, S)$$ and $$v$$ represents the students $$(B, S)$$ where $$A \cap B = \emptyset$$ and $$ \|S\| = x $$. Under this framing, we can show that that the set of students $$W$$ that are not in  $$u \cup v$$ has size $$x + 1$$. We can now construct a path from $$u \to v$$ using the following intuition.

We make a path that has the form 

$$ (A, S) \to (B, W') \to (A, S') \to (B, W'') \to (A, S'')\to   \dots \to (B, S)$$

where we use the extra student in $$W$$ to iteratively swap in one more student of $$S$$ into the group with $$B$$ and vice versa until the subgroup paired with the $$B$$ subgroup eventually becomes the desired $$S$$ subgroup [^1].

Now that we have argued that $$N=2k + 1$$ is sufficient for our graph to be connected, and we have "evidence" now that suggests this should also be sufficient for there to exist a hamiltonian path, what else can we do except also conjecture that $$N(k) = 2k + 1$$ and call it a day? Well it turns out that our graph with $$N = 2k + 1$$ nodes is actually a special and well-studied graph. It turns out for $$N = 2 k + 1$$, we have actually described the *[Odd Graph](https://en.wikipedia.org/wiki/Odd_graph)*, $$O_{k + 1}$$ which, for $$ k \geq 3$$,  **is known to contain a Hamiltonian cycle (and thus a path)!**[^2]  Therefore, our choice of $$N = 2k + 1$$ will ensure that every grouping of children can get candy and since this is the lower bound on possible values of $$N$$, we indeed have that 

$$\boxed{N(k) = 2k + 1}.$$


[^1]: To really see how this works, try to do this with the case of $$k= 3$$ and $$N = 7$$.
[^2]: A bit anticlimactic that we’re able that we are able to simply point to the literature and call it a day but, but that’s research sometimes! This was a great problem to learn more graph theory!



