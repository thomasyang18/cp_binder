# Expected Distance

Date: 11-28-2024

[Link to Problem](https://codeforces.com/gym/102979/problem/E)

## Approach

Do the standard distance trick, dep[u] + dep[v] - 2 * dep[lca(u, v)], and work out the formulas. 

The hard part obviously is dep[lca(u, v)]. 

You have this [cryptic comment](https://codeforces.com/blog/entry/87916?#comment-768039), but basically what it's saying is that if you consider the given algorithm for lca(u, v) for u < v, lca(u, a) = lca(u, b) as long as u < a  and u < b, for all v. 

Which **only** works as a quirk of the system; because of how the tree is generated, you know the LCA must be a node <= u, and for nodes > u, you will just reroll anyways, so they're basically all the same. 

This means when you write out how the transitions look for a given lca(u, v), you can just sort of disregard v entirely and just look at just the higher node. 

## Moral of the story

Cute problem that really abuses how the tree is generated. It's just cached here in the back of my mind; I don't really know how to generalize this. 

I guess here, you write out the obvious equations; then you have to start thinking about, "hey, do I actually need these equations?" Although you have to also start thinking about this weird LCA setup to get to the crucial observation. 

I guess among all the LCA formulations, this is one that's easy to put into a recursive form that also doesn't rely on the depths of the nodes (although you'd have to make the observation that again, that LCA(u, v) <= min(U, v) for this graph specifically. )

Cheesy? Very. But there's still some takeaways to look for. 