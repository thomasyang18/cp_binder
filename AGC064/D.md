# D 

Date: 12-18-2024

---

Fuck dude. I remember looking at this problem, and my first instinct is like, there's probably some really nice property like you can just look at every pair of chips and reason about that, then count some reduced version of the problem. 

That still is the case, the issue is, well, at least if I try to observe that observation directly it feels impossible to prove. 

---

Also initially for some reason I thought that you flipped the order of chips like a stack, you don't do that.

----

Okay, so immidiately yeah, if we analyze for every pair of chips, their relative order does not change as soon as you put one chip on top of another. 

How can you possibly count the number of ways tho. 

----

Maybe it's sometimes always possible to get all configurations of chips? **No, you must terminate with blue, for example.**

Well, (B*) R (B*) will get you anything ending in B, lol. 

Right, recall that the starting configuration is important as well, and we just want the number of potential ending configurations. **This probbaly implies some direct "Do it" dp state machine simulation isn't gonna work**, we have to throw some observations at it. 

There's some structure here when the starting configuration is fixed and you must stack everything on top of blue squares. 

Let's see. 

====

Ah, for some reason I thought it was arbitrary; but no, you must do i-->n-1; if you could just do arbitrary swaps, clearly the answer is just (every combination of the first n-1 elements) since you can arbitrarily order them on the ending guy. 

----

Okay for the two stone case it only works if 


X ---- Y -------- N

with a B at a position >= Y. Otherwise, you can just do X to N, y To N. 

But if there's a B (not N) at >= Y, then you can do shit. 

----

Think backwards? Perhaps we have any final ending configuration; move some subset of stones backwards s.t. it remains legit, how many ending configurations can end (reversed) in the starting configurations? 

---

I don't think permutations matter at all lol. Like, the point is there's a lot of ways, right idk. 

Maybe we should just try for small examples and see what the answer is at this point. 

---

Also, one "duh" statement: At every step $i$, every single cell $j > i$ has stones in it. 

---

If we look at stone $n$ and stone $n-1$, a simple observation is that stone $n$ must always come after stone $n-1$. 

RB => RB
BB => BB

---
RRB => {RRB}
RBB => {RBB, BRB}
BRB => {BRB, RBB}
BBB => {BBB}

---
Gonna just keep writing brute forcers. 

