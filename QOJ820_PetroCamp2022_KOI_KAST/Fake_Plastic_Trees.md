# Fake Plastic Trees

Date: 11-27-2024

[Problem Link](https://qoj.ac/contest/820/problem/2562)

---

Well, [Swistakk endorsed this problem](https://codeforces.com/blog/entry/109968#comment-979681), so it's gotta be good. Also, pretty much every problem I've read from this contest I really liked and thought was great, even streelights though I didn't really understand it at all lol. There are 3 that I haven't read but I'd imagine one of them is bad, the rest is good (I'd be really surprised if this contest had only good problems)

## Approach

I mean, you should be able to immidiately come up with the tree DP approach that's pseudo-polynomial (basically exponential in terms of input bits). 

N = 1000, K = god damn 50. So those numbers don't matter, I just want an approach that can get rid of the * maxA factor. 

Now, I had some observations:

- If L = 0, clearly the optimal tree is always going to be the minimal possible weight. 
- If R = INF, clearly the optimal tree is going to be the maximal possible weight. 

To try and make use of this, we consider two types of trees, "lo" and "mid" trees. 

- lo < L
- L <= mid <= R

Lo trees must combine, mid trees may or may not combine. 

Notice that once a tree goes into "mid" range, it's optimal to always take the smallest tree possible.

I was hoping that there was some structure to the tree DP that would make what is effectively subset sum nice involving these observations, but sadly that's not the case here...

## Solution

Editorial solution is just absurd. 

So what Lemma 2 is saying is, for a given rooted subtree T[v], you know that k subtrees have already been filled. This means that the range of subtree sum values is between [sum[v] - kR, sum[v] - kL], which is how you get that bound. 

And it happens that your sufficiency check is also effectively an interval query of [L, R], so the most intuitive thing is to just delete the middle of close enough triplets. 

Though it is pretty difficult to prove that that approach even works, since it gets weird once you have to include sums and stuff. I think if you just make the bucket sizes (R-L) /2 or something it might just be even easier to prove? 

I think if I were smarter or thought about this more deeply I'd be able to generalize this a bit more. Yes I understand the steps (well, besides the last part's proof that I skipped lol), but I don't really know what to categorize this trick as or think about it as. 

Also, why the hell weren't my above observations good??? I don't know how to make that work :( 

    