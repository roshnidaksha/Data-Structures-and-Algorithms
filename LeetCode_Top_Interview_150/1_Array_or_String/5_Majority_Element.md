# 5. Majority Element

**Link:** [Majority Element](https://leetcode.com/problems/majority-elementy)

**Difficulty:** Easy

**Topics:** Array, Hash Table, Divide and Conquer, Sorting, Counting

## Problem Description

Given an array `nums` of size `n`, return the *majority element*.

The majority element is the element that appears more than $\lfloor \frac{n}{2} \rfloor$ times. You may assume that the majority element always exists in the array.

### Constraints:

* $n == nums.length$
* $1 \leq n \leq 5 \times 10^4$
* $-10^9 \leq nums[i] \leq 10^9$
* The input is generated such that a majority element will exist in the array.

### Example

* **Input:** nums = [3,2,3]
* **Output:** 3

## Brute Force Solution (Using Sort)

Since the majority element occurs more than $\frac{n}{2}$ times, and it is guaranteed that there exists a majority element, it will always occupy the middle position when the array is sorted. Therefore, we can sort the array and return the element at index $\lfloor \frac{n}{2} \rfloor$.

```c++
class Solution {
public:
    int majorityElement(vector<int>& nums) {
        sort(nums.begin(), nums.end());
        int n = nums.size();
        return nums[n / 2];
    }
};
```

### Complexity

* **Time Complexity:** $O(nlogn)$ as we are sorting the array.
* **Space Complexity:** $O(1)$ as no extra space is needed.


## Better Solution (Hash Map)

We can use a hash map to count the occurrences of each element in the array and then identify the element that occurs more than $\lfloor \frac{n}{2} \rfloor$ times. 

```c++
class Solution {
public:
    int majorityElement(vector<int>& nums) {
        int n = nums.size();
        unordered_map<int, int> mp;
        
        for (int i = 0; i < n; i++) {
            mp[nums[i]]++;
        }
        n = n / 2;
        for (auto x: mp) {
            if (x.second > n) {
                return x.first;
            }
        }
        return 0;
    }
};
```

### Complexity

* **Time Complexity:** $O(n)$ because we are traversing the array once to calculate the count, and then through the hashmap (has a maximum size of the number of distinct elements in the array).
* **Space Complexity:** $O(n)$ To store the counts of distinct elements in the array.


## Optimal Solution (Boyer-Moore Majority Voting Algorithm)

If an element occurs more than $\lfloor \frac{n}{2} \rfloor$ times, then all other elements combined occur less than $\lfloor \frac{n}{2} \rfloor$ times. The voting process cancels out non-majority elements, leaving the majority element as the final candidate. 

The algorithm has two phases:

1. Find a candidate: Traverse teh array while maintaining a `candidate` and a `count`. If count is 0, set the current element as the new `candidate` and `count = 1`. If the current element equals the `candidate`, increment `count`. Otherwise, decrement. By the end, `candidate` will be the potential majority element. 
2. Verify the candidate: Count the occurrences of `candidate` in teh array. If it appears more than $\lfloor \frac{n}{2} \rfloor$ times, it is the majority element. Otherwise, no majority exists. 

Since the question guarantess the presence of a majority element, phase 2 is not required for this question. 

```c++
class Solution {
public:
    int majorityElement(vector<int>& nums) {
        int n = nums.size();
        int candidate;
        int count = 0;

        // Phase 1: Find a candidate
        for (int i = 0; i < n; i++) {
            if (count == 0) {
                count = 1;
                candidate = nums[i];
            }
            else if (nums[i] == candidate) {
                count++;
            } else {
                count--;
            }
        }

        // Phase 2: Verify the candidate
        count = 0;
        for (int i = 0; i < n; i++) {
            if (nums[i] == candidate) {
                count++;
            }
        }
        if (count <= n / 2) {
            return -1;
        }

        return candidate;
    }
};
```

### Complexity

* **Time Complexity:** $O(n)$ because we traverse the array exactly twice.
* **Space Complexity:** $O(1)$ as no extra space is used.