# Collection 1 

Date: 11-29-2024

## Bracket Merging problem 

### Statement 

Given a directed graph with n vertices and 2m edges, each edge has a label representing either a left parenthesis or a right parenthesis. There are k different types of parentheses, meaning there can be 2k different labels in total. The vertices, edges, and types of parentheses are all numbered starting from 1.

Each edge in the graph appears in pairs. Specifically, if there is an edge labeled with the w-th type of left parenthesis from vertex u to vertex v, then there must also be an edge labeled with the w-th type of right parenthesis from v to u. Similarly, every right parenthesis edge will correspond to a reverse-direction left parenthesis edge of the same type.

Count the number of pairs of vertices 1 <= x <= y <= n s.t. there exists a correct bracket sequence path from x to y. 

### Result 

Because of the reverse condition, that means that whenever a node has multiple in-edges of the same color, there's a path between them. 

(You can always "shorten" a valid path by merging these two, which proves the correctness of this algorithm.)

That observation comes pretty naturally, I think the implementation can be tricky if you think about it wrong; it certainly messed up our team and Mines. 

You want to code your merge function something like this 

```
void merge(ll u, ll v) {
    u = find(u), v = find(v);
    if (u - v) {        
        if (in_edges[u].size() < in_edges[v].size()) swap(u, v);

        dsu[v] = u;
        vector<pl> merges;
        for (auto &[color, vec]: in_edges[v]) {
            if (!in_edges[u].count(color)) in_edges[u][color] = vec;
            merges.emplace_back(vec, in_edges[u][color]);
        }
        in_edges[v].clear();
        for (auto [x, y]: merges) merge(x, y);
    }
}
```

because you don't want to have mutating state in the middle of recursion; finalizing your state beforehand 

It's not really a super clean, nice dsu problem, I just thought that that was interesting, and specifically that I messed up implementaiton that bad in contest specifically because I didn't think about it like this. It's more of a "find the right formulation" problem than a DSU problem. Nevertheless, I found the main observation pretty cute, if not very obvious from the contrived problem statement. 

Having different types of brackets is an interesting twist, since usually bracket sequences just deal with 1 type of sequence. This did yield some nice tree ideas. 

## USACO Plat 2 Dec 2023 

[Problem Link](https://usaco.org/index.php?page=viewproblem2&cpid=1357)

A bit standard, eventually got it in contest, though in hindsight it took embarassingly long (4 hours I believe). 

It's pretty obvious that you have to consider the edges in sorted order. I solved for the path graph case using some cartesian tree stuff, since it's clear you have to divide up the graph into two, but I had no idea how to generalize that; I was way too focused on dividing up the graph instead of thinking backwards. 

The critical observation is that once you enter a connected component from a certain node, you know both the sequence of nodes you must take before and after that merge is, and you just apply it to all nodes in your connected component. But that requires thinking from a kruskal's approach; I suppose it may be more obvious if you think from that perspective initially. 

This seems like a standard problem...? If nothing else, I remember how much time I wasted on this shit though. 