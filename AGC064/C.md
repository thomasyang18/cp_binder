# C 

Date: 12-15-2024

----

The operation is basically, "delete all 0s or 1s, then bitshift." 

This yields a reversed trie perspective I think.

----

The operation for [l, r] is equiv to 

[l, r] ==> fix parities by +1,-1 to delete the corresponding elements, then floor divide

Like, in that sense, there's the exponentially going to 0 bit, but also the exponentially shrinking range bit. IDK. 

---

If we have the range [2^x, 2^(x + 1)) and [2^(x + 1), 2^(x + 2)) , we can discard the first range. Because any and all halving shenanigans must go through the second (same copies etc.)



More generally, if some guy is like 

XXXX[same suffix]
....[same suffix]

if some guy is superceded like that we can discard the same suffix. 

----

This implies that we can move L up until it's impossible that that's true or something? Not sure. 

Definitely means something like at least R < 4 * L, or some small constant times L, as otherwise we could move it up. 

That... still doesn't really mean anything tho. Like 

[1e9, 4e9]

still takes 30 iterations to converge. Sadge. 


It is mod boundaries, it's X mod 2^y for some X, y. 

----

No can do. Brain stalling. Gonna go read a book 
