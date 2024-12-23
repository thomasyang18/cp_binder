# Pair Guessing AGC069B

Date: 11-27-2024

[Link to Problem](https://atcoder.jp/contests/agc069/tasks/agc069_b)

---

Wow. Just wow. I had a slightly wrong idea at every step, it's so easy on this problem to think you know the answer but you don't. 

# Pitfalls

## N-1 operations

For a good 30 minutes or so, I was convinced you needed more operations, because I did not know how to handle (i, j) cleanly. Fortunately, I came up with the "off main cross" solution eventually, which is the key to pigeonholing the choices correctly. 

By querying pairs {r, c} off the cross over and over, at the end, you get to a point where there's truly only two candidates (with 1 query left), so you can pigeonhole correctly. 

But then I just hastily generalized...

## Graph model failure

In general for 2D grid problems, I think I should try the row-column approach, I don't think about it enough. Fortunately for this problem, I interpreted as some kind of bipartite matching problem. Unfortuanately for this problem, thinking about it as matching is horribly wrong. 

The actual condition for removal is "you can remove (r, c) if any 0 is on the cross". I'm not even sure if this is possible to model properly with bipartite matching; I certainly had a lot of trouble actually implementing this (and for some reason implementing blatantly wrong approaches, such as the classic "connect up iff s[i][j] == 0")

# Post reading editorial

## N-2 operations

Yes, you only need N-2 operations in the general case. 

Take this configuration

```
11
11
```

In the N=2 case, its impossible, since you must query **within** the bounds, which admits the possibility of (i, j), a recurring problem child. 

However, when N > 2, then when you recruse down to the n=2 case, you can query out of bounds, and generate a boolean truth table.

```
 Q
Q11
 11
```

And the strategy doesn't really work for N=3; editorial gives an insane proof but the simple answer IMO is just "you need 2 * N - 2 queries". 

## Looking at the graph just... normally

So just look at the graph just normally. Yes it's bipartite, no you don't really care that much about its bipartiteness (at least explicitly), which is such a "duh" moment lol. 

Playing around with some examples, you realize that the operation of picking a cross corresponds to 

"Pick a row and column node such that their sum of degrees is at least 1, and delete both nodes and all edges connecting them." 

Very quickly you can pick up on the idea that a tree respects this removal process well. 

## One last failure 

(x and y mean row nodes and column nodes)

I started thinking in terms of connected component sizes, w.r.t. how many x's and y's each had. I just assumed that in the end, you can basically match those up with operations correctly in the end, even if order of removal does matter (pretty sure this is true given the editorial solution). 

Came up with some insane algorithm involving counting the number of isolated x and y components, then for the components that have edges in them, did some greedy matching thing to pair down. 

Honestly still not sure why this would fail, but I'm also sus about the proof. TL;DR this is a complete guess algoriithm, boohoo.

## A cleaner way 

Acyclic subgraph with n-2 edges. 

Uh, yeah, duh. We've already established that you need to delete at least 1 edge per query, and you're deleting an entire node as well, so the resulting component must be deleted in a "tree like" fashion. (n-1 deletions, n-1 edges with 1 extra node remaining)

It's a little less clear that there will always be "leftover nodes" for you to eat, but idk that's basically the same guess as my failed solution. 


<details>
<summary>
on second thought
</summary>


At first I thought this had to deal with the bipartite row-column graph condition, and true that's decent intuition, but pretty sure this is just true of any graph with 2N nodes and an acyclic subgraph of N-2 edges. Basically within each component, you get a free node, and otherwise you have a bunch of isolated nodes; balancing them out should give you enough free nodes to work with. 

"important" nodes: 2 * (N - 2) - (# CC's)

If CC's is low, then there's a lot of free nodes 

If CC's is high, then "important nodes" is small, e.g. free nodes is high again (since again you get 1 free node per CC)

cba to work out the math precisely but pretty sure this just works for any graph w/ these numbers
</details>

## Moral of the story

Not really sure if there's a moral here. Perhaps think of more approaches and much more careful in thinking. A lot of subtle edge cases or slightly wrong algorithms. 

I think it might be my process iteration? To get intuition for a problem space, I'll think about different approaches, but I don't really have a good handle of when to stop DFSing, or to rein in the handles, or to take a step back, recouperate my observations and continue. 

For pretty much every "hard problem" I solved, I think that pretty much the first few things I tried just worked. And I've developed a habit of trying not to read editorial (or even if I do, I'll just mostly skim it like I did here until I get stuck again) under the reasoning that I want to come up with the ideas myself, since ideas in CP generally shouldn't be insane (again, geared towards short form contests, and high schoolers). 

I feel like in this way my thinking has really stagnated over the years. Or maybe people can just iterate through ideas faster because they're smarter, idk. Now that I'm out of college, I probably want competitive programming to take a back seat, like a more relaxed approach. 