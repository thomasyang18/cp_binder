# K : Connect The Dots

Date: 12-1-2024

[Link to Problem](https://codeforces.com/gym/104427/problem/K)

---

Only 31 solves in gym. Kinda crazy. And it took us the whole night to stumble across this surprisingly simple observation. 

## Crucial Observation

First, every adjacent pair of points can be merged for free (as well as 1 and n). This should be obvious.

Now, suppose we merge two points that look like this 

```
x y z
```

and merge x z. **In the new array, it's the same problem, as if y disappeared.**

If we have two colors, this doesn't seem always possible, e.g. 

```
x y x y x y x  
```

no two direct adjacent merges are valid. On the other hand, when there are three or more distinct colors, you can always retain 3 distinct colors after every step (until you get down to an array of size 2) using only adjacent merges.

This can be shown because:

- if every adjacent pair of elements is different, then because there are 3 colors, at some point "xyz" must occur 
- otherwise, there exists a pair of same elements; because not all elements are the same color, the sequence "xxy" exists

This leads us to consider this constructive solution: First, while there exists adjacent elements, merge them together. Now, no pairs of elements are distinct. Maintain some linked list stuff over which tuples (a[i], a[i + 2]) are different and you're done. 

---

In the 2 color case, I dunno, just do the obvious greedy. Where like: 

```
x y y => x y
x x x x x y ==> x y
x y x... just let it be 
```

etc. while sweepling left to right, and eventually you'll be left with some forced alternating blocks if you're unlucky at the end. I have no proof for this but this AC's. 

## Editorial solution:

Treat it as a circular array, then treat it as a polygon, and a lot of it becomes more obvious. I don't really understand editorial solution; it seems like it doesn't handle

```
x y x y x y x y x y x y z 
```

cleanly but w/e.

Also their proof for the 2 color case is neat, I had no proof for that (whereas you can just show N - 3 is maximal by construction). 

## Reflection

So the observation that >= 3 colors behaves differently from 2 colors is very, very subtle and tricky, but seems obvious in hindsight. For example, I got the 2 * n - 3 - (# adjacent similar) bound extremely quickly, and conjectured that maybe it's always possible. Turns out its not, but only in the m=2 case (sometimes). 

I didn't even get the inspiration until Nick pointed out that, hey, if we have a bad doodoo case like 

```
x y x y x y x y x y 
```

this can all just be fixed maximally by just appending 1 "z". Connect every color with this "z", and it reduces back to the 2 color case, which seems like it would have a nice solution. 

As it turns out, this greedy algorithm for the 3 color case is pretty wrong, because the 2 color case can't always be maximally done. So you don't want to remove distinct colors until you're down to your last legs. 

Nevertheless, combining all of these "wrong" "observations", with the brute force observation, actually lead to the correct algorithm immidiately afterwards (though it was still a pain to type up).

I say "wrong" and "observation" because:

- Observations aren't really lemmas, and especially not here. Nick did make a statement, it definitely did not generalize to a correct algorithm, but it still gave inspiration from one. Very weird. 

- Wrong, because yeah, that naive algorithm again is totally wrong in the 3 color case, you have to be a lot more careful than that. But is it "wrong" if some feeling of that statement lead to the correct conclusion? 

Plus, if it wasn't paired with my brute forcer, I don't know if I would've even noticed that hey, the 3 color case is special (since only 2 color case was failing). 

And again, it's not like this problem's easy. It has the 3rd lowest number of solves in this gym, behind (what seems like) some insane garbage math problem. It had less solves than a small to large merging li chao tree problem FFS (and in my opinion the transitions were not that obvious, though maybe to someone more experienced it would be standard). 

So the observation isn't hard because of knowledge diff, it's not hard because it's some insane object (for example, I still suck at reasoning about advanced flow and linear algebra stuff, just because it seems so heavy on the theorems sometimes...). It's not hard because it's some insane genius big brain monotonic greedy algorithm that's paper worthy, or some insane casework bash, or some insane data structure maintenance (which yes, can be creative too! But a lot of code and a lot of good impl skills). It's not some pattern you just directly brute force (at least to me; I brute forced it and didn't make the connection of 3+ colors). 

It's just... hard. Which is what I like the most about CP. So this is a really great problem. 10/10. 

Reminds me a lot of [this IMO problem](https://artofproblemsolving.com/wiki/index.php/2024_IMO_Problems/Problem_5) and [this IMO problem](https://artofproblemsolving.com/wiki/index.php/2023_IMO_Problems/Problem_5), these two are very codeforces-ey.