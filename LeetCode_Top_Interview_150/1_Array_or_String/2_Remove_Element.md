# 2. Remove Element

**Link:** [Remove Element](https://leetcode.com/problems/remove-element)

**Difficulty:** Easy

**Topics:** Array, Two Pointers

## Problem Description

Given an integer array `nums` and an integer `val`, remove all occurrences of `val` in `nums` **in-place**. The order of the elements may be changed. Then return *the number of elements* in `nums` *which are not equal* to `val`.

Consider the number of elements in `nums` which are not equal to `val` be `k`, to get accepted, you need to do the following things:

* Change the array nums such that the first `k` elements of `nums` contain the elements which are not equal to `val`. The remaining elements of `nums` are not important as well as the size of `nums`.
* Return `k`.

**Custom Judge:**

The judge will test your solution with the following code:

```
int[] nums = [...]; // Input array
int val = ...; // Value to remove
int[] expectedNums = [...]; // The expected answer with correct length.
                            // It is sorted with no values equaling val.

int k = removeElement(nums, val); // Calls your implementation

assert k == expectedNums.length;
sort(nums, 0, k); // Sort the first k elements of nums
for (int i = 0; i < actualLength; i++) {
    assert nums[i] == expectedNums[i];
}
```

If all assertions pass, then your solution will be **accepted**.

### Constraints:

* $0 \leq nums.length \leq 100$
* $0 \leq nums[i] \leq 50$
* $0 \leq val \leq 100$

### Example

* Input: nums = [3,2,2,3], val = 3
* Output: 2, nums = [2,2,_,_]
* Explanation: Your function should return k = 2, with the first two elements of nums being 2. It does not matter what you leave beyond the returned k (hence they are underscores).

## Optimal Solution (Two Pointers)

We need to iterate through the array and keep track of two pointers: `insertPos` and `i`. The `insertPos` pointer represents the position where the next non-val element should be placed, while the `i` pointer iterates through all the array elements. Through this, we effectively re-write the first necessary elements of `nums`.

```c++
class Solution {
public:
    int removeElement(vector<int>& nums, int val) {
        int insertPos = 0;
        for (int i = 0; i < nums.size(); i++) {
            if (nums[i] != val) {
                nums[insertPos] = nums[i];
                insertPos++;
            }
        }
        return insertPos;
    }
};
```

### Complexity

* **Time Complexity:** $O(n)$ because we process each element exactly once.
* **Space Complexity:** $O(1)$ because we are modifying `nums` in place.

## Optinal Solution 2 (Memory-Write Optimized)

The standard two-pointer approach writes every valid element to a new index. While the time complexity is $O(n)$, this incurs unnecessary memory write operations if the elements to remove are rare. To optimize performance, we can place pointers at the start and the active end of the array. When we encounter `val` at the `start` pointer, we overwrite it with the element at the `end` and shrink the array's effective size. We do not advance `start` immediately, as the newly swapped element must also be evaluated. This ensures write operations *only* occur when an element actually needs to be removed.

Since the custome judge does not check relative order in the original array, it is ok if during the swap, certain element's order is not maintained. 

**Loop Invariant:** 
At the start of every iteration, `end` accurately reflects the current valid length of the array, and the subarray `nums[0...start - 1]` is guaranteed to contain no instances of `val`.

```c++
class Solution {
public:
    int removeElement(vector<int>& nums, int val) {
        int start = 0;
        int end = nums.size();
        while (start < end) {
            if (nums[start] == val) {
                nums[start] = nums[end - 1];;
                end--;
            } else {
                start++;
            }
        }
        return end;
    }
};
```

### Complexity

* **Time Complexity:** $O(n)$ - Both pointers traverse at most $n$ steps combined.
* **Space Complexity:** $O(1)$ - Modification is strictly in-place.