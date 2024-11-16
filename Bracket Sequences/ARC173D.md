# ARC173 D - Bracket Walk

Date: 11-15-2024

[Link to problem](https://atcoder.jp/contests/arc173/tasks/arc173_d)

---

# Problem Statement

You're given a directed graph, where each graph has a label: "(" or ")".

Determine if there's a walk on the graph that starts and ends at the same vertex, that uses every edge at least once, and forms a RBS by concacenating the edges in order.

(no multiedges or self loops, graph is guaranteed strongly connected). N=4000, M=8000.

# Observations 

<details>
<summary>Author Note</summary>
I've solved this problem before, as usual; for all of these, I want these to be a semi-formal writeup instead of a jumble of thoughts, therefore I've already solved the problem. But I forgot the solution, so I'll just think through it lightly here.
</details>

Just call it +1 and -1, as usual when dealing w/ bracket sequences.

Immidiately I recall my first thought was, "So if you can get a cycle that arbitrarily increases your total count of +1, that's a really, really good sign." 

The resulting answer is going to be some multiset of cycles that span all edges.

Bracket sequences => just get the count to be zero, a weaker condition, therefore in this simpler problem we just need that we either have at least 1 positive and negative cycle, or no positive and negative cycles. If this is the case, we have a solution.

After this, I was stuck for a fair while, and just guessed that this is an IFF. Turns out, yes, it is.

# Reasoning

Gigantic brain reasoning: A bracket sequence with 0 count can always be shifted to be made correct; there is always at least 1 circular shift that works. So in fact, solving the simplified problem corresponds precisely, as operating on counts will always get us a 0 count string.

The reasoning is intermediate value theorem-esque. 

# Hangup

If you **don't** think like this, let's see... you start trying to argue about when & where you leave a positive cycle and get onto a negative one. But this runs into issues where you're worried that you entire a cycle at a "bad time", causing you to make a prefix of the bracket sequence fail. Or perhaps even if you can manage to visit every cycle by spamming the positive cycle over and over, there's no way to re-complete the cycle properly. 

It's also an issue in the all 0 cycle case as well. 

So yeah, the gigantic brain reasoning just saves all of this headache and nonsense. Wow. 

---