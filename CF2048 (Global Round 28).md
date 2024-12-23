Date: 12-19-2024

---
A. Lol, misread for a bit. Definitely a bit of a contest math meme problem - you just have to know 11's divisibility rules which you'd only really remember if you were a contest math guy (even 3 divisiblity rules is a meme)

B. Greedy guess

C. Nice brute force/greedy proven solution ig 

D. This one... nice problem, misread it for a bit. Definitely should not have taken 20 minutes, I started typing up something hastily here.

E. This one, wow, it took 50 minutes? That's crazy. Regardless, I think I did this overall well? Like, I came up with a semi-construction, thought, "this shit is kinda sus", then kept refining it with more and more examples, it was hard for me to really reason about (tbh I still don't entirely understand the construction of my final submission, that one was more reasoning locally about stuff and then things just worked out, vs understanding the global construction)

Also, nice that we reasoned about the bound properly, thats good. 

F. A lot of observations here.

- Repeated ceil div is commuatative
- The answer $\leq$ 60
- "Min on interval" screams cartesian tree 

But then I started thinking about dumb DP formulations, involving the set of all prime factors, etc. and it's just like, okay.

I'm glad that this contest, I was able to pinpoint precisely what part of my solution was really sus (being dependent on value of a[i] cheese) and say, 'No thomas, bad thomas, don't try to cache every recursed interval and say that there can't be that many states.' 

It only clicked with me to do bottom up subproblem combining when I started thinking about it as, "For every element, there is an optimal interval that has this as the min" (cartesian tree). 

Then, you're not trying to keep a "stack" of operations going top down, you're just applying them immidiately bottom up, and notice that only the "max" of an interval matters.

- So sometimes the DP DFS formulation is the best, sometimes the bottom up subproblem interpretation is the best. 

---

But even once I got the high level details, optimizing it was a pain. I knew there was something called min + convolution but had no idea how to apply it, so then worked on constant factor opt like noticing that b[i] = 2 doesn't need to be done, so log_3(40) is at most the only thing I have to keep in mind. 

Good problem overall, I think. You can argue it's a classic DP where you can either store (a, b) in the DP state and optimize for the other, where the "value" of your DP state has some nice properties going for it. 

But it's a good kind of "inverted thinking" problem IMO. 

---
# Lessons Learned

I think this was a good contest. Importantly, **even though we fucking bricked on some problems, we dug ourselves out** every time. 

I think this is just more sustainable and healthy than expecting us to just 1 shot the problems every time. 

---

I think there is a pang of regret of not taking this attitude earlier, but I suppose that's just maturity diff, really. 

I mean, this is definitely a skill that you have to develop in contest, this not-tilting attitude. It could transfer to other parts of life, but only superficially. 

---

Oh yeah, and sometimes we don't even have a polynomial time algorithm for it and we immidiately go to cheese. I mean obviously there's always going to be a line and for here, I don't think it's possible to have a poly time algorithm without observing that a[i] can be reduced but like... man, c'mon. Suspicious bro. 

---

# Reading Editorial

Wow, C actually has an O(n) solution, though I'm not that surprised. It seems like one of those gigantic brain greedy problems. Thank god they didn't set O(n) (though I bet you could cheese with string hashing bullshit)

Reading E, I feel like a total dumbass. Like, dude. The equation is obvious if you just work out the algebra, but I still wrote a brute force :holyfuck: 

Skill Issue 

Reading F, **exactly.** 

So like, yes, we can make a cartesian tree and think about recursing subproblems. But what **actually** makes progress is thinking about **the set of intervals you can operate on** - I thought those exact words, and then the bottom-up DP came naturally. 

It's kind of crazy 

The min/max convolution stuff I probably should read... at the same time, skill issue teehee. 

I think, if I want to get better, I should definitely just set aside the whole day for just reviewing after a CF Round, I've literally never done that. 

---

Nah, maybe back in college I should've done that. Now, I think it's fine to want to upsolve a whole set, but like, idk do it at your own pace, we got other shit to worry about too. 

---

WTF  [LOL](https://codeforces.com/blog/entry/137387?#comment-1229227) literally problem C just showed up before orz 