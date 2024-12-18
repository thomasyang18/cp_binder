# Solved C Natty :) 

Date: 12-18-2024

---

Nice, really proud of us to stick through this without checking editorial.

As soon as we had the idea to merge the modulos and then just like do the game tree DP straight up, like, GG :) 

I have no clue what the editorial approach is; perhaps we should view it later? It seems like this equation is doing all the heavy lifting

```
An interval is deleted iff floor((l + x)/2^k) = floor((r + 1 + x) / 2^k)
```

I didn't think about when an interval would be deleted; instead I thought about it as y modulo 2^x as a suffix, but maybe it ends up being the same thing. No idea what they're doing with the dp though. 