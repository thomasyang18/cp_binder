# B

Date: 12-15-2024

---

Okay, I already looked at the editorial before, but obviously forgot the details; re-derived it though. 

I remember when originally thinking about this problem, I already reduced it to:

- Red blue components are ALWAYS good to connect
- So you have these "clouds" of reds and blues.
- This forms a **bipartite graph.**

and for some reason I thought that was super important. That's not, really important; what's important now is to notice (this is what I missed the first time)

- some nodes are **good** because they are satisfied
- in order to propogate "goodness", you **need** to have 1 good, 1 not good 

This immidiately defines a BFS that works. 

Why is this true? Well, the remaining edges can only satisfy one node, and a spanning tree has n-1 edges; so at least 1 of the starting nodes must be good. 

## Bug 

The bug I had was that I forgot that nodes edges can be satisfied and "good", but we might still be missing edges just because we said it wasn't "optimal" to connect them. Sure, it's not optimal to connect them for the greedy, but in the end you gotta connect them (thankfully input graph is guaranteed to be connected)

## The moral

So here, it was really important to think of "satisfying **nodes**". I mean, I guess in hindsight you can argue that arose "naturally" from the problem statement, but no, it truly is kind of weird for this problem, especially since you have to make the (n-1 edges, but only n nodes in a spanning tree) observation. 

