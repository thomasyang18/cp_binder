# Mex Tree CF1830D

Date: 12-18-2024

[Link to Problem](https://codeforces.com/contest/1830/problem/D)

## Approach

First, notice that a bipartite coloring easily gets us MAX - 3n value. 

Then, thinking a bit more, we can reframe the cost as MAX - 2 * (for every 1 component, sz choose 2) - (for every 0 component, sz choose 2). 

So we can just do tree DP, keeping track of the size of the components which can be at most sqrt(N), for a total of NsqrtN. 

---

So, some nice observations, which then leads to the classic tree DP meme solution. 

---

I was actually thinking about, "okay, what if we didn't have to do this tree DP? Perhaps there's some local properties about a tree that are nice?" and I don't know if there's anything but it turns out that k=258 works because of some inequalities they got. Wow, that's insnae.

---

#### Side Note

Side note, I'm feeling a bit annoyed at going through old problems. They're nice for sure, but like, I'm not really doing anything with it, it's more like, "huh". 

I think reflecting isn't really enough, because it's only in the context of, "how do I solve this problem." Extending it is more important. That being said, like it goes back to "why am I doing this over anything else more useful." 

I *claim* that it's because "I'll find something fun", but like, most observations are sort of just dead ends, idk. Nevertheless it's important to keep on writing. 