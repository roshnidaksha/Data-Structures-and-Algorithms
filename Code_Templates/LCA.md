# Lowest Common Ancestor (Binary Lifting)

**Tags:** `#trees`, `#graphs`, `#binary-lifting`, `#dynamic-programming`

**Prerequisites:** Depth-First Search (DFS), Bit Manipulation, Sparse Tables

## 💡 The Core Concept
Binary lifting is an algorithmic technique used to efficiently jump up a tree. By precomputing the $2^i$-th ancestor of every node, it allows us to find the LCA - the lowest, or deepest, node in a tree that has both node `u` and node `v` as descendants. This precomputation enables you to answer path queries in logarithmic, or $O(\log N)$, time rather than traversing the tree step-by-step.

## ⏱️ Complexity
* **Time Complexity:** 
  * Build (Precomputation): $O(N \log N)$
  * Query (Find LCA): $O(\log N)$
* **Space Complexity:** $O(N \log N)$ to store the `up` (ancestor) table.


## 💻 C++ Implementation
*Optimized for competitive programming. Uses standard binary lifting state transitions.*

```cpp
#include <vector>
#include <cmath>
#include <algorithm>

using namespace std;

class LCA {
private:
    int n, log;
    vector<vector<int>> up;
    vector<int> depth;

    void dfs(int node, int parent, const vector<vector<int>>& adj) {
        up[node][0] = parent;
        
        // Precompute the 2^i-th ancestor
        for (int i = 1; i <= log; i++) {
            if (up[node][i - 1] != -1) {
                up[node][i] = up[up[node][i - 1]][i - 1];
            } else {
                up[node][i] = -1;
            }
        }
        
        for (int neighbor : adj[node]) {
            if (neighbor != parent) { // Prevent infinite loops
                depth[neighbor] = depth[node] + 1;
                dfs(neighbor, node, adj);
            }
        }
    }

public:
    LCA(int n, int root, const vector<vector<int>>& adj) {
        this->n = n;
        this->log = ceil(log2(n));
        up.assign(n, vector<int>(log + 1, -1));
        depth.assign(n, 0);
        dfs(root, root, adj);
    }

    int getLCA(int u, int v) {
        // Ensure u is the deeper node
        if (depth[u] < depth[v]) {
            swap(u, v);
        }
        
        // Equalize depths
        int diff = depth[u] - depth[v];
        for (int i = 0; i <= log; i++) {
            if ((diff >> i) & 1) {
                u = up[u][i];
            }
        }
        
        if (u == v) return u;
        
        // Jump up together
        for (int i = log; i >= 0; i--) {
            if (up[u][i] != up[v][i]) {
                u = up[u][i];
                v = up[v][i];
            }
        }
        
        return up[u][0];
    }
};
```

## 💻 Python Implementation
*Optimized for competitive programming and fast execution.*

```Python
import sys
import math

# Crucial for deep trees in Python
sys.setrecursionlimit(200000) 

class LCA:
    def __init__(self, n: int, root: int, adj: list[list[int]]):
        self.n = n
        self.log = math.ceil(math.log2(n)) if n > 1 else 1
        self.up = [[-1] * (self.log + 1) for _ in range(n)]
        self.depth = [0] * n
        
        self._dfs(root, root, adj)
        
    def _dfs(self, node: int, parent: int, adj: list[list[int]]) -> None:
        self.up[node][0] = parent
        
        for i in range(1, self.log + 1):
            if self.up[node][i - 1] != -1:
                self.up[node][i] = self.up[self.up[node][i - 1]][i - 1]
                
        for neighbor in adj[node]:
            if neighbor != parent:
                self.depth[neighbor] = self.depth[node] + 1
                self._dfs(neighbor, node, adj)
                
    def get_lca(self, u: int, v: int) -> int:
        if self.depth[u] < self.depth[v]:
            u, v = v, u
            
        diff = self.depth[u] - self.depth[v]
        for i in range(self.log + 1):
            if (diff >> i) & 1:
                u = self.up[u][i]
                
        if u == v:
            return u
            
        for i in range(self.log, -1, -1):
            if self.up[u][i] != self.up[v][i]:
                u = self.up[u][i]
                v = self.up[v][i]
                
        return self.up[u][0]
```

## ⚠️ Common Pitfalls & Edge Cases

* **Tree Depth Limits:** In Python, a deep tree (e.g., a straight line graph of $10^5$ nodes) will cause a recursion limit exceeded error in the `_dfs` initialization. Always add `sys.setrecursionlimit(200000)` at the top of your file when using this boilerplate.

* **0-indexing vs 1-indexing:** This boilerplate defaults to 0-indexing. If the problem uses 1-indexed nodes (from $1$ to $N$), instantiate arrays of size $N + 1$ and set your root accordingly.

* **Graph vs. Tree:** The edges input represents an undirected tree. The line `if neighbor != parent:` in the DFS is critical to prevent infinite loops.

* **Querying Distances:** The distance between any two nodes u and v can be instantly calculated once you have the LCA using the formula: `depth[u] + depth[v] - 2 * depth[lca]`.

## 🎯 Practice Problems (LeetCode)

Problem | Difficulty | Notes | 
|-------|-------|-------|
