# Augmenting a DSU

Date: 11-14-2024

There's quite a lot of random hacky things you can do with the dsu structure, especially abusing the fact that with small-to-large you get log(n) height guarantees.

---

### Example Problem

You're given a weighted undirected connected graph. Each node has an associated A[i] and C[i] value. Each edge has a weight W[e]. You have to answer multiple queries of the form (S, T, R). For every query, do this following process:

You start at node S, and want to reach node T. You have an initial rating of R. You can travel through an edge iff W[e] <= R.

At each node, you can perform the following two actions:

    - Gain A[i] rating for free
    - Gain 1 rating for C[i] coins 

Find the minimum number of coins required to reach T from S.

### Solution

Classic dsu reasoning; notice that as we process edge weights in increasing order (motivated because our rating is increasing), we expand our components to get more options. The greedy optimal strategy for any query looks something like, eat as many uneaten A[i] as possible, increase your rating by the minimum C[i] in your connected component if applicable, and keep expanding outwards until you hit T.

To implement this is tricky. I'm sure there's some direct binary lifting solution in O(log(n)), but instead we try this.

First use KRT to simplify the structure; now the algorithm is just:

- R = R[q] + (sum of A[i] in subtree), coins = 0
- every time we go up an edge, coins += max(0, W[e] - R) * (min_C[i] in subtree), R = max(R, W[e]), R += (A[i] in other subtree not used)

until we hit the LCA of S and T in the tree.

This seems really binary lifting optimizable now, but I had no clue how to do it. Instead, I simulated this with small to large. We can batch queries together, akin to a sort of sweepline. What's not obvious at all is how step 2 is remotely fast, since you have to apply this to every query in the batch.

The trick is to have another DSU for the queries; notice that if we define queries by the two important variables (current_rating, coins_used), what actually happens is (if we sort by rating)

```
R R R R [cutoff] R R R R
```

where everything before the cutoff is pushed to the cutoff, and everything above the cutoff is ignored. So we can actually afford to manually update everything before the cutoff because it will be amortized, if we group queries of similar rating together if they meet.  

But it's tricky because each query also has an associated coins cost that we have to retrieve. Recall this line

```
coins += max(0, W[e] - R) * (min_C[i] in subtree)
```

Now, coins does not refer to a single query, but a batch of queries. This is where small-to-large comes into play. Similar to [CF 1290C](https://codeforces.com/contest/1290/problem/C), to simulate lazy propagation on a DSU, instead, have the tags trace themselves up the tree instead, as the dsu structure will have high degree but low depth.

But there is yet again another complication. Because we impose this small-to-large structure, there's this hierarchy that we don't necessarily want.

Let's say we have two queries at once get batched at a cutoff and we deem to merge them, each with their own lazy tag structures, A, E. E is a set of 100 queries, A is just one.

By small-to-large logic, we should make A a child of E. However, E does not want to give its lazy tags to A; if we do an uptracing algo, we will inherit E's tags when we don't want to.

To fix this, we could just give A a lazy tag of -lazy[E]. However, if we have multiple queries, you could work out the complications on paper for the naive appraoch.

Instead, when we have a batch of queries that we want, we need to first merge them like normal. Then, every single previous root, independently, will negate itself with its new parent's previous lazy tag, and you can work out the math to see that this works.