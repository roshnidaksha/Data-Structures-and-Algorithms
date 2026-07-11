# Union Find (Disjoint Set Union)

**Tags:** `#data-structures`, `#graphs`, `#trees`

**Prerequisites:** Graphs, Connected Components, Recursion

## 💡 The Core Concept
The Union Find data structure keeps track of a set of elements partitioned into a number of disjoint (non-overlapping) subsets. It provides near-constant time operations to add new sets, merge existing sets, and determine whether elements are in the same set.

**Use Case:** When you need to group items dynamically, find connected components, or efficiently detect cycles in an undirected graph.

## ⏱️ Complexity
* **Time Complexity:** 
  * Build: $O(N)$
  * Query (Find): Amortized $O(\alpha(N))$ which is practically $O(1)$
  * Update (Union): Amortized $O(\alpha(N))$ which is practically $O(1)$
  *(Note: $\alpha$ refers to the Inverse Ackermann function, which grows so slowly it is $\le 4$ for all reasonable values of $N$)*
* **Space Complexity:** $O(N)$ to store the `parent` and `rank` arrays.

---

## 💻 C++ Implementation
*Optimized for speed and competitive constraints. Uses Path Compression and Union by Rank.*

```cpp
#include <vector>
#include <numeric>

using namespace std;

class UnionFind {
public:
    vector<int> parent;
    vector<int> rank;
    int count; // Tracks the number of disjoint components

    UnionFind(int n) {
        parent.resize(n);
        iota(parent.begin(), parent.end(), 0); // Fills 0, 1, ..., n-1
        rank.resize(n, 1);
        count = n;
    }

    int find(int x) {
        if (parent[x] == x) {
            return x;
        }
        // Path compression: dynamically points nodes directly to the root
        return parent[x] = find(parent[x]);
    }

    bool unite(int x, int y) {
        int rootX = find(x);
        int rootY = find(y);

        if (rootX != rootY) {
            // Union by Rank: attach smaller tree under root of larger tree
            if (rank[rootX] > rank[rootY]) {
                parent[rootY] = rootX;
            } else if (rank[rootX] < rank[rootY]) {
                parent[rootX] = rootY;
            } else {
                parent[rootY] = rootX;
                rank[rootX] += 1;
            }
            count--; // Merged two components into one
            return true; // Successfully merged
        }
        return false; // Elements are already in the same set (Cycle detected!)
    }
};
```

## 💻 Python Implementation
*Optimized for speed and competitive constraints.*

```Python
class UnionFind:
    def __init__(self, n: int):
        self.parent = list(range(n))
        self.rank = [1] * n
        self.count = n  # Tracks the number of disjoint components

    def find(self, x: int) -> int:
        if self.parent[x] != x:
            # Path compression
            self.parent[x] = self.find(self.parent[x])
        return self.parent[x]

    def unite(self, x: int, y: int) -> bool:
        root_x = self.find(x)
        root_y = self.find(y)

        if root_x != root_y:
            # Union by Rank
            if self.rank[root_x] > self.rank[root_y]:
                self.parent[root_y] = root_x
            elif self.rank[root_x] < self.rank[root_y]:
                self.parent[root_x] = root_y
            else:
                self.parent[root_y] = root_x
                self.rank[root_x] += 1
            
            self.count -= 1
            return True # Successfully merged
        
        return False # Elements are already in the same set (Cycle detected!)
```

## ⚠️ Common Pitfalls & Edge Cases

* **1D to 2D Mapping:** When using Union Find on grids (like the cycle detection problem), you usually need to map 2D coordinates (r, c) to a 1D index to pass it to the Union Find array. The formula is usually index = r * COLS + c.

* **0-indexing vs 1-indexing:** The boilerplate defaults to 0-indexing. If a problem uses 1-indexed node numbers (e.g., nodes $1$ to $N$), instantiate UnionFind(n + 1) and safely ignore index 0.

* **Forgetting Path Compression:** Forgetting parent[x] = find(parent[x]) in the find function degrades the time complexity from $O(1)$ to $O(N)$ in the worst-case (linked-list shaped trees).

## 🎯 Practice Problems (LeetCode)

Problem | Difficulty | Notes | 
|-------|-------|-------|
[Detect cycles in 2D grid](https://leetcode.com/problems/detect-cycles-in-2d-grid/) | Medium | Basic application of Union-Find. Requires mapping 2D (r, c) coordinates to 1D arrays. |
