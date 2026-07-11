# Segment Tree

**Tags:** `#data-structures`, `#range-queries`, `#divide-and-conquer`

**Prerequisites:** Recursion, Binary Trees, Divide and Conquer

## 💡 The Core Concept
*A Segment Tree is a highly flexible binary stree data structure that stores aggregated information (like sum, max, or min) about intervals of an array.*

**Use Case:** When you need to query ranges and update individual array elements dynamically and efficiently in logarithmic time.

## ⏱️ Complexity
* **Time Complexity:** 
  * Build: $O(N)$
  * Query: $O(\log N)$
  * Update: $O(\log N)$
* **Space Complexity:** $O(N)$ (Specifically, an array of size 4N is allocated to safely ensure enough space for all leaf and internal nodes of a full binary tree representing N elements).


## 💻 C++ Implementation
*Optimized for speed and competitive constraints.*

```cpp
#include <vector>
#include <algorithm>

using namespace std;

class SegmentTree {
private:
    vector<long long> tree;
    int n;

    long long merge(long long left_child, long long right_child) {
        return max(left_child, right_child); 
    }

    void build(int node, int start, int end, const vector<long long>& arr) {
        if (start == end) {
            tree[node] = arr[start];
            return;
        }
        int mid = start + (end - start) / 2;
        build(2 * node + 1, start, mid, arr);
        build(2 * node + 2, mid + 1, end, arr);
        tree[node] = merge(tree[2 * node + 1], tree[2 * node + 2]);
    }

    void update(int node, int start, int end, int idx, long long val) {
        if (start == end) {
            tree[node] = val; // Update Base Case
            return;
        }
        int mid = start + (end - start) / 2;
        if (idx <= mid) {
            update(2 * node + 1, start, mid, idx, val);
        } else {
            update(2 * node + 2, mid + 1, end, idx, val);
        }
        tree[node] = merge(tree[2 * node + 1], tree[2 * node + 2]);
    }

    long long query(int node, int start, int end, int l, int r) {
        if (l > end || r < start) return 0; // Identity value
        
        if (l <= start && end <= r) return tree[node];
        
        int mid = start + (end - start) / 2;
        long long left_res = query(2 * node + 1, start, mid, l, r);
        long long right_res = query(2 * node + 2, mid + 1, end, l, r);
        
        return merge(left_res, right_res);
    }

public:
    SegmentTree(int size) {
        n = size;
        tree.assign(4 * n + 1, 0); 
    }

    SegmentTree(const vector<long long>& arr) {
        n = arr.size();
        tree.assign(4 * n + 1, 0);
        build(0, 0, n - 1, arr);
    }

    void update(int idx, long long val) {
        if (idx < 0 || idx >= n) return;
        update(0, 0, n - 1, idx, val);
    }

    long long query(int l, int r) {
        if (l > r || l < 0 || r >= n) return 0; // Identity value
        return query(0, 0, n - 1, l, r);
    }
};
```

## 💻 Python Implementation
*Optimized for speed and competitive constraints.*

```Python
class SegmentTree:
    def __init__(self, size_or_arr):
        if isinstance(size_or_arr, int):
            self.n = size_or_arr
            self.tree = [0] * (4 * self.n + 1)
        else:
            self.n = len(size_or_arr)
            self.tree = [0] * (4 * self.n + 1)
            self._build(0, 0, self.n - 1, size_or_arr)

    def _merge(self, left_child: int, right_child: int) -> int:
        return max(left_child, right_child)

    def _build(self, node: int, start: int, end: int, arr: list) -> None:
        if start == end:
            self.tree[node] = arr[start]
            return
        mid = start + (end - start) // 2
        self._build(2 * node + 1, start, mid, arr)
        self._build(2 * node + 2, mid + 1, end, arr)
        self.tree[node] = self._merge(self.tree[2 * node + 1], self.tree[2 * node + 2])

    def _update(self, node: int, start: int, end: int, idx: int, val: int) -> None:
        if start == end:
            self.tree[node] = val # Update Base Case
            return
        mid = start + (end - start) // 2
        if idx <= mid:
            self._update(2 * node + 1, start, mid, idx, val)
        else:
            self._update(2 * node + 2, mid + 1, end, idx, val)
        self.tree[node] = self._merge(self.tree[2 * node + 1], self.tree[2 * node + 2])

    def update(self, idx: int, val: int) -> None:
        if idx < 0 or idx >= self.n:
            return
        self._update(0, 0, self.n - 1, idx, val)

    def _query(self, node: int, start: int, end: int, l: int, r: int) -> int:
        if l > end or r < start:
            return 0  # Identity value
            
        if l <= start and end <= r:
            return self.tree[node]
            
        mid = start + (end - start) // 2
        left_res = self._query(2 * node + 1, start, mid, l, r)
        right_res = self._query(2 * node + 2, mid + 1, end, l, r)
        
        return self._merge(left_res, right_res)

    def query(self, l: int, r: int) -> int:
        if l > r or l < 0 or r >= self.n:
            return 0  # Identity value
        return self._query(0, 0, self.n - 1, l, r)
```

## ⚠️ Common Pitfalls & Edge Cases

* **0-indexing vs 1-indexing:** This implementation strictly uses 0-indexing for both the array elements and the internal tree nodes.

* **Off-by-one errors:** Always initialize the tree array size to 4 * n + 1 (or 4 * n) to prevent out-of-bounds access at the deepest leaf nodes.

* **Data types:** In C++, long long is heavily recommended to prevent integer overflow, especially for range sum queries. (Python automatically handles arbitrarily large integers, so standard int is fine).

* **The "Identity Value":** Ensure the out-of-bounds return value in your query method won't corrupt your merge function. 
    * 0 for Sum/GCD/XOR
    * -1e18 (or float('-inf')) for Max
    * 1e18 (or float('inf')) for Min

* **merge() function:** This dictates what the tree actually calculates. Update the function here to what you want.

* **Base Case in update():** Look at `tree[node] = val` inside the `update` method. This dictates how new values interact with the tree?
    *  If the problem says "replace the value at index `i` with `x`": `tree[node] = val;`

    * If the problem says "add `x` to the value at index `i`": `tree[node] += val;`

    * DP maximums: `tree[node] = max(tree[node], val);`


## 🎯 Practice Problems (LeetCode)

Problem | Difficulty | Notes | 
|-------|-------|-------|
[Good Subsequence Queries](https://leetcode.com/problems/good-subsequence-queries/description/) | Hard | Uses a Segment Tree to efficiently maintain and query the Greatest Common Divisor (GCD) of array elements after dynamic point updates.  |
[Maximum Sum of Alternating Subsequence With Distance at Least K](https://leetcode.com/problems/maximum-sum-of-alternating-subsequence-with-distance-at-least-k/description/) | Hard | Uses delayed Segment Tree updates (a "waiting room" window) and range maximum queries to enforce distance constraints while optimizing DP transitions. |