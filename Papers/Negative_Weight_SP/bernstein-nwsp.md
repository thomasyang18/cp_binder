# Bernstein's paper talk 

Date: 12-14-2024

[Link to talk](https://www.youtube.com/watch?v=Bpw3yqWT_d0)

---

His extensive speech at the beginning is important, I think. I think it's clear he's worried that like, they came out with this huge result that shattered so many graph algorithms, so what's the "point" of these simpler algorithms.

Aside form "it's cool", yeah simpler algorithms do elucidate new information. Well technically, any sort of knowledge has the potential to elucidate new information and build your worldview or whatever, but idk we don't have infinite time in the world. 

---

## Simplifying assumptions

- G has constant degree; what does this mean, why can't I have a high degree graph :(
- All edge weights are integral and have w(e) >= 1: okay... how the fuck is this legit? I mean I guess you can extend out an edge by padding ones, but like, cringe? 
- G does not contain negative-weight cycle: okay this reduction I know

## Price functions

- Define a function v(x) => Z s.t. now w(u, v) ==> w(u, v) + v(u) - v(v)
- by telescoping sum, this is the same (shortest paths), nice trick

### Goal:

Compute a price function s.t. the weights of all the edges are non-negative

### It always exists:

Just some dumb construction that assumes you've already computed the distances (source, v) for every node v, with the algebra you can rearrange the price function to be 

dist(s, u) + w(u, v) >= dist(s, v) which is literally triangle inequality.... wait, okay, for negative distances it's weird but still works. Because you can argue, "if this inequality was broken, then dist(s, v) could be updated to a lower value!" Even if it's negative distance. That's crazy.  

### The point 

Price functions => distances

Distances => price functions

Try successive approximations!

## More folklore algorihtms

BF + dijkstra can compute, assuming all shortest paths in the graph has only k negative edges, the solution in O(mk), just keep alternating between BF iteration, and dijsktra iteration, lmao. Pretty clever. 

### WTF? 

What does "average vertex" mean, constant degree, etc, whatever, I trust. So on average, the # of negative edges is low. 

### Intermediate goal

[16:19](https://youtu.be/Bpw3yqWT_d0?si=0Y5UgzyVSjgyu17D&t=979) I like don't understand what's going on but whatever, I trust that it reflects the iterative price function + distance relaxaiton.

## Low diameter decomposition... yeah....

Kind of crazy, you just have to rewatch the video, I think. It's like, wow. [16:47](https://youtu.be/Bpw3yqWT_d0?si=dgGR1hrinYbYY_4R&t=1007)

### High level algorithm (undirected, non negative)

Pick a random source, grow a ball (s, T) for some random distance T \in [0, D]. Disconnect the ball from the graph and conitnue (all edges on boundary are seperator edges)

#### Proof

In unweighted setting, if we just analyze the probability for any edge to be a seperator edge in this process, because we picked a random distance it's 1/D probability to get cut... hmm... ehh... It makes sense to me, but **I would have to verify it myself**, the proof exercise seems like it would be cool. 

- Oh, it's a geometric distribution? Huh... okay so yeah fair, you have to do the math and verify that this crazy idea works the way you say it does. 

### High level algorihtm (directed, non negative)

[21:54](https://youtu.be/Bpw3yqWT_d0?si=dgGR1hrinYbYY_4R&t=1007)

Huh... it's like, okay this sort of just got lost on me, yes I can "vibe" it out but now I don't really have an intuition for what this kind of decomposition is trying to do. The only thing that stands out intuitively to me is that the backedges (red edges) are all separator edges, sure, but like, what the fuck, you can like "break out" of your scc too? Using the red edges? That picture he drew is really good since it breaks a lot of my intuition for what's going on here. Seems cool. 

    Okay, jesus christ, so there isn't even like a direct translation for this algorihtm, not quite what they want. JFC yeah, this problem seems really complicated, I'm surprised even just random sampling works for the above. So they invented a good algorithm for this problem (or possibly re-derived this stuff)... wow lots of context. 

    Also it goes to show that even in academia, there's a lot of just context that you need to soak in; the difference between industry and academia obviously is that industry is closed while academia is (generally) open.

## Using LDD for negative edge weights?

[25:11](https://youtu.be/Bpw3yqWT_d0?si=mxqh_FdJfra5XoGH&t=1510)

w(e) = max(w(e), 0).

LMFAO. What. No fucking way this just works. 

### High level idea:

Call LDD for some large D estimate. Three types of edges to "fix" now 

1. SCC Edges get fixed to be non-negative (w/ price function)
2. DAG edges get fixed to be non-negative (w/ price function)
3. Seperator edges (red edges) get fixed to be non-negative (w/ price function)

(1) Is not easy, seems really hard. ??????????????????????
(2) is easy because DAGs are easy  
(3) is easy because there are few red edges

-----

## Okay, my brain is fried at this point lowkey. He didn't even get to the hard part yet. Maybe I will review this another time. But I got the basics down, I guess? 