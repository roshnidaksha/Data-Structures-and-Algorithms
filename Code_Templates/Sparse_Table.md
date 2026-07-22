# Sparse Table

**Tags:** `data-structures`, `arrays`, `binary-lifting`

**Prerequisites:** Bit manipulation, Dynamic Programming, Logarithms

## 💡 The Core Concept
A Sparse Table is a data structure that allows you to answer range queries on a **static** (immutable) array. It precomputes answers for all intervals of length $2^j$ using dynamic programming. Any arbitrary range can then be constructed by overlapping two precomputed intervals of a power of 2. 

**Use Case:** When you have an array that *does not change* (no updates), and you need to perform massive amounts of Range Minimum, Range Maximum, Range GCD, or Range Bitwise Queries in absolute $O(1)$ time.

## ⏱️ Complexity
* **Time Complexity:** 
  * Build: $O(N \log N)$
  * Query: $O(1)$ (for idempotent operations like Min, Max, GCD, AND, OR)
* **Space Complexity:** $O(N \log N)$ to store the 2D table.

## 💻 C++ Implementation
*Optimized for competitive programming. Uses `std::__lg` for ultra-fast $O(1)$ logarithm calculations.*

```cpp
#include <vector>
#include <algorithm>
#include <cmath>

using namespace std;

class SparseTable {
private:
    vector<vector<int>> st;
    int n;
    int k; // Maximum power of 2 needed

public:
    SparseTable(const vector<int>& arr) {
        n = arr.size();
        k = __lg(n); // fast log2
        
        st.assign(n, vector<int>(k + 1));
        
        // Base case: intervals of length 1 (2^0)
        for (int i = 0; i < n; i++) {
            st[i][0] = arr[i];
        }
        
        // Build the table using DP
        for (int j = 1; j <= k; j++) {
            for (int i = 0; i + (1 << j) <= n; i++) {
                // Change `min` to `max`, `__gcd`, etc., based on the problem
                st[i][j] = min(st[i][j - 1], st[i + (1 << (j - 1))][j - 1]);
            }
        }
    }
    
    // 0-indexed, inclusive range [L, R]
    int query(int L, int R) {
        if (L > R) return 0; // Prevent __lg(0) and return identity value
        
        int j = __lg(R - L + 1);
        // Overlap the two halves to get the answer in O(1)
        return min(st[L][j], st[R - (1 << j) + 1][j]); 
    }
};
```

## 💻 Python Implementation
*Optimized for readability. Python's `bit_length()` is used as a fast alternative to `math.log2()`.*

```Python
class SparseTable:
    def __init__(self, arr: list[int]):
        self.n = len(arr)
        self.k = self.n.bit_length()
        
        # Initialize table with 0s
        self.st = [[0] * self.k for _ in range(self.n)]
        
        # Base case: intervals of length 1
        for i in range(self.n):
            self.st[i][0] = arr[i]
            
        # Build the table
        for j in range(1, self.k):
            for i in range(self.n - (1 << j) + 1):
                # Change `min` to `max`, `math.gcd`, etc.
                self.st[i][j] = min(self.st[i][j - 1], self.st[i + (1 << (j - 1))][j - 1])
                
    # 0-indexed, inclusive range [left, right]
    def query(self, left: int, right: int) -> int:
        length = right - left + 1
        j = length.bit_length() - 1
        
        # Overlap the two halves
        return min(self.st[left][j], self.st[right - (1 << j) + 1][j])
```

## ⚠️ Common Pitfalls & Edge Cases

* **Idempotent Operations Only:** The $O(1)$ query trick only works for operations where overlapping elements don't change the result (e.g., $min(x, x) = x$). If you try to use this for Range Sum Queries, the overlapping segment gets counted twice! (Use Prefix Sums or a Fenwick tree for sums instead).

* **Updates:** Sparse Tables are strictly for static data. If the problem requires you to update values in the array, you must abandon the Sparse Table and use a Segment Tree.

* **C++ Logarithms:** Avoid using the standard log2() function inside your query loop in C++, as floating-point math is relatively slow. Use std::__lg(x) for integers.

## 🎯 Practice Problems (LeetCode)

Sparse Tables are often used to crush problems where Segment Trees are slightly too slow, or as a building block for Lowest Common Ancestor (LCA) algorithms.

Problem | Difficulty | Notes | 
|-------|-------|-------|
| [Maximum Total Subarray Value II](https://leetcode.com/problems/maximum-total-subarray-value-ii) | Hard | Use Sparse table for fast range min and max queries
