# ARC146E - Simple Speed

Date: 11-18-2024

[Link To Problem](https://atcoder.jp/contests/arc146/tasks/arc146_e)

## Problem Statement.

Given array of integers (1 <= A[i] <= 2e5, N <= 2e5), compute the number of integer sequences B s.t. 

- There are exactly A[i] occurences of i in the sequence B. 
- |B[i] - B[i + 1]| = 1

## Solution

So, I didn't solve this legit. I wish I still had the notes for this problem; I recall trying a lot of various DP setups (obviously) involving scanning left to right, as one does, but they would keep overcounting or failing one of the conditions. 

The shift in perspective is: **Build the sequence, value up.**

Once you have this, then it becomes extremely clear what happens. The only two variables you need to keep track of is the current **i** value you're placing, and how many "free spaces" of **i** values you have (with some edge cases for the edges.)

As it turns out, the number of gaps you can have is pretty much fixed as well. Because once you have, for example 

```
11111
```

placed, you **must** fill in between each one, some amount of twos. 

Obviously you can place the twos at the ends, so that adds a tiny bit more casework, so the # of free gaps might not be perfect, but overall there's only O(n) states to really consider. 

## Takeaway

So... this is an objectively hard observation. It showed up as a [2600 problem on codeforces](https://codeforces.com/contest/1919/problem/E), in which the constraints were lowered artificially, and it appears the coordinators either [didn't know about this solution or thought it was too hard](https://codeforces.com/blog/entry/124220). Furthermore, on kenkooo, this problem is rated 3131. 

The second observation comes naturally, once you start thinking in the above framework; I think if you give any master the crucial observation, they can figure out the DP themselves. To me at least, the most intuitive form of DP is precisely when you can formulate the problem as a decision problem / game, where you try to minimally encode the state of the game, so as long as you have that framework, the DP becomes obvious. 

The first observation is the true meat. It's such a simple and elegant idea in hindsight. I can rapid-fire shoot out several theories as to what happened:

- Naturally you build a sequence left to right on paper, and type as well. It's just a very human instinct. 
- Sequences we naturally think about in terms of segments, or nice patterns. For example, recursive sequences often have some nice structure that jumps around the sequence and breaks the linearity hypothesis. But who the hell thinks about inserting "randomly" in between elements in a sequence and building it up that way? 

In that sense, I think it's very much a question of, "can you stumble across this clever way to build a sequence" and break free of the "natural" way to think about these things. It's as if everybody were tasked with a drawing, and some of us instinctually gravitated towards pens/pencils, some towards brushes, some towards spray paint, without realizing it. 

It's not like there's some common unifying theory of how to make a stroke of art, nor is there some unifying theory on how to generate a sequence. It's not, "oh, this is obviously n^2 dp, okay now how to optimize?". It's not some extremely cheeky [MO style problem](https://codeforces.com/contest/1270/problem/G).

Of course, if you've seen this trick before, now you know in the future to watch out for this specific trick. But I like how this goes to show how deep hidden assumptions can run and ruin your problem solving. 

## Deduction?

There were not many solves on this in contest despite being so "simple". It seemed like everybody required at least 30 minutes to think of this observation. 

I genuinely just don't think there's a good argument for there being a straightforward, logical deduction from first principles here. Like there's no way that people said, "oh, look at the |B[i] - B[i + 1]| = 1 condition, surely you build this bottom up, right?" 

But the random walk argument also seems like an unsatisfying cop out. 

## Future Strategy

As I stated in the Takeaway section, I think that this kind of problem shows just how deep some hidden first biases can run and completely ruin your problem solving process. And that often is a frequent thing I'm worried about when solving these problems - I feel like my mind, way too often, latches onto something within minutes of reading a problem, and either that will carry me to solving it instantly or make me struggle for aeons. 

I had a stint where I tried to formalize everything, which I think is good. But I still struggle with this balance between intuition and formalism. In my mind, I have it as, intuition should guide me for X amount of steps, then formalism should immidiately clarify the search space around me locally to make sure I'm not making any obvious mistakes. **I'm not sure how to really advance this model right now.**

I've read before that one of tourist's strengths is just the ability to quickly iterate through tons of ideas. Yes, he's seen a lot, and yes, he's been so engrossed in math and probably has a much better intuitive grasp on some of these things than I do. 

## Conclusion

I feel like I have a lot to say yet not that much at all, so I'll just leave it here. This was a good note. 