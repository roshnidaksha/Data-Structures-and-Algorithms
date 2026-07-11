# 1. Merge Sorted Array

**Link:** [Merge Sorted Array](https://leetcode.com/problems/merge-sorted-array)

**Difficulty:** Easy

**Topics:** Array, Two Pointers, Sorting

## Problem Description

You are given two integer arrays `nums1` and `nums2`, sorted in **non-decreasing order**, and two integers `m` and `n`, representing the number of elements in `nums1` and `nums2` respectively.

**Merge** `nums1` and `nums2` into a single array sorted in **non-decreasing order**.

The final sorted array should not be returned by the function, but instead be *stored inside* the array `nums1`. To accommodate this, `nums1` has a length of `m + n`, where the first `m` elements denote the elements that should be merged, and the last `n` elements are set to `0` and should be ignored. `nums2` has a length of `n`.

### Constraints:

* $nums1.length == m + n$
* $nums2.length == n$
* $0 \leq m, n \leq 200$
* $1 \leq m + n \leq 200$
* $-10^9 \leq nums1[i], nums2[j] \leq 10^9$

### Example

* Input: nums1 = [1,2,3,0,0,0], m = 3, nums2 = [2,5,6], n = 3
* Output: [1,2,2,3,5,6]
* Explanation: The arrays we are merging are [1,2,3] and [2,5,6].
* The result of the merge is [1,2,2,3,5,6] with the underlined elements coming from nums1.

## Brute Force Solution (Using STL Sort)

1. Traverse through `nums2` and append its elements to the end of `nums1` starting from index `m`.
2. Sort the entire `nums1` array using the `sort()` function.

### Complexity

* **Time Complexity:** $O((m + n)log(m + n))$ because we are sorting the entire array.
* **Space Complexity:** $O(1)$ because we are modifying `nums1` in place.

## Optimal Solution (Two Pointers)

1. Since both arrays are already sorted, we can be sure that the largest element after combining both arrays is one of `nums1[m - 1]` or `nums2[n - 1]`. Using this property, we can fill the final array backwards.

```c++
class Solution {
public:
    void merge(vector<int>& nums1, int m, vector<int>& nums2, int n) {
        int end = m + n - 1;
        m--;
        n--;
        while (n >= 0) {
            if (m >= 0 && nums1[m] >= nums2[n]) {
                nums1[end] = nums1[m];
                m--;
            } else {
                nums1[end] = nums2[n];
                n--;
            }
            end--;
        }
    }
};
```

**Loop Invariant:** `nums1[end + 1 ... m + n - 1]` contains the largest elements when combining both arrays and perfectly sorted. 

### Complexity

* **Time Complexity:** $O(m + n)$ because we process each element exactly once.
* **Space Complexity:** $O(1)$ because we are modifying `nums1` in place.