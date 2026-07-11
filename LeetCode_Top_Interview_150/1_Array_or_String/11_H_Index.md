# 11. H-Index

**Link:** [H-Index](https://leetcode.com/problems/h-index)

**Difficulty:** Medium

**Topics:** Array, Sorting, Counting Sort

## Problem Description

Given an array of integers `citations` where `citations[i]` is the number of citations a researcher received for their `ith` paper, return the *researcher's h-index*.

According to the definition of h-index on Wikipedia: The h-index is defined as the maximum value of `h` such that the given researcher has published at least `h` papers that have each been cited at least `h` times.

### Constraints:

* $n == citations.length$
* $1 \leq n \leq 5000$
* $0 \leq citations[i] \leq 1000$

### Example

* **Input:** citations = [3,0,6,1,5]
* **Output:** 3
* **Explanation:** [3,0,6,1,5] means the researcher has 5 papers in total and each of them had received 3, 0, 6, 1, 5 citations respectively.
Since the researcher has 3 papers with at least 3 citations each and the remaining two with no more than 3 citations each, their h-index is 3.


## Optimal Solution (Binary Search)

Given a value of `h`, to check if it is a valid h-index, we need to iterate through all the `citations` and check whether it has been cited at least `h` times. This would take $O(n)$ if there are `n` citations.  

The maximum value `h` can take is the total number of citations available. According to the constraints, this is a maximum of 5000. Therefore, even if we iterate through all the possible values, we would still pass the question. 

Let's say `i` is a valid h-index. Then we need to note that all values $\leq i$ is also a valid h-index. This shows that we can use binary search on the result array, reducing the time complexity.

```c++
class Solution {
public:
    bool isHIndex(vector<int>& citations, int h) {
        int count = 0;
        for (int citation: citations) {
            if (citation >= h) count++;
        }
        return count >= h;
    }

    int hIndex(vector<int>& citations) {
        int minH = 0;
        int maxH = citations.size();

        int ans = 0;

        while (minH <= maxH) {
            int mid = (minH + maxH) / 2;
            if (isHIndex(citations, mid)) {
                ans = mid;
                minH = mid + 1;
            } else {
                maxH = mid - 1;
            }
        }
        return ans;
    }
};
```

### Complexity

* **Time Complexity:** $O(n \times logn)$ as we need $O(n)$ to check if an index is a valid h-index and we do the computation $O(logn)$ times.
* **Space Complexity:** $O(1)$