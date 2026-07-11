# Best Time to Buy and Sell Stock

Questions: 7, 8


| Task 1 | Task 2 | Task 3 | Task 4 | Task 5 |
| :--- | :--- | :--- | :--- | :--- |
| [Best Time to Buy and Sell Stock](https://leetcode.com/problems/best-time-to-buy-and-sell-stock) | [Best Time to Buy and Sell Stock II](https://leetcode.com/problems/best-time-to-buy-and-sell-stock-ii) | [Best Time to Buy and Sell Stock III](https://leetcode.com/problems/best-time-to-buy-and-sell-stock-iii) | [Best Time to Buy and Sell Stock IV](https://leetcode.com/problems/best-time-to-buy-and-sell-stock-iv) | [Best Time to Buy and Sell Stock V](https://leetcode.com/problems/best-time-to-buy-and-sell-stock-v) |
| Easy | Medium | Hard | Hard | Medium |
| Array, Dynamic Programming | Array, Dynamic Programming, Greedy | Array, Dynamic Programming | Array, Dynamic Programming | Array, Dynamic Programming |

## Problem Description

Overarching Goal: Return the *maximum profit you can achieve* given an array `prices` where `prices[i]` is the price of a given stock on the `ith` day.

| Task | Max Transactions allowed | Constraints |
| :--- | :--- | :--- |
| 1 | 1 | Can perform 0 transactions and return 0 |
| 2 | $\infty$ | Cannot hold more than one share of stock at a time |
| 3 | 2 | Cannot hold more than one share of stock at a time |
| 4 | k | Cannot hold more than one share of stock at a time |
| 5 | k | Can sell first, cannot start a transaction on the same day as the previous transaction ends <br><br> **Normal transaction:** Buy on day `i`, then sell on a later day `j` where `i < j`. You profit `prices[j] - prices[i]`. <br> **Short selling transaction:** Sell on day `i`, then buy back on a later day `j` where `i < j`. You profit `prices[i] - prices[j]`. |

### Constraints:

Each Task has its own constraints that vary slightly

| Task | Constraints |
| :--- | :--- |
| 1 | $1 \leq prices.length \leq 10^5$ <br> $0 \leq prices[i] \leq 10^4$ |
| 2 | $1 \leq prices.length \leq 3 \times 10^4$ <br> $0 \leq prices[i] \leq 10^4$ |
| 3 | $1 \leq prices.length \leq 10^5$ <br> $0 \leq prices[i] \leq 10^5$ |
| 4 | $1 \leq k \leq 100$ <br> $1 \leq prices.length \leq 1000$ <br> $0 \leq prices[i] \leq 1000$ |
| 5 | $1 \leq k \leq prices.length / 2$ <br> $2 \leq prices.length \leq 1000$ <br> $1 \leq prices[i] \leq 10^9$ |


## Task 1

Since we can perform at most one transaction, the question requires us to choose a pair of prices that gives us the highest profit. 

We traverse through the prices array, and keep track of the minimum price and maximum profit received. At every price, we update these two variables.

```c++
class Solution {
public:
    int maxProfit(vector<int>& prices) {
        int mini = 1e5;
        int res = 0;
        for (int i = 0; i < prices.size(); i++) {
            mini = min(mini, prices[i]);
            res = max(res, prices[i] - mini);
        }
        return res;
    }
};
```

### Complexity

* **Time Complexity:** $O(n)$ we traverse through the array of prices once.
* **Space Complexity:** $O(1)$ only two additional variables is used for DP.


## Task 2

The key insight is that we can capture every upward price movement. If the price goes up from day `i` to day `i + 1`, we can always "buy" on day `i` and "sell" on day `i + 1` to capture that profit. We do not need to track actual transactions because consecutive gains are equivalent to holding through multiple days. So, we can apply a Greedy approach here.

```c++
class Solution {
public:
    int maxProfit(vector<int>& prices) {
        int profit = 0;
        for (int i = 1; i < prices.size(); i++) {
            if (prices[i] > prices[i - 1]) {
                profit += (prices[i] - prices[i - 1]);
            }
        }
        return profit;
    }
};
```

### Complexity

* **Time Complexity:** $O(n)$ 
* **Space Complexity:** $O(1)$


## Task 3

### Brute Force Solution

The most intuitive way to handle two transactions is to split the timeline into two distinct periods, say day `i`, and calculate the maximum profit achievable from `prices[0..i - 1]` and `prices[i..n - 1]`.

This would take $O(n^2)$ as there are `n` possibilities for splitting and each computation takes $O(n)$.

### Better Approach

To eliminate the nested loops of the brute force solution, we can precompute the maximum profits from the left and from the right and store them in arrays. 

```c++
class Solution {
public:
    int maxProfit(vector<int>& prices) {
        int n = prices.size();

        vector<int> leftProfits(n, 0);
        int mini = prices[0];
        for (int i = 1; i < prices.size(); i++) {
            mini = min(mini, prices[i]);
            leftProfits[i] = max(leftProfits[i - 1], prices[i] - mini);
        }

        vector<int> rightProfits(n + 1, 0);
        int maxi = prices[n - 1];
        for (int i = n - 2; i >= 0; i--) {
            maxi = max(maxi, prices[i]);
            rightProfits[i] = max(rightProfits[i + 1], maxi - prices[i]);
        }

        int res = 0;
        for (int i = 0; i < n; i++) {
            res = max(res, leftProfits[i] + rightProfits[i + 1]);
        }
        return res;
    }
};
```

* **Time Complexity:** $O(n)$ we make 3 separate passes through the array. 
* **Space Complexity:** $O(n)$ we allocate two extra arrays of size `n` and `n + 1` to store prefix and suffix calculations. 

### Optimal Approach

To minimise the space, we can model the process as a finite state machine. At any given day, you would be in one of the below four states:
1. **First Buy:** You purchase the first stock. You want to minimise this cost.
2. **First Sell:** You sell the first stock. You want to maximise this profit.
3. **Second Buy:** You purchase the second stock. You want to minimise the net cost, which is the current stock price minus the previous profit.
4. **Second Sell:** You sell the second stock. You want to maximise the final profit.

```c++
class Solution {
public:
    int maxProfit(vector<int>& prices) {
        int cost1 = INT_MAX;
        int profit1 = 0;
        int cost2 = INT_MAX;
        int profit2 = 0;

        for (int price : prices) {
            cost1 = min(cost1, price);
            profit1 = max(profit1, price - cost1);
            cost2 = min(cost2, price - profit1);
            profit2 = max(profit2, price - cost2);
        }

        return profit2;
    }
};
```

* **Time Complexity:** $O(n)$ we iterate through `prices` array exactly once.
* **Space Complexity:** $O(1)$ we only require 4 variables regardless of input size.


## Task 4

### Better solution (Top-Down and Bottom-Up)

In Task 3, when the limit was exactly 2, we could loop through the array to find a single split point. But now, we need `k - 1` split points, making manual looping impossible. The brute force solution is using recursion, where every single day, we have two choices - do nothing, buy / sell.

```c++
class Solution {
private:
    vector<int> prices;
    vector<vector<vector<long long>>> memo;

    long long dp(int i, int k_left, int holding) {
        // base case(s)
        if (i >= prices.size() || k_left == 0) return 0;

        if (memo[i][k_left][holding] != -1) {
            return memo[i][k_left][holding];
        }

        long long dontDo = dp(i + 1, k_left, holding);
        long long buy_sell = 0;
        if (holding == 1) {
            // Sell stock if already holding a share
            buy_sell = prices[i] + dp(i + 1, k_left - 1, false);
        } else {
            // Buy a stock
            buy_sell = dp(i + 1, k_left, true) - prices[i];
        }

        memo[i][k_left][holding] = max(buy_sell, dontDo);
        return memo[i][k_left][holding];
    };

public:
    int maxProfit(int k, vector<int>& prices) {
        this->prices = prices;
        memo = vector<vector<vector<long long>>>(
            (int)prices.size(),
            vector<vector<long long>>(k+1, vector<long long>(2, -1))
        );
        return dp(0, k, false);
    }
};
```

* **Time Complexity:** $O(n \times k)$ there are `nk` states and computation takes $O(1)$ time.
* **Space Complexity:** $O(n \times k)$ there are `2nk` states.

The above is the top-down recursive approach. You can also write a bottom-up approach as below.

Let `dp[t][i]` be the maximum profit possible on day `i` using at most `t` transactions. 
* If we do nothing: The profit remains the same. `dp[t][i] = dp[t][i-1]`
* Sell today: You must have bought the stock on some previous day `j`.
    * Profit from this transaction: `prices[i] - prices[j]`
    * Add profit from previous transactions: `dp[t - 1][j]`
    
    Since we want the maximum, we need to iterate through every possible value of `j` lesser than `i`.

```c++
for (int t = 1; t <= k; t++) {
    for (int i = 1; i < n; i++) {
        int maxi = 0;
        for (int j = 0; j < i; j++) {
            maxi = max(maxi, prices[i] - prices[j] + dp[t - i][j]);
        }
        dp[t][i] = max(dp[t][i - 1], maxi);
    }
}
```

But this is slow. Due to the inner loop, this gives $O(k \times n^2)$

Looking closely, `prices[i]` does not change, so we only need to maximise `- prices[j] + dp[t - 1][j]`. We can just keep a running track of this maximum value.

```c++
class Solution {
public:
    int maxProfit(int k, vector<int>& prices) {
        int n = prices.size();
        vector<vector<int>> dp(k + 1, vector<int>(n, 0));
        for (int t = 1; t <= k; t++) {
            int max_diff = -prices[0]; 
            for (int i = 1; i < n; i++) {
                dp[t][i] = max(dp[t][i - 1], prices[i] + max_diff);
                max_diff = max(max_diff, dp[t - 1][i] - prices[i]);
            }
        }
        return dp[k][n - 1];
    }
};
```

* **Time Complexity:** $O(n \times k)$ there two loops.
* **Space Complexity:** $O(n \times k)$ for the dp table.


### Optimal Approach (Space optimized)

If $k \geq n/2$ we can fallback to Task 2 as that is the maximum limit. This is $O(n)$ time and $O(1)$ space complexity. Or else, we generalise the optimal approach of Task 2 to length `k` by using an array. 

```c++
class Solution {
public:
    int maxProfit(int k, vector<int>& prices) {
        int n = prices.size();
        
        if (k >= n / 2) {
            // O(n) algorithm
            int profit = 0;
            for (int i = 1; i < prices.size(); i++) {
                if (prices[i] > prices[i - 1]) {
                    profit += (prices[i] - prices[i - 1]);
                }
            }
            return profit;
        }

        vector<int> cost(k + 1, INT_MAX);
        vector<int> profit(k + 1, 0);
        for (int price : prices) {
            for (int t = 1; t <= k; t++) {
                cost[t] = min(cost[t], price - profit[t - 1]);
                profit[t] = max(profit[t], price - cost[t]);
            }
        }
        return profit[k];
    }
};
```

* **Time Complexity:** $O(n \times k)$ there two loops.
* **Space Complexity:** $O(k)$ for the dp tables.


## Task 5

Now we have 3 states since we are allowed to short sell:
* 0 - Hold no shares
* 1 - Hold a share that was bought
* 2 - Hold a shorted stock

### Top-Down 3D-DP

```c++
class Solution {
private:
    vector<int> prices ;
    vector<vector<vector<long long>>> memo;
    long long dp(int i , int k_left, int state , bool can_ss) {
        // base case(s)
        if (i >= prices.size()) return 0;

        if (memo[i][k_left][state] != -1) {
            return memo[i][k_left][state];
        }

        long long dontDo = dp(i + 1, k_left, state, can_ss);
        long long buy_sell = INT_MIN;

        if (k_left > 0) {
            if (state == 1) {
                // Sell stock if already holding a share (state == 1)
                buy_sell = prices[i] + dp(i + 1, k_left - 1, 0, can_ss);
            } else if (state == 2) {
                // Buy stock that was short selled (state == 2)
                buy_sell = dp(i + 1, k_left - 1, 0, can_ss) - prices[i];
            } else {
                // Buy a stock
                buy_sell = dp(i + 1, k_left, 1, can_ss) - prices[i];
                if (can_ss) {
                    // Short sell a stock
                    long long sell = prices[i] + dp(i + 1, k_left, 2, can_ss);
                    buy_sell = max(buy_sell, sell);
                }
            }
        }

        memo[i][k_left][state] = max(buy_sell, dontDo);
        return memo[i][k_left][state];
    };

public:
    long long maximumProfit(vector<int>& prices, int k) {
        this->prices = prices;
        memo = vector<vector<vector<long long>>>(
            (int)prices.size(),
            vector<vector<long long>>(k+1, vector<long long>(3, -1))
        );
        return dp(0, k, 0, true);
    }
};
```

* `can_ss` variable was added here so that the above code can be generalised to all the 5 tasks.
* Without memoization, the time complexity would be $O(3^n)$.

* **Time Complexity:** $O(n \times k)$
* **Space Complexity:** $O(n \times k)$


### Bottom-Up:

```c++
class Solution {
public:
    long long maximumProfit(vector<int>& prices, int k) {
        int n = prices.size();
        bool can_ss = true;

        // dp[i][k_left][state]
        vector<vector<vector<long long>>> dp(
            n, 
            vector<vector<long long>>(k + 1, vector<long long>(3, INT_MIN))
        );

        // Base cases for Day 0
        for (int k_left = 0; k_left <= k; ++k_left) {
            dp[0][k_left][0] = 0; 
            if (k_left > 0) {
                dp[0][k_left][1] = -prices[0];
                if (can_ss) dp[0][k_left][2] = prices[0];
            }
        }

        // Build the table
        for (int i = 1; i < n; ++i) {
            for (int k_left = 0; k_left <= k; ++k_left) {      
                // STATE 0: NEUTRAL
                dp[i][k_left][0] = dp[i - 1][k_left][0]; // dontDo
                
                // Close a position: if we close today, yesterday we must have had k_left + 1 transactions
                if (k_left + 1 <= k) {
                    if (dp[i - 1][k_left + 1][1] != INT_MIN) 
                        dp[i][k_left][0] = max(dp[i][k_left][0], dp[i - 1][k_left + 1][1] + prices[i]);
                    
                    if (can_ss && dp[i - 1][k_left + 1][2] != INT_MIN) 
                        dp[i][k_left][0] = max(dp[i][k_left][0], dp[i - 1][k_left + 1][2] - prices[i]);
                }

                // STATE 1 & 2: LONG & SHORT
                if (k_left > 0) {
                    // Open a Long: k_left stays the same
                    dp[i][k_left][1] = dp[i - 1][k_left][1]; // dontDo
                    if (dp[i - 1][k_left][0] != INT_MIN)
                        dp[i][k_left][1] = max(dp[i][k_left][1], dp[i - 1][k_left][0] - prices[i]);

                    // Open a Short: k_left stays the same
                    if (can_ss) {
                        dp[i][k_left][2] = dp[i - 1][k_left][2]; // dontDo
                        if (dp[i - 1][k_left][0] != INT_MIN)
                            dp[i][k_left][2] = max(dp[i][k_left][2], dp[i - 1][k_left][0] + prices[i]);
                    }
                }
            }
        }
        
        // The maximum profit will be on the last day, in the neutral state, 
        // with any number of transactions left.
        long long max_profit = 0;
        for (int k_left = 0; k_left <= k; ++k_left) {
            max_profit = max(max_profit, dp[n - 1][k_left][0]);
        }
        return max_profit;
    }
};
```

* **Time Complexity:** $O(n \times k)$
* **Space Complexity:** $O(n \times k)$


### Space Optimized

Notice that in the 3D array, `dp[i]` only relies on `dp[i-1]`. We don't care about day `i-2` or day `0`. This means we can completely drop the `i` dimension to save massive amounts of memory, bringing space complexity down to just $O(k)$.

```c++
class Solution {
public:
    long long maximumProfit(vector<int>& prices, int k) {
        int n = prices.size();
        bool can_ss = true;
        const long long MIN_VAL = -1e15;

        vector<long long> neutral(k + 1, 0);
        vector<long long> long_pos(k + 1, MIN_VAL);
        vector<long long> short_pos(k + 1, MIN_VAL);
        
        // Base cases for day 0
        for (int k_left = 1; k_left <= k; ++k_left) {
            long_pos[k_left] = -prices[0];
            if (can_ss) short_pos[k_left] = prices[0];
        }
        
        for (int i = 1; i < n; ++i) {
            int price = prices[i];
            for (int k_left = 0; k_left <= k; ++k_left) {
                long long prev_neutral = neutral[k_left]; // Cache yesterday's neutral
                
                // 1. Update Neutral (closing a transaction consumes from k_left + 1)
                if (k_left + 1 <= k) {
                    if (long_pos[k_left + 1] != MIN_VAL) 
                        neutral[k_left] = max(neutral[k_left], long_pos[k_left + 1] + price);
                    
                    if (can_ss && short_pos[k_left + 1] != MIN_VAL) 
                        neutral[k_left] = max(neutral[k_left], short_pos[k_left + 1] - price);
                }
                
                // 2. Update Positions (opening a transaction uses yesterday's cached neutral)
                if (k_left > 0 && prev_neutral != MIN_VAL) {
                    long_pos[k_left] = max(long_pos[k_left], prev_neutral - price);
                    if (can_ss) 
                        short_pos[k_left] = max(short_pos[k_left], prev_neutral + price);
                }
            }
        }
        
        long long max_profit = 0;
        for (int k_left = 0; k_left <= k; ++k_left) {
            max_profit = max(max_profit, neutral[k_left]);
        }
        return max_profit;
    }
};
```

* **Time Complexity:** $O(n \times k)$
* **Space Complexity:** $O(k)$


## Bonus - With Cooldown

Link: [With Cooldown](https://leetcode.com/problems/best-time-to-buy-and-sell-stock-with-cooldown/)

Difficulty: Medium

In this version, you cannot short sell and can perform as many transactions as possible. But, after you sell your stock, you cannot buy stock on the next day (i.e., cooldown one day).

### Top-Down

Since we can do any number of transactions, we don't need the `k_left` variable. We now have 3 distinct states:
* State 0 (Neutral): You are not holding a stock and you are free to buy.
* State 1 (Holding): You are holding a stock. You can hold it or sell it.
* State 2 (Cooldown): You just sold a stock yesterday. You are forced to do nothing today.

```c++
class Solution {
private:
    vector<int> prices ;
    vector<vector<int>> memo;
    int dp(int i, int state) {
        // base case(s)
        if (i >= prices.size()) return 0;

        if (memo[i][state] != -1) {
            return memo[i][state];
        }

        long long dontDo = 0;
        long long buy_sell = 0;

        if (state == 0) {
            // Neutral: we can either buy or do nothing
            dontDo = dp(i + 1, 0);
            buy_sell = dp(i + 1, 1) - prices[i];
        } else if (state == 1) {
            // Holding a share
            dontDo = dp(i + 1, 1);
            buy_sell = dp(i + 1, 2) + prices[i];
        } else if (state == 2) {
            // Cooldown
            dontDo = dp(i + 1, 0);
            buy_sell = INT_MIN;
        }

        memo[i][state] = max(dontDo, buy_sell);
        return memo[i][state];
    };

public:
    int maxProfit(vector<int>& prices) {
        this->prices = prices;
        memo = vector<vector<int>>(prices.size(), vector<int>(3, -1));
        return dp(0, 0);
    }
};
```

* **Time Complexity:** $O(n)$
* **Space Complexity:** $O(n)$ for memo table and recursive stack trace

### Bottom-Up

```c++
class Solution {
public:
    int maxProfit(vector<int>& prices) {
        int n = prices.size();
        vector<vector<int>> dp(n, vector<int>(3, 0));

        dp[0][0] = 0;
        dp[0][1] = -prices[0];
        dp[0][2] = INT_MIN;

        for (int i = 1; i < n; i++) {
            dp[i][0] = max(dp[i - 1][0], dp[i - 1][2]);
            dp[i][1] = max(dp[i - 1][1], dp[i - 1][0] - prices[i]);
            if (dp[i - 1][1] != INT_MIN) {
                dp[i][2] = dp[i - 1][1] + prices[i];
            } else {
                dp[i][2] = INT_MIN;
            }
        }

        return max(dp[n - 1][0], dp[n - 1][2]);
    }
};
```

* **Time Complexity:** $O(n)$
* **Space Complexity:** $O(n)$ 

### Space Optimized

Day `i` only refers to day `i - 1`. So we only need to store the previous day variables.

```c++
class Solution {
public:
    int maxProfit(vector<int>& prices) {
        int n = prices.size();
        
        int neutral = 0;
        int holding = - prices[0];
        int cooldown = INT_MIN;

        for (int i = 1; i < n; i++) {
            int price = prices[i];

            int prev_neutral = neutral;
            int prev_holding = holding;
            int prev_cooldown = cooldown;

            neutral = max(prev_neutral, prev_cooldown);
            holding = max(prev_holding, prev_neutral - price);

            if (prev_holding != INT_MIN) {
                cooldown = prev_holding + price;
            } else {
                cooldown = INT_MIN;
            }
        }

        return max(neutral, cooldown);
    }
};
```

* **Time Complexity:** $O(n)$
* **Space Complexity:** $O(1)$ 

