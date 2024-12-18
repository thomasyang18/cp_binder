# Thinly Disguised Problems

Date: 11-19-2024

---

Of course, from a large enough distance, every competitive programming problem is some contrived application of some techniques, some standard, some not, some standard within the CP community, some not. 

Let's stop beating around the bush and just rattle off the problems.

----

## Gennady Korotkevich Contest 7

[Link to Problem](https://qoj.ac/contest/1223/problem/6409)

Basically, make sparse lazy segment tree operations in O(q) memory. Normally you'd need O(q \log N) memory, where N is the true size of the range you're trying to encode (here N = 2e9). 

The funny is that you can encode your sparse segment tree as just a normal segment tree - a binary tree where each node is an interval, and each child is a disjoint union of the parent interval. 

What happens if the queries become unbalanced? Just rebalance the tree lol. 

<details>
<spoiler>Although...</spoiler>

I believe there is a complicated O(q) solution for sparse segment tree as well where you compress each node for an interval as much as possible, sort of like a compressed trie would do to get O(n) nodes (where n is the number of input strings) instead of O(sum of string lengths) nodes. 

In some sense, this is more intuitive if you're stuck in the framework of "every node corresponds to exactly this interval" of a segment tree. But it's definitely harder. 

</details>

Basically, can you think 


# Takeaway

There's definitely an argument. 


-----------

https://codeforces.com/problemset/problem/1920/F2


https://codeforces.com/contest/1270/problem/G