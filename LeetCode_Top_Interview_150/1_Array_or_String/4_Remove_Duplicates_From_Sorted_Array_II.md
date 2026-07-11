# 4. Remove Duplicates from Sorted Array II

**Link:** [Remove Duplicates from Sorted Array II](https://leetcode.com/problems/remove-duplicates-from-sorted-array-ii)

**Difficulty:** Medium

**Topics:** Array, Two Pointers

## Problem Description

Given an integer array `nums` sorted in **non-decreasing** order, remove the duplicates **in-place** such that each unique element appears **at most twice**. The relative order of the elements should be kept the **same**.

Since it is impossible to change the length of the array in some languages, you must instead have the result be placed in the **first part** of the array `nums`. More formally, if there are `k` elements after removing the duplicates, then the first ``k`` elements of `nums` should hold the final result. It does not matter what you leave beyond the first `k` elements.

Return `k` after placing the final result in the first `k` slots of `nums`.

Do **not** allocate extra space for another array. You must do this by **modifying the input array in-place** with $O(1)$ extra memory.

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
* $-10^4 \leq nums[i] \leq 10^4$
* nums is sorted in non-decreasing order.

### Example

* **Input:** nums = [1,1,1,2,2,3]
* **Output:** 5, nums = [1,1,2,2,3,_]
* **Explanation:** Your function should return k = 5, with the first five elements of nums being 1, 1, 2, 2 and 3 respectively.
It does not matter what you leave beyond the returned k (hence they are underscores).

## Optimal Solution (Two Pointers)

This is an improvement on the previous question. Here, instead of having each unique element appear only once, it can happen at most twice. So, the main change is to check the element 2 steps before instead of the before element. Since the array is sorted, if `nums[i] == nums[insertPos - 2]`, it implies that the element at `insertPos - 1` is also the same value. Adding `nums[i]` would create an invalid triplet, so we skip it. 

```c++
class Solution {
public:
    int removeDuplicates(vector<int>& nums) {
        int insertPos = 0;
        for (int i = 0; i < nums.size(); i++) {
            if (insertPos < 2 || nums[i] != nums[insertPos - 2]) {
                nums[insertPos] = nums[i];
                insertPos++;
            }
        }
        return insertPos;
    }
};
```

**Loop Invariant:** At the start of every iteration of the loop, the subarray `nums[0...insertPos]` contains the processed elements in sorted order, with no element appearing more than twice.

### Complexity

* **Time Complexity:** $O(n)$ - Both pointers traverse at most $n$ steps combined.
* **Space Complexity:** $O(1)$ - Modification is strictly in-place.