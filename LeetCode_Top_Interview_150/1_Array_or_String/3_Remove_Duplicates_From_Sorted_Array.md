# 3. Remove Duplicates from Sorted Array

**Link:** [Remove Duplicates from Sorted Array](https://leetcode.com/problems/remove-duplicates-from-sorted-array)

**Difficulty:** Easy

**Topics:** Array, Two Pointers

## Problem Description

Given an integer array `nums` sorted in **non-decreasing** order, remove the duplicates **in-place** such that each unique element appears only **once**. The relative order of the elements should be kept the **same**.

Consider the number of *unique elements* in `nums` to be `k​​​​​​​​​​​​​`​. After removing duplicates, return the number of unique elements `k`.

The first `k` elements of nums should contain the unique numbers in **sorted order**. The remaining elements beyond index `k - 1` can be ignored.

**Custom Judge:**

The judge will test your solution with the following code:

```
int[] nums = [...]; // Input array
int[] expectedNums = [...]; // The expected answer with correct length

int k = removeDuplicates(nums); // Calls your implementation

assert k == expectedNums.length;
for (int i = 0; i < k; i++) {
    assert nums[i] == expectedNums[i];
}
```
If all assertions pass, then your solution will be **accepted**.

### Constraints:

* $1 \leq nums.length \leq 3 \times 10^4$
* $-100 \leq nums[i] \leq 100$
* nums is sorted in non-decreasing order.

### Example

* **Input:** nums = [1,1,2]
* **Output:** 2, nums = [1,2,_]
* **Explanation:** Your function should return k = 2, with the first two elements of nums being 1 and 2 respectively.
It does not matter what you leave beyond the returned k (hence they are underscores).

## Optimal Solution (Two Pointers)

Similar to the previous question, remove element, we use two pointers. One to denote the current position, and another that points to the location where insertion should take place. Since the array is already sorted, all duplicate elements are guaranteed to be adjacent.

```c++
class Solution {
public:
    int removeDuplicates(vector<int>& nums) {
        int insertPos = 0;
        int n = nums.size();
        for (int i = 1; i < n; i++) {
            if (nums[insertPos] != nums[i]) {
                insertPos++;
                nums[insertPos] = nums[i];
            }
        }
        return insertPos + 1;
    }
};
```

**Loop Invariant:** At the start of every iteration of the loop, the subarray `nums[0...insertPos]` contains only the unique elements seen so far, maintaining their original sorted order.

### Complexity

* **Time Complexity:** $O(n)$ - Both pointers traverse at most $n$ steps combined.
* **Space Complexity:** $O(1)$ - Modification is strictly in-place.