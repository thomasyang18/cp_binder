# ARC 164 Review 

Date: 11-27-2024

---

idk mannnnnn why am I literally reviewing 1300 problems 

whatever. I think this is something I'm doing years too late. I'm not even sure what I want out of this, but w/e let's roll with it. 

## B

Really cute problem lol, the "what you should do" is pretty obvious, "how you should do it" is not and I was really satisfied when I came up with the "think about spanning trees only w.r.t. 01 edges". I feel like you can definitely make this idea into a very hard problem with this still has the core observation. 

## C 

The way I finally convinced myself of the solution was to group up cards into pairs. Label "10" cards as a[i] > b[i], and "01" cards as a[i] < b[i].

01 cards are standalone, if alice touches them you get the full value.

When (10 10) groups up, if alice picks one, you can always pick the other, and now alice can never touch the other card (which is now a 01), 

The only issue is when (10) can't pair up, you have to consider when (10) pairs up with (01), you can actually just fail either pair if you do the casework. 

I was trying to do some monotonic greedy global exchange argument, and sure while you can do that (ig it's used implicitly in this analysis), the more important thing is really just trying to limit the complexity of the game. Although I first arrived at the pairing intuition through sorting a[i] - b[i] and then trying to greedily pair things up. 

Overall, felt like a standard problem, not really sure why I'm writing an essay for a 1600 problem that I didn't really find that memorable. 

I think forcing myself to say out loud the steps I took, even if trivial, is helpful, ultimately? I feel like my self reflection in general is horrible - I tend to hate metrics since then you start optimizing for them (e.g. problem rating! but also things like time solved, penalty, etc.), but my framework building is kinda doodoo as well lol. 

## D 

<details>
<summary>
random musings
</summary>
Immidiately upon reading this problem, though I have no real belief that this observation is useful; frequently in CP or just the math literature in general, there's this "reflection trick" that you can do to handle uniform, momentum conserving collisions between things. For example, Maxwell's Daemon in WF 2024 or the classic ants on a line problem. 

I feel like this kind of problem, idk, is kinda dumb? Though "what I know is standard and uninspiring and what I invent right now is cool" is a very real phenomenon :(  
</details>

------------------

[D and E to be looked at later...]