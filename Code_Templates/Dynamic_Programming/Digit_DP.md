# Digit Dynamic Programming (Digit DP)

**Tags:** `#dynamic-programming`, `#math`, `#strings`

**Prerequisites:** Recursion, Memoization, Combinatorics

## 💡 The Core Concept
Digit DP is a specific pattern used to count the number of integers within a range $[L, R]$ that satisfy a specific property related to their individual digits (e.g., "how many numbers have a sum of digits equal to $K$"). Instead of iterating through all numbers linearly, which is too slow for large ranges, Digit DP builds the valid numbers digit-by-digit as a string.

**Use Case:** When you need to count integers satisfying digit-based constraints in a massive range (often up to $10^{18}$), meaning linear $O(N)$ traversal would cause a Time Limit Exceeded (TLE) error.

## ⏱️ Complexity
* **Time Complexity:** $O(N \times S \times 10)$ where $N$ is the number of digits (string length) and $S$ is the number of custom states. Since $N$ is typically at most 18 (for a 64-bit integer), this runs in near $O(1)$ time.
* **Space Complexity:** $O(N \times S)$ to store the memoization table.



## 💻 C++ Implementation
*Optimized for competitive constraints. Uses a multi-dimensional array or `vector` for fast memoization.*

```cpp
#include <iostream>
#include <string>
#include <vector>
#include <cstring>

using namespace std;

class DigitDP {
private:
    // Dimensions: [index][is_tight][is_leading_zero]... + [custom state]
    int memo[20][2][2]; 

    int dp(const string& s, int idx, bool tight, bool is_leading_zero /*, int custom_state */) {
        // Base Case: We have successfully built a valid number
        if (idx == s.length()) {
            return 1; // Modify based on what constitutes a "valid" state
        }
        
        // Return cached result if already computed
        if (memo[idx][tight][is_leading_zero] != -1) {
            return memo[idx][tight][is_leading_zero];
        }

        int limit = tight ? (s[idx] - '0') : 9;
        int ans = 0;

        for (int digit = 0; digit <= limit; digit++) {
            bool new_tight = tight && (digit == limit);
            bool new_leading_zero = is_leading_zero && (digit == 0);
            
            // Add custom constraints here (e.g., skipping certain digits)
            
            ans += dp(s, idx + 1, new_tight, new_leading_zero /*, updated_custom_state */);
        }

        return memo[idx][tight][is_leading_zero] = ans;
    }

public:
    int countValidNumbers(int R) {
        string s = to_string(R);
        memset(memo, -1, sizeof(memo));
        return dp(s, 0, true, true);
    }
    
    // To solve for range [L, R], compute solve(R) - solve(L - 1)
    int solveRange(int L, int R) {
        return countValidNumbers(R) - countValidNumbers(L - 1);
    }
};
```

## 💻 Python Implementation
*Optimized for readability. Uses the built-in @cache decorator, entirely eliminating the need to manually manage memoization tables.*

```Python
from functools import cache

class DigitDP:
    def solveRange(self, L: int, R: int) -> int:
        def countValidNumbers(num: int) -> int:
            s = str(num)
            
            @cache
            def dp(idx: int, tight: bool, is_leading_zero: bool, custom_state: int = 0) -> int:
                # Base Case: We have successfully built a valid number
                if idx == len(s):
                    return 1 # Modify based on what constitutes a "valid" state
                
                limit = int(s[idx]) if tight else 9
                ans = 0
                
                for digit in range(limit + 1):
                    new_tight = tight and (digit == limit)
                    new_leading_zero = is_leading_zero and (digit == 0)
                    
                    # Add custom constraints here (e.g., skipping certain digits)
                    
                    ans += dp(idx + 1, new_tight, new_leading_zero, custom_state)
                    
                return ans
                
            return dp(0, True, True)
            
        return countValidNumbers(R) - countValidNumbers(L - 1)
```

## ⚠️ Common Pitfalls & Edge Cases

* **Solving Ranges:** Almost all Digit DP problems ask for answers in a range $[L, R]$. The standard approach is to calculate `solve(R) - solve(L - 1)`.

* **The `tight` variable:** This boolean ensures we don't build numbers larger than our upper bound. If we are trying to count up to 345 and we place a 3 in the first position, tight remains true, meaning the next digit cannot exceed 4. If we place a 2, tight becomes false, and the next digit can be anything up to 9.

* **The `is_leading_zero` variable:** Crucial for problems where the absolute length of the number matters. For example, if you are counting occurrences of the digit '0', leading zeros do not count towards the total.

* **Negative Numbers in Range:** If $L = 0$, calling `solve(L - 1)` will pass `-1` to the function. Always handle the $L = 0$ edge case manually to avoid negative string conversions.

## 🎯 Practice Problems (LeetCode)

Problem | Difficulty | Notes | 
|-------|-------|-------|
[Number of Digit One](https://leetcode.com/problems/number-of-digit-one/description) | Hard | Classic introductory problem. The custom state tracks the count of `'1's` seen so far.  |
[Numbers At Most N Given Digit Set](https://leetcode.com/problems/numbers-at-most-n-given-digit-set/description) | Hard | Requires modifying the inner loop to only iterate through the allowed digit set.
[Count Special Integers](https://leetcode.com/problems/count-special-integers/description/) | Hard | Use a bitmask as your custom state to keep track of which digits have already been used. |

