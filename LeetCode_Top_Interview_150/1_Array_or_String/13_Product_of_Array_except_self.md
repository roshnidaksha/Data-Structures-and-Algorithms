# 13. Product of Array except self

**Link:** [Product of Array except self](https://leetcode.com/problems/product-of-array-except-selfy)

**Difficulty:** Medium

**Topics:** Array, Prefix Sum

## Problem Description

Given an integer array `nums`, *return an array `answer` such that `answer[i]` is equal to the product of all the elements of `nums` except `nums[i]`*.

The product of any prefix or suffix of `nums` is **guaranteed** to fit in a **32-bit integer**.

You must write an algorithm that runs in `O(n)` time and *without using the division operation*.
 

### Constraints:

* $2 \leq nums.length \leq 10^5$
* $-30 \leq nums[i] \leq 30$
* The input is generated such that answer[i] is guaranteed to fit in a 32-bit integer.

### Example

* **Input:** nums = [1,2,3,4]
* **Output:** [24,12,8,6]

## Brute Force Solution

Brute force solution is to traverse the entire array to calculate the product for every index. This takes $O(n^2)$ time and $O(1)$ space and would give a TLE.

## Better Solution

A better approach is to keep track of two arrays - prefixProd and suffixProd.
For every index in the output, we multiply the corresponding values in the above arrays.

### Complexity

* **Time Complexity:** $O(n)$ 3 passes through arrays of length `n`
* **Space Complexity:** $O(n)$ to store prefix and suffix arrays

## Optimal Solution

The space occupied can be improved by using the output array to calculate prefix/suffix. Then for the other, use a variable (suffix/prefix). 

```c++
class Solution {
public:
    vector<int> productExceptSelf(vector<int>& nums) {
        int n = nums.size();

        vector<int> ans(n, 1);
        for (int i = 1; i < n; i++) {
            ans[i] = ans[i - 1] * nums[i - 1];
        }

        int suffix = 1;
        for (int i = n - 1; i >= 0; i--) {
            ans[i] = ans[i] * suffix;
            suffix *= nums[i];
        }
        return ans;
    }
};
```

### Complexity

* **Time Complexity:** $O(n)$ 2 passes through arrays of length `n`
* **Space Complexity:** $O(1)$