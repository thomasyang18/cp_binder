# Combining Across Intervals (in a segtree)

Date: 11-28-2024, 12-2-2024

I feel like I'm still bad at reasoning about this kind of segment tree / algorithm, despite being a "ds main" lmao. Actually, there's quite a bit of DS topics I just brush under the rug and call it a day. Let's try to improve that today by actually understanding some editorials, yeah? 

---

# Bitwise Paradox 

[Link to Problem](https://codeforces.com/contest/1936/problem/D) 

## Approach 

I recall in contest, I was trying some offline stuff; if there's no updates, you can greedily check the condition by finding the minimal r[i] for every starting left position that it works. Then it becomes some 2d rectangle query. Trying to make that work with updates seems impossible lol. 

## Solution 

Having "range OR >= x" implies that you want the range to be big, while minimizing max implies you want the interval to be small, so there seems to be a contradiction. 

However, "range OR" has a very nice property, in that it's easily digit DP'able. So that removes one axis. 

All that remains is to know that you can just encode all of these observations in a segment tree combine. 

In that sense, it's a very "standard" problem, but it's nice since in the implementation, you very much care about the specific values in the range combine that you care about. I think in a segment tree, a lot of the times, I just forget it's ultimately just a decomposition of the original problem; even though I understand exactly how segtree and a lot of its variants work, I treat it more as a separate data structure entirely, so I just forget that you can treat it more directly. 

Like for example, I've augmented segment trees to do crazy things like [range dsu queries](https://codeforces.com/contest/1956/submission/256870559), but that required thinking about, first, a reduction to interval queries, then an exploration of data structures (e.g. I tried treaps first, obviously too slow). 

Whereas in this problem, the segment tree is more of a decomposition in a sense, that required the underlying problem heavily instead of only just a reduction.

# Number of Components (Goodbye 2019)

[Link to Problem](https://codeforces.com/problemset/problem/1270/H)

<details>
<summary>Side note</summary>

[Problem G](https://codeforces.com/contest/1270/problem/G) from this contest is really funny. The MO people probably like it. I'm all for observation problems, this... ain't one of them. Look at atcoder game or tree problems if you want some truly good mindblowing ones. 

</details>

## Approach

Lol, qlog^2n via the offline query tree approach. Because the connected components form intervals, you can maintain it with a lazy segment tree. The observation that propelled this was 

```
edge is added ===> i < j and a[i] < a[j] ===> all elements [i, j] must be connected 
```

So if you just looked for the rightmost element that was higher than you, or the leftmost element that was lower than you, you can reconstruct the entire state with range DSU. (when you add in element a[i] at position i)

(Why this doesn't work naively, normally, is becuase an element can belong to multiple interval endpoints).

## Editorial Solution (nlogn)

Editorial reduces it to counting the # of prefix mins that are larger than the corresponing suffix max's, which makes sense with the above observation. I mean, fair, once you have that observation you just maintain it :/ 

I think there's an interesting thing here though. Logically, there's not a difference between "range DSU queries" and "the connected components form an interval"; if your merges are of the form of range DSU merges, then yes, the logical conclusion is that the connected components must form an interval. But explicitly saying, "the connected components form an interval" implies that maybe you want to stop thinking about them as connected components themselves, and question them as intervals ; when do the breaks appear, is there some structure to it, is there some easier representation? 

Sometimes the abstraction helps, sometimes it hurts. 

There were two problems that had a similar flavor: [1](https://codeforces.com/contest/1710/problem/D) [2](https://codeforces.com/problemset/problem/1956/F). 

<details>
<summary>Disclaimer</summary>


All the following solutions are qlog^2n.

Actually, [I had one of my problems 1 shot recently in a similar situation](https://www.acmicpc.net/source/86753613). Very cool HLD idea in my opinion, but because you're doing max queries you can just 1 shot with the observation in [this problem](https://codeforces.com/contest/1930/problem/H).

I dunno, I thought that idea was very, very neat, the idea of "using HLD indirectly" is just cool (as in [this problem](https://codeforces.com/blog/entry/71534?#comment-559319)).  

[I have no idea what Jereonodb means by this](https://discord.com/channels/555883512952258563/555883680967426048/1208475545197609081), the only way I can think of to answer this query, is, well my HLD solution. Maybe I do need to relearn HLD lol if you can just do it "directly" by querying on the path. I mean, sure there's a log^3 solution with binary search, but :skull:  

It seems like data structure approaches, even if creative, can get just 1 shot with being smarter :D 


</details>


## FizzyDavid's solution

[Link to solution](https://codeforces.com/blog/entry/72611?#comment-569105)

### First Impressions

This is the crazy solution, this is the main reason why I wanted to look at this blog. 

First of all, it's 764ms, like wtf? The solution runs in O(n + qlog^2n). 

Second of all, it looks like he's recursing into both sides of the segment tree, which is like, no wonder why he thought that his solution could be O(qn) worst case. This is actually some black magic. 

### Trying to break it down step by step

Store in each node 

```
mn, mx = min, max value in segtree range
a value ans
l_ask_r = cached version of tr->query(tl->mn, -1)
r_ask_l = cached version of tl->query(1e7, tr->mx)
```

Immidiately in the query function we see this line

```cpp
int ret = (min(x, tl->mn)>max(y, tr->mx));
```

so FizzyDavid is again trying to count # of prefix mins better than maxes. 

If we just ignore l_ask_r and r_ask_l and replace them with their true meanings, we get 

```cpp
int get_ans()
{
    return query(INF, -INF)+1;
}
int query(int x, int y)
{
    if (is_leaf) return 0;
    if (x <= y) return 0;
    if (x <= mn || mx <= y) return 0;
    
    int ret = min(x, tl->mn) > max(y, tr->mx);

    if (x >= tl->mx && max(y, tr->mx) == tr->mx) ret += tl->query(INF, tr->mx);
    else ret += tl->query(x, max(y, tr->mx));

    if (y<=tr->mn&&min(x, tl->mn)==tl->mn) ret += tr->query(tl->mn, -INF);
    else ret += tr->query(min(x, tl->mn), y);

    return ret;
}
```

We see that the if statements are just more caching behavior, so we finally get the original, intended query

```cpp
int query(int x, int y)
{
    if (is_leaf) return 0;
    if (x <= y) return 0;
    if (x <= mn || mx <= y) return 0;
    
    int ret = min(x, tl->mn) > max(y, tr->mx);

    ret += tl->query(x, max(y, tr->mx));
    ret += tr->query(min(x, tl->mn), y);

    return ret;
}
```

I mean, yeah. This is indeed, how to count the number of prefix mins greater than the number of suffix mins. You literally just... count it. Lol. 

Now let's go back and see how the caching works.

```cpp
if (x >= tl->mx && max(y, tr->mx) == tr->mx) ret += tl->query(INF, tr->mx);
else ret += tl->query(x, max(y, tr->mx));
if (y <= tr->mn && min(x, tl->mn) == tl->mn) ret += tr->query(tl->mn, -INF);
else ret += tr->query(min(x, tl->mn), y);
```

So we make the optimization that, if the result of the query at this node is already known because the prefix min / suffix max is "good enough", we return a cached version of that query.

What's surprising is just how robust this optimization makes the query function, and why the hell it's only log^2n.

Okay, so first: Why do we have to even re-dfs down the segtree? 

Because, for example, if we computed F(x) on the interval [0, n/2], we would not take into account the suffix max from [n/2 + 1, n]. If the suffix max changes, everything in the interval [0, n/2] has to be recomputed. 

So we make an important simplifying assumption: At each node in the segtree, we simply compute, "what is the answer, assuming the answer *didn't* change due to an external prefix/suffix max", which is what all those checks are for. 

Another helpful mentality to keep in mind, is that if x == INF, every single cache optimization check will pass for that variable; likewise for y == -INF. 

So actually, you just replace get_ans with

```cpp
int get_ans() {
    // return query(INF, -INF)+1;
    return l_ask_r + r_ask_l + (tl->mn>tr->mx) + 1; // account for n == 1 separately
}
```

So the only work being done is precisely on the recomputation. We recompute on log(n) levels; assume that all sublevel queries are cached. Our goal is to now prove that, based on access patterns, a single call to compute() on a node accesses O(log(n)) nodes.

Well, let's just look at a call to l_ask_r. 

```
(x = prefix min)... [ tr, query node ] ... y = -INF
```

y = -INF means all checks are ignored, so

```cpp
if (x >= tl -> mx) ret += r_ask_l; /* stops */ 
else ret += tl->query(x, max(y, tr->mx)); /* The only sus case */

if (x >= tl -> mn /*min(x, tl->mn)==tl->mn*/) ret += l_ask_r; /* stops */ 
else ret += tr->query(min(x, tl->mn), y);   /* x is effectively -INF from the perspective of tr */
```

is the conclusion you can come to if you stare at the inequalities hard enough (I imagine WLOG it applies to the r_ask_l case too).

In the sus case, when (tl -> mn <= x < tl->mx) and y == -1, we recurse into the left tree with;

```
(x = prefix min)... [  tl, queried  ] (y = suffix max)    
```

However, notice the inequality we just wrote out; (tl -> mn <= x < tl->mx). That's a suspicious inequality. Our range now looks something like 

```
        |  mx        | 
    x   |    [tl]    |
        |     mn     |
```

(the order of mx and mn doesn't really matter). 

We know there can't be prefix min > suffix max until after pos[mx]. I imagine the checks in the segtree enforce that. And then once we get to pos[mx], it might behave like INF again? 

I've spent way too much time trying to understand the intuition, and I think it just has to do with: 
- when there is a candidate answer in our interval <==> one of x or y behaves like INF or -INF, so there's only log(n) queries as we proved with casework (there is only at most one splitting call per layer, all others get cached?) 
- when there is no candidate answer in our interval <===> both x and y are NOT INF/-INF, but because there are no candidates, the checks just work out in log(n) regardless until you push to the next INF/-INF state. 

Not a full proof, just handwaving, but I think I finally got the intuition down for why this might be fast. 

Though I can definitely see why FizzyDavid was suspicious; I mean the proof's already hard enough (I don't have a full proof), but for example, when I argued that "x is effectively -INF from the perspective of tr", yes, that's true, but the code doesn't re-use the cached query? But it still runs fast? That's kind of insane.

And this is supported in his [changed code](https://codeforces.com/contest/1270/submission/67931310); he's just adding a lot more checks to say, "hey, let's re-use based on these inequalities we've coerced into".  

This deserves its own, in-depth writeup. It's really genius. But I've spent like a week trying to write this blog; I want to publish something :sob: 

Perhaps there's a nice visual proof for why this works. 

### Takeaway

This is a really fucking cool solution, but it's not really a true "crossing information across intervals" solution, as I thought the other solutions (such as mango_lassi's) would be. 



## mangolassi's solution 

[Link to comment](https://codeforces.com/blog/entry/72611?#comment-569526)

Okay, so that's just taking the main observation, augmenting it a bit (you're basically unioning two "candidate intervals" here) then maintaining it online with the interval connected components formulation. This is basically the online version of what my solution does, though obviously my solution has a garbage, garbage constant factor. 

Cool beans. 

## yosupo's solution

I took like 5 seconds at [this solution](https://codeforces.com/contest/1270/submission/67924579) and it just seems like he's maintaing editorial solution (# of prefix min > suffix max) on sqrt lol. 

## Moral of the story

Oh here, definitely need to take away to reformulate problems cleaner to shave a log factor. So many times I've seen there be a scuffed (or even nice) log^2 solution when a clean, log solution that has a totally orthogonal observation exists. 

# XXI Open Cup, Grand Prix of Suwon L

[Problem Link](https://codeforces.com/gym/102979/problem/L)

[This is very hard](https://codeforces.com/blog/entry/87916?#comment-764340)

<span style="color:red"> I think this just requires a separate blog post. It's definitely of a different flavor, in that this approach is more general and an alternative solution to basically "k best solutions" problems. I think it's pretty interesting, but its different. </span>

# Takeaways

Wow, I got baited hard by some of these, but I also did find some true gems too (like FizzyDavid's solution, even if it didn't really truly turn into "cross over intervals"). There is some good inspiration here for some cracked problems, I think. I'm glad I finally sat down and went through some of these things. 

# Discarded fake problems

<details>

<summary>
Wine Factory 1919F(1, 2)
</summary>

## Easy Version

[Link to Problem](https://codeforces.com/problemset/problem/1919/F1)

The O(n) algorithm is given in the statement, turning it to DS problem is tricky? 

Looking back on it now, I think what I didn't do the first time I came across this problem (didn't get it in contest) was that I didn't treat wizards converting water to wine, also as an expendable resource that needs to be matched to the water itself. 

Once you frame the interpretation like that, the approach & correcntess of the algorithm becomes obvious. 

## Hard Version 

### First thoughts (Approach)

So, I've heard whispers about this problem, that it's modeling a DS problem in a flow graph. I'll give myself some time to think about this problem given this hint. 

--- 

Thought for 20 minutes, could only come up with this merge algorithm which I have no idea if its correct or not 

```

sub = min(c, left.a, right.b) 

newcap = c - sub 
left.a -= sub 
right.b -= sub 

a = right.a + min(newcap, left.a) 
b = left.b + min(newcap, right.b) 
```

I'm pretty sure it's wrong, as otherwise it would have way more solves in contest. This is the most natural extension of the previous algorithm. 

I tried formulating it as min - cut, yes I have the graph, no I don't have any real observations about it. 

### Solution 

Well, [Zhtluo](https://codeforces.com/contest/1919/submission/240574390) implemented what I had in mind actually correctly, it appears like my model is slightly flawed, but yeah the same idea of "you want to match all the water and wizards, therefore at the end you can just greedily assume water/wizards go to one end", a natural extension from F1. I'm just bad at implementing ig. This approach is definitely tricky.

With the min-cut formulation (editorial formulation), the transitions become much more lucid and obvious, which I think is a good thing that the editorial went with that formulation. Although it's definitely less intuitive at first; for example, the obvious observation that you take min(A[i], B[i]), becomes slightly altered, in that you either take A[i] or B[i] and are trying to cut edges if the subsequence "AB" exists uninterrupted in your mincut. 

### Afternote

Nice solution, I definitely liked the min-cut formulation more since although it's hard to come up with (at least harder than the "direct" implementation), the resulting intution, at least to me doesn't feel like a bunch of annoying casework.

This ended up not really being a combining across intervals problem, I don't know why I thought it would be. 

</details>

<details>
<summary>
ICPC WF 2024 J 
</summary>

# Discussion

I'll admit, in contest I didn't even think about segment tree. Considering that "point update range query" is literally so segtree-able, like, duh. 

(To be fair, we were having a stroke on like 6 easier problems though)

After hearing the initial solution, I was confused at first, but now I think I understand it. In the DP, you basically go left to right, and you make some assumptions about when a robot can come to you. And if you do run into a robot, you can "cash in" on those assumptions. Classic state machine transition stuff. 

Initially, I was thinking this was another classic "combine over intervals" problem (which is why this is now a separate problem) but actually it's an "optimize DP with segment tree" problem. 

The idea is cute I guess. I think the more important thing how I got baited by "combine over interval" approaches in contest and even post-contest by assuming the editorial solution. (but somehow didn't think segtree? I guess because there's no segtree combine over interval solution? Though there definitely is, given the robot independence observation...)

# Moral of the story

Uhh, I don't know, there really is not one here. 

</details>