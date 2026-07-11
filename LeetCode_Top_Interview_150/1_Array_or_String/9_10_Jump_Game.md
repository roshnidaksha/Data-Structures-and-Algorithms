# 9, 10 Jump Games

# Jump Game I

**Link:** [Jump Game](https://leetcode.com/problems/jump-game)

**Difficulty:** Medium

**Topics:** Array, Dynamic Programming, Greedy

## Problem Description

You are given an integer array `nums`. You are initially positioned at the array's **first index**, and each element in the array represents your maximum jump length at that position.

Return `true` *if you can reach the last index*, or `false` *otherwise*.

### Constraints:

* $1 \leq nums.length \leq 10^4$
* $0 \leq nums[i] \leq 10^5$

### Example

* **Input:** nums = [2,3,1,1,4]
* **Output:** true
* **Explanation:** Jump 1 step from index 0 to 1, then 3 steps to the last index.

## Brute Force Solution

The question asks if we can reach the last index of the array from the first index. At every position `i`, the value `nums[i]` is the maximum jump length. This means, we can jump to any index from `i + 1` and `i + nums[i]`. Using recursion, we can try all possible jumps. 

### Complexity

* **Time Complexity:** $O(n!)$ (without memoization) $O(n^2)$ (with memoization)
* **Space Complexity:** $O(n)$ recursive stack space

## Optimal Solution (Greedy)

We can keep a variable to store the farthest index reachable at any point. If this variable is $\geq n - 1$ then return true, false otherwise. 

```c++
class Solution {
public:
    bool canJump(vector<int>& nums) {
        int farthest = 0;
        for (int i = 0; i < nums.size(); i++) {
            if (i > farthest) return false;
            farthest = max(farthest, nums[i] + i);
        }
        return true;
    }
};
```

### Complexity

* **Time Complexity:** $O(n)$
* **Space Complexity:** $O(1)$

<br>

# Jump Game II

**Link:** [Jump Game II](https://leetcode.com/problems/jump-game-ii)

**Difficulty:** Medium

**Topics:** Array, Dynamic Programming, Greedy

## Problem Description

You are given an integer array `nums`. You are initially positioned at the array's **first index**, and each element in the array represents your maximum jump length at that position.

Return the *minimum number of jumps to reach* index `n - 1`. The test cases are generated such that you can reach index `n - 1`.

### Constraints:

* $1 \leq nums.length \leq 10^4$
* $0 \leq nums[i] \leq 1000$

### Example

* **Input:** nums = [2,3,1,1,4]
* **Output:** 2
* **Explanation:** The minimum number of jumps to reach the last index is 2. Jump 1 step from index 0 to 1, then 3 steps to the last index.

## Brute Force Solution

Similar to Jump Game I we can use recursion here and keep track of the steps. 

### Complexity

* **Time Complexity:** $O(n!)$ (without memoization) $O(n^2)$ (with memoization)
* **Space Complexity:** $O(n)$ recursive stack space

## Optimal Solution

The approach is similar to Jump Game I where we keep track of a `farthest` variable. In addition, we keep track of 2 more variables:
* `toJump`: signifies end of current maximum point reachable
* `jumpes`: number of jumps (answer)

```c++
class Solution {
public:
    int jump(vector<int>& nums) {
        int farthest = 0;
        int jumps = 0;
        int toJump = 0;
        for (int i = 0; i < nums.size() - 1; i++) {
            farthest = max(farthest, i + nums[i]);
            if (i == toJump) {
                toJump = farthest;
                jumps++;
            }
        }
        return jumps;
    }
};
```

### Complexity

* **Time Complexity:** $O(n)$
* **Space Complexity:** $O(1)$

<br>

# Jump Game III

**Link:** [Jump Game III](https://leetcode.com/problems/jump-game-iii)

**Difficulty:** Medium

**Topics:** Array, DFS, BFS

## Problem Description

You are given an integer array `nums`. You are initially positioned at the array's **first index**, and each element in the array represents your maximum jump length at that position. You can jump to `i + arr[i]` or `i - arr[i]`.

Check if you can reach **any** index with value 0. You cannot jump outside of the array at any time. 

### Constraints:

* $1 \leq arr.length \leq 5 \times 10^4$
* $0 \leq arr[i] < arr.length$
* $0 \leq start < arr.length$

### Example

* **Input:** arr = [4,2,3,0,3,1,2], start = 5
* **Output:** true
* **Explanation:** All possible ways to reach at index 3 with value 0 are: 
index 5 -> index 4 -> index 1 -> index 3 
index 5 -> index 6 -> index 4 -> index 1 -> index 3 

## Optimal Solution (BFS/DFS)

Imagine each index is like a graph node. Then this simplifies to a graph traversal question with early stopping criteria. We also need a `visited` array to avoid cycles/infinite loop.

```c++
class Solution {
public:
    bool canReach(vector<int>& arr, int start) {
        int n = arr.size();
        vector<bool> vis(n, false);
        queue<int> q;
        q.push(start);

        while(!q.empty()) {
            int i = q.front();
            q.pop();
            if (i < 0 || i >= n || vis[i]) continue;
            if (arr[i] == 0) {
                return true;
            } 
            vis[i] = true;
            q.push(i + arr[i]);
            q.push(i - arr[i]);
        }

        return false;
    }
};
```

### Complexity

* **Time Complexity:** $O(n)$
* **Space Complexity:** $O(n)$

<br>

# Jump Game IV

**Link:** [Jump Game IV](https://leetcode.com/problems/jump-game-iv)

**Difficulty:** Hard

**Topics:** Array, Hash Table, BFS

## Problem Description

You are given an integer array `nums`. You are initially positioned at the array's **first index**. In one step, you can jump from index `i` to index:
* `i + 1` where `i + 1 < arr.length`
* `i - 1` where `i - 1 >= 0`
* `j` where `arr[i] == arr[j]` and `i != j`

Return the *minimum number of jumps to reach* index `n - 1`. You cannot jump outside of the array at any time.

### Constraints:

* $1 \leq arr.length \leq 10^4$
* $-10^8 \leq arr[i] \leq 10^8$

### Example

* **Input:** arr = [100,-23,-23,404,100,23,23,23,3,404]
* **Output:** 3
* **Explanation:** You need three jumps from index 0 --> 4 --> 3 --> 9. Note that index 9 is the last index of the array.


## Optimal Solution (BFS)

Now, we need a way to group the indices who have the same value. For this, we can use a hashmap. To avoid creating a separate visited array, we will use this hashmap as our visited array.

```c++
class Solution {
public:
    int minJumps(vector<int>& arr) {
        int n = arr.size();
        if (n == 1) return 0;

        // mp[k] is the list of all indices whose value is k
        unordered_map<int, vector<int>> mp;
        for (int i = 0; i < n; i++) {
            mp[arr[i]].push_back(i);
        }

        int step = 0;
        queue<int> q;
        q.push(0);
        while (!q.empty()) {
            int size = q.size();
            for (int i = 0; i < size; i++) {
                int j = q.front();
                q.pop();
                if (j == n - 1) return step;
                if (j - 1 >= 0 && mp.find(arr[j - 1]) != mp.end()) {
                    q.push(j - 1);
                }
                if (j + 1 < n && mp.find(arr[j + 1]) != mp.end()) {
                    q.push(j + 1);
                }
                if (mp.find(arr[j]) != mp.end()) {
                    for (int k : mp[arr[j]]) {
                        if (k != j) {
                            q.push(k);
                        }
                    }
                }
                mp.erase(arr[j]);
            }
            step++;
        }
        return step;
    }
};
```

### Complexity

* **Time Complexity:** $O(n)$
* **Space Complexity:** $O(n)$

<br>

# Jump Game V

**Link:** [Jump Game V](https://leetcode.com/problems/jump-game-v)

**Difficulty:** Hard

**Topics:** Array, Dynamic Programming, Sorting

## Problem Description

Given an array of integers `arr` and an integer `d`. In one step you can jump from index `i` to index:
* `i + x` where: `i + x < arr.length` and  `0 < x <= d`.
* `i - x` where: `i - x >= 0` and  `0 < x <= d`.

In addition, you can only jump from index `i` to index `j` if `arr[i] > arr[j]` and `arr[i] > arr[k]` for all indices `k` between `i` and `j` (More formally `min(i, j) < k < max(i, j)`).

You can choose any index of the array and start jumping. Return the *maximum number of indices you can visit*.

Notice that you can not jump outside of the array at any time.

### Constraints:

* $1 \leq arr.length \leq 1000$
* $1 \leq arr[i] \leq 10^5$
* $1 \leq d \leq arr.length$

### Example

* **Input:** arr = [6,4,14,6,8,13,9,7,10,6,12], d = 2
* **Output:** 4
* **Explanation:** You can start at index 10. You can jump 10 --> 8 --> 6 --> 7.
Note that if you start at index 6 you can only jump to index 7. You cannot jump to index 5 because 13 > 9. You cannot jump to index 4 because index 5 is between index 4 and 6 and 13 > 9.
Similarly You cannot jump from index 3 to index 2 or index 1.

## Optimal Solution (DFS)

Let `dp[i]` represent the maximum number of indices that can be visited starting from position `i`. Then, `dp[i] = max(dp[j]) + 1` where `j` must satisfy the 2 conditions from the problem statement and be a valid index. 

For any position `i`, according to the first condition, we only need to scan at most `d` elements on both sides. To ensure all valid positions of `j` have already been processed before computing `dp[i]`, we use memoized search. Here, whenever we need `dp[j]`:
* if it has already been computed, we directly reuse the stored value
* otherwise, temporarily pause the computation of `dp[i]`, recursively compute `dp[j]` and then use the result to update `dp[i]`.

It is to note that there cannot occue a cycle in this problem due to condition 2.

```c++
class Solution {
public:
    vector<int> arr;
    int n;
    vector<int> dp;
    int d;

    void dfs(int index) {
        if (dp[index] != -1) return;
        dp[index] = 1;
        for (int i = index - 1; i >= 0 && index - i <= d && arr[index] > arr[i]; i--) {
            dfs(i);
            dp[index] = max(dp[index], dp[i] + 1);
        }
        for (int i = index + 1; i < n && i - index <= d && arr[index] > arr[i]; i++) {
            dfs(i);
            dp[index] = max(dp[index], dp[i] + 1);
        }
    }

    int maxJumps(vector<int>& arr, int d) {
        this->arr = arr;
        n = arr.size();
        dp.resize(n, -1);
        this->d = d;
        
        for (int i = 0; i < n; i++) {
            dfs(i);
        }
        return *max_element(dp.begin(), dp.end());
    }
};
```

### Complexity

* **Time Complexity:** $O(n \times d)$
* **Space Complexity:** $O(n)$

<br>

# Jump Game VI

**Link:** [Jump Game VI](https://leetcode.com/problems/jump-game-vi)

**Difficulty:** Medium

**Topics:** Array, Dynamic Programming, Queue, Heap, Monotonic Queue

## Problem Description

You are given a **0-indexed** integer array `nums` and an integer `k`.

You are initially standing at index `0`. In one move, you can jump at most `k` steps forward without going outside the boundaries of the array. That is, you can jump from index `i` to any index in the range `[i + 1, min(n - 1, i + k)]` **inclusive**.

You want to reach the last index of the array (index `n - 1`). Your score is the sum of all `nums[j]` for each index `j` you visited in the array.

Return the ***maximum score*** you can get.

### Constraints:

* $1 \leq arr.length \leq 5 \times 10^4$
* $0 \leq arr[i] < arr.length$
* $0 \leq start < arr.length$

### Example

* **Input:** nums = [*1*,*-1*,-2,*4*,-7,*3*], k = 2
* **Output:** 7
* **Explanation:** You can choose your jumps forming the subsequence [1,-1,4,3] (italics above). The sum is 7.

## Brute Force (Top-Down or Bottom-Up)

Let `dp[i]` represent the maximum score acievable to reach index `n - 1` from index `i`.

```c++
class Solution {
public:
    int maxResult(vector<int>& nums, int k) {
        int n = nums.size();
        vector<int> dp(n, INT_MIN);
        dp[n - 1] = nums[n - 1];
        for (int i = n - 1; i >= 0; i--) {
            for (int j = 1; j <= k && i + j < n; j++) {
                dp[i] = max(dp[i], dp[i + j] + nums[i]);
            }
        }
        return dp[0];
    }
};
```
### Complexity

* **Time Complexity:** $O(n \times k)$
* **Space Complexity:** $O(n)$

But this gives TLE due to the problem constraints.


## Optimal Solution (BFS/DFS)

Observing the TLE code, in the inner loop, we are always choosing the `dp[i + j]` which gives the maximum score. 
We can use a deque to efficiently insert and retrieve the maximum which runs in amortized $O(1)$.

```c++
class Solution {
public:
    int maxResult(vector<int>& nums, int k) {
        int n = nums.size();

        vector<int> dp(n, INT_MIN);
        dp[n - 1] = nums[n - 1];
        deque<int> dq;
        dq.push_back(n - 1);

        for (int i = n - 2; i >= 0; i--) {
            if (dq.front() > i + k) dq.pop_front();
            dp[i] = dp[dq.front()] + nums[i];
            while (!dq.empty() && dp[dq.back()] <= dp[i]) {
                dq.pop_back();
            }
            dq.push_back(i);
        }
        return dp[0];
    }
};
```

### Complexity

* **Time Complexity:** $O(n)$
* **Space Complexity:** $O(k)$

<br>

# Jump Game VII

**Link:** [Jump Game VII](https://leetcode.com/problems/jump-game-vii)

**Difficulty:** Medium

**Topics:** String, Dynamic Programming, Sliding Window, Prefix Sum

## Problem Description

You are given a **0-indexed** binary string `s` and two integers `minJump` and `maxJump`. In the beginning, you are standing at index `0`, which is equal to `'0'`. You can move from index `i` to index `j` if the following conditions are fulfilled:

* `i + minJump <= j <= min(i + maxJump, s.length - 1)`, and
* `s[j] == '0'`.

Return `true` *if you can reach index `s.length - 1` in `s`, or `false` otherwise*.

 
### Constraints:

* $2 \leq s.length \leq 10^5$
* $s[i] \ is \ either \ '0' \ or \ '1'$
* $s[0] == \ '0'$
* $1 \leq minJump \leq maxJump < s.length$

### Example

* **Input:** s = "011010", minJump = 2, maxJump = 3
* **Output:** true
* **Explanation:** In the first step, move from index 0 to index 3. In the second step, move from index 3 to index 5.

## Optimal Solution

Here, we use a simple sliding window technique. `dp[i]` is true if index `i` is reachable from index `0`, false otherwise. Now, variable `reachable` tracks how many indices in the range `[i - maxJump, i - minJump]` are reachable.

```c++
class Solution {
public:
    bool canReach(string s, int minJump, int maxJump) {
        int n = s.size();
        if (s[n - 1] != '0') return false;

        vector<bool> dp(n, false);
        dp[0] = true;
        int reachable = 0;
        for (int i = 1; i < n; i++) {
            if (i >= minJump && dp[i - minJump]) reachable++;
            if (i > maxJump && dp[i - maxJump - 1]) reachable--;
            if (reachable > 0 && s[i] == '0') dp[i] = true;
        }
        return dp[n - 1];
    }
};
```

### Complexity

* **Time Complexity:** $O(n)$
* **Space Complexity:** $O(n)$

<br>

# Jump Game VIII

**Link:** [Jump Game VIII](https://leetcode.com/problems/jump-game-viii)

LOCKED BY PREMIUM

<br>

# Jump Game IX

**Link:** [Jump Game IX](https://leetcode.com/problems/jump-game-ix)

**Difficulty:** Medium

**Topics:** Array, Dynamic Programming

## Problem Description

You are given an integer array `nums`.

From any index `i`, you can jump to another index `j` under the following rules:

* Jump to index `j` where `j > i` is allowed only if `nums[j] < nums[i]`.
* Jump to index `j` where `j < i` is allowed only if `nums[j] > nums[i]`.

For each index `i`, find the **maximum value** in `nums` that can be reached by following any sequence of valid jumps starting at `i`.

Return an array `ans` where `ans[i]` is the **maximum value** reachable starting from index `i`.

 
### Constraints:

* $1 \leq nums.length \leq 10^5$
* $1 \leq nums[i] \leq 10^9$

### Example

* **Input:** nums = [2,1,3]
* **Output:** [2,2,3]
* **Explanation:**
    * For i = 0: No jump increases the value.
    * For i = 1: Jump to j = 0 as nums[j] = 2 is greater than nums[i].
    * For i = 2: Since nums[2] = 3 is the maximum value in nums, no jump increases the value.
    Thus, ans = [2, 2, 3].

## Optimal (Prefix and Suffix arrays)

We can keep track of prefix max and suffix min values. The suffix minimum values are required to see if from index `i`, you can jump to any forward index and then go backward to land on a higher value. 

```c++
class Solution {
public:
    vector<int> maxValue(vector<int>& nums) {
        int n = nums.size();
        if (n == 0) return {};

        vector<int> pre_max(n);
        pre_max[0] = nums[0];
        for (int i = 1; i < n; ++i) {
            pre_max[i] = max(pre_max[i - 1], nums[i]);
        }

        vector<int> ans(n);
        ans[n - 1] = pre_max[n - 1];
        int suf_min = nums[n - 1];

        for (int i = n - 2; i >= 0; i--) {
            if (pre_max[i] > suf_min) {
                ans[i] = ans[i + 1];
            } else {
                ans[i] = pre_max[i];
            }
            suf_min = min(suf_min, nums[i]);
        }

        return ans;
    }
};
```

### Complexity

* **Time Complexity:** $O(n)$
* **Space Complexity:** $O(n)$
