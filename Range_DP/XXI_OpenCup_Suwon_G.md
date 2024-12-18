# XXI Open Cup Suwon G 

Date: 11-29-2024

[Link to Problem](https://codeforces.com/gym/102979/problem/G)

Up to date, this is probably the most insane range DP I've seen.

I'll make a blog post later describing the buildup, but basically my understanding of range DP's has been:

1. what is DP?
2. oh, yeah, makes sense you can do dp[l][r] (after learning some trivial dp's)
3. yeah, so the types of subproblems you can do are pretty varied, you can break off some subsegment, some set of subsegments, etc. as long as all dependent subproblems are subsegments themselves
4. wtf???? [CERC 2014L](https://cerc.tcs.uj.edu.pl/2014/data/l.pdf) you can think about the problem as, "I delete a point from my interval, delete all points that intersect with that point, and when I recurse, ignore all segments that aren't strictly inside of my new interval????" This blew my mind when I first saw it. It totally makes sense from that perspective, but wow. 

And now you get **this damn problem**. 

## Approach

So obviously, given the above knowledge, the most "natural" DP you can think of is 

```
dp[i][l][r] = in the range (l, r), either pick or don't pick the ith (or largest element <= ith element) largest element
```

and you can find the best thing in your interval in... uh... actually I don't really know, but point is this is o(n^2 * k) so it's doomed regardless.

The first thing is to think if you made your DP sparse, maybe you could avoid visiting all states? You can quickly convince yourself no, you probably have to visit everything w/ this approach. 

I was trying some genuinely crazy things. I recall trying to sweep in ascending order and do some kind of push dp; if I pushed index i for some values A and C, I would push the cartesian product union of all segments [l, i] and [i, r], but obviously this is still o(n^2 * k) in the worst case. I tried to argue maybe you only needed to do n updates, didn't seem true. Maybe push the optimal with some magic iterative algorithm? No luck. 

--- 

Then, I realized that the a[i] * q(interval) - c[i] condition was basically a set of lines (q(interval) is the "x" that is shared across all equations for a fixed interval). Surely you can't just iterate over all possible transition points and assume you take the maximum line on the convex hull at that query point .... right???

## Yes you can, actually 

As [Swistakk](https://codeforces.com/blog/entry/87916?#comment-764627) mentions, with this method you can just eliminate the need to "sweep" with your DP state. 

---

Clearly, this can never undershoot the answer. As compared to the naive approach, the worry is that we're going to select some medium A[i] for the initial cost, then select some huge A[i] on a subinterval, which would completely kill the "pick max, delete interval, recurse" framework. 

---

This can never overshoot the answer either; since a[i] and q(interval) are all non-negative, and a q(subinterval) <= q(superinterval), if you were going to pick two numbers a[i] and a[j], where a[i] < a[j], if you recursed on a[i] first you'd be hamstringing your costs every time. 

Well, that's not exactly clear. I think of it as, if we played the classic cartesian tree interval "game" here, but instead of forcing to pick max we were allowed not to pick max, but still had to delete and subrecurse, it's always optimal to do the greedy cartesian tree ordering. That seems slightly more intuitive, still annoying to prove? 

---

As to why CHT just works here... idfk 



## Moral of the story. 

Like, when I first saw CERC 2014L, I was amazed at first, but over time as I started to really internalize that pattern more and more (especially helped after learning divide and conquer), it's like, "Oh, sure. You're imposing some sequence of deletions, so of course this works." 

It's not intuitive **at all** why you don't even have to care about the order of deletions here. Here, the bait is even **more** clear; one of the **first** tricks you think of when first coming across cartesian tree-esque problems is "casework across the maximum element or not". 

Overall? Great problem that fucks with your intuition. 