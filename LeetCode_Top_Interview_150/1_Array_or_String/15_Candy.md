# 15. Candy

**Link:** [Candy](https://leetcode.com/problems/candy)

**Difficulty:** Hard

**Topics:** Array, Greedy

## Problem Description

There are `n` children standing in a line. Each child is assigned a rating value given in the integer array `ratings`.

You are giving candies to these children subjected to the following requirements:
* Each child must have at least one candy.
* Children with a higher rating get more candies than their neighbors.

Return *the minimum number of candies you need to have to distribute the candies to the children*.

### Constraints:

* $n == ratings.length$
* $1 \leq n \leq 2 \times 10^4$
* $0 \leq ratings[i] \leq 2 \times 10^4$

### Example

* **Input:** 
* **Output:** 
* **Explanation:**

## Optimal Solution

First initialise the final array with 1 as every person needs to get at least one candy.

Use a two-pass solution. In the first pass, traverse the array in the forward direction and if the current rating is greater than the previous rating, increment the candies by 1.

In the second pass, we traverse backward to check and increment the number of candies if the current rating is greater than the rating on their right. 

```c++
class Solution {
public:
    int candy(vector<int>& ratings) {
        int n = ratings.size();
        if (n <= 1) return n;

        vector<int> candy(n, 1);
        for (int i = 1; i < n; i++) {
            if (ratings[i] > ratings[i - 1]) {
                candy[i] = candy[i - 1] + 1;
            }
        }

        for (int i = n - 2; i >= 0; i--) {
            if (ratings[i] > ratings[i + 1]) {
                candy[i] = max(candy[i], candy[i + 1] + 1);
            }
        }

        return accumulate(candy.begin(), candy.end(), 0);
    }
};
```

### Complexity

* **Time Complexity:** $O(n)$
* **Space Complexity:** $O(n)$ for storing internediate candy count results.

## Optimal Solution (Space Optimized)

The space can be further optimized by keeping track of the current upward and downward streak by using variables.  

```c++
class Solution {
public:
    int candy(vector<int>& ratings) {
        int ans = ratings.size();
        int up = 0;
        int down = 0;
        int peak = 0;
        for (int i = 1; i < ratings.size(); i++) {
            if (ratings[i] > ratings[i - 1]) {
                down = 0;
                up++;
                peak = up;
                ans += up;
            } else if (ratings[i] < ratings[i - 1]) {
                up = 0;
                down++;
                ans += (peak < down) ? down : down - 1;
            } else {
                up = down = peak = 0;
            }
        }
        return ans;
    }
};
```

### Complexity

* **Time Complexity:** $O(n)$
* **Space Complexity:** $O(1)$