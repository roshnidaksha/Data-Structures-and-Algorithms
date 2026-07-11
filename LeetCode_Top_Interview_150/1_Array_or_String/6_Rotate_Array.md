# 6. Rotate Array

**Link:** [Rotate Array](https://leetcode.com/problems/rotate-array)

**Difficulty:** Medium

**Topics:** Array, Math, Two Pointers

## Problem Description

Given an integer array `nums`, rotate the array to the right by `k` steps, where `k` is non-negative.

### Constraints:

* $1 \leq nums.length \leq 10^5$
* $-2^{31} \leq nums[i] \leq 2^{31} - 1$
* $0 \leq k \leq 10^5$

### Example

* **Input:** nums = [1,2,3,4,5,6,7], k = 3
* **Output:** [5,6,7,1,2,3,4]
* **Explanation:** <br>
rotate 1 steps to the right: [7,1,2,3,4,5,6] <br>
rotate 2 steps to the right: [6,7,1,2,3,4,5] <br>
rotate 3 steps to the right: [5,6,7,1,2,3,4]

## Brute Force Solution

The simplest way is to perform `k` single rotations. In each rotation, we save the last element, shift every element one position to the right, and place the saved element at the front. 

```c++
class Solution {
public:
    void rotate(vector<int>& nums, int k) {
        int n = nums.size();
        k %= n;
        while (k > 0) {
            int tmp = nums[n - 1];
            for (int i = n - 1; i > 0; i--) {
                nums[i] = nums[i - 1];
            }
            nums[0] = tmp;
            k--;
        }
    }
};
```

### Complexity

* **Time Complexity:** $O(n \times k)$ since we are performing `k` single iterations and each iteration requires shifting all elements of the array of size `n`.
* **Space Complexity:** $O(1)$ as no extra space is used.



## Extra Space

Instead of repeatedly shifting elements, we can directly compute the final position of each element. If an element is at index `i`, after rotation it will be at index `(i + k) % n`. By using a temporary array to store the rotated result, we can place each element in its correct position in a single pass, then copy everything back.

```c++
class Solution {
public:
    void rotate(vector<int>& nums, int k) {
        int n = nums.size();
        vector<int> tmp(n);
        for (int i = 0; i < n; i++) {
            tmp[(i + k) % n] = nums[i];
        }
        for (int i = 0; i < n; i++) {
            nums[i] = tmp[i];
        }
    }
};
```

### Complexity

* **Time Complexity:** $O(n)$ We traverse the entire array twice (two for loops).
* **Space Complexity:** $O(n)$ `tmp` array is of size `n`.


## Optimal Solution (Using Reverse)

A clever observation: rotating an array by `k` is equivalent to moving the last `k` elements to the front. We can achieve this with three reversals. First, reverse the entire array. Now the last `k` elements are at the front, but in reverse order. Reverse the first `k` elements to fix their order. Finally, reverse the remaining elements to restore their original order.

```c++
class Solution {
public:
    void rotate(vector<int>& nums, int k) {
        k = k % nums.size();
        reverse(nums.begin(), nums.end());
        reverse(nums.begin(), nums.begin() + k);
        reverse(nums.begin() + k, nums.end());
    }
};
```

### Complexity

* **Time Complexity:** $O(n)$ we reverse the entire array twice.
* **Space Complexity:** $O(1)$ no extra space is used.