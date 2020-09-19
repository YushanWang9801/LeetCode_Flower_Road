# 1202 smallest string with swaps

You are given a string s, and an array of pairs of indices in the string pairs where pairs[i] = [a, b] indicates 2 indices(0-indexed) of the string.

You can swap the characters at any pair of indices in the given pairs any number of times.

Return the lexicographically smallest string that s can be changed to after using the swaps.

## General Idea

It is a graph problem rather a string problem. The point is to find which components are connected and which are not and then sort all the connected elements in the lexicographically smallest to largest. The way of finding all
the connected components would be using Union Find.

## Union Find

It is also could be called as disjoint-set/Union-find Forest

mainly would be two functions:

`find(x)` : find the root id of node `x`

`Union(x, y)` : merge two cluster which contain node `x` and node `y`

Time complexity could be `O(1)` for both functions if with optimization.

Two optimization could be donw:
> 1. made the tree flat during `find`
> 2. union by rank: merge law rank tree to high rank tree

Pesudo Code
```
Class UnionFindSet:
    func UnionFindSet(n):
        parents = [1...n]
        ranks = [0...0]

    func find(x):
        if x != parents[x]:
            parents[x] = Find(parents[x])
        return parents[x]
    
    func union(x,y);
        rx , ry = find(x), find(y)
        if ranks[rx] > ranks[ry] : parents[ry] = rx
        if ranks[ry] > ranks[rx] :
        parents[rx] = ry
        else:
            parents[ry] = rx
            ranks[rx]++ 

```


## Python
```python

class Solution(object):
    
    def smallestStringWithSwaps(self, s, pairs):
        """
        :type s: str
        :type pairs: List[List[int]]
        :rtype: str
        """
        
        # initialize a graph
        parent = [i for i in range(len(s))]
        
        # define find in union-find        
        def find(x):
            if x == parent[x]:
                return x
            return find(parent[x])
        
        # define union in union-find
        def union(x,y):
            px, py = find(x), find(y)
            if px > py:
                parent[px] = py
            else :
                parent[py] = px
                
        for pair in pairs:
            union(pair[0], pair[1])
        
        dict = {}
        for i in range(len(parent)):
            p = find(i)
            parent[i] = p
            dict[p] = dict.setdefault(p,'') + s[i]

        for key in dict.iterkeys():
            dict[key] = ''.join(sorted(list(dict[key])))

        result = ''
        for i in parent:
            result += dict[i][0]
            dict[i] = dict[i][1:]

        return result  

```