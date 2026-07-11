# 3. Gas Station

**Link:** [Gas Station](https://leetcode.com/problems/gas-station)

**Difficulty:** Medium

**Topics:** Array, Greedy

## Problem Description

There are `n` gas stations along a circular route, where the amount of gas at the `ith` station is `gas[i]`.

You have a car with an unlimited gas tank and it costs `cost[i]` of gas to travel from the `ith` station to its next `(i + 1)th` station. You begin the journey with an empty tank at one of the gas stations.

Given two integer arrays `gas` and `cost`, return the starting *gas station's index if you can travel around the circuit once in the clockwise direction, otherwise return `-1`*. If there exists a solution, it is **guaranteed to be unique**.

### Constraints:

* $n == gas.length == cost.length$
* $1 \leq n \leq 10^5$
* $0 \leq gas[i], \ cost[i] \leq 10^4$
* The input is generated such that the answer is unique.

### Example

* **Input:** gas = [1,2,3,4,5], cost = [3,4,5,1,2]
* **Output:** 3
* **Explanation:** Start at station 3 (index 3) and fill up with 4 unit of gas. Your tank = 0 + 4 = 4 <br>
Travel to station 4. Your tank = 4 - 1 + 5 = 8 <br>
Travel to station 0. Your tank = 8 - 2 + 1 = 7 <br>
Travel to station 1. Your tank = 7 - 3 + 2 = 6 <br>
Travel to station 2. Your tank = 6 - 4 + 3 = 5 <br>
Travel to station 3. The cost is 5. Your gas is just enough to travel back to station 3. <br>
Therefore, return 3 as the starting index.


## Brute Force Solution

The brute force solution would be to try the simulation from every gas station. This would take $O(n^2)$ leading to TLE. 

## Optimal Solution (Two Pointers)

The optimal solution is obtained by keen observation.
* If the total amount of gas is less than the total cost, then completing the circuit is impossible.
* Since the solution is guaranteed, we can use a greedy approach. 

1. Keep track of two variables - `currentGas` and `start`. 
1. Traverse every station and update currentGas.
    * If the `currentGas` goes below `0`, it emans that we cannot reach the next station from the current starting point. 
    * Greedy observation: If starting from `start` fails, then starting from any index between `start` and `i` would also fail as the total gas accumulated is insufficient. So, update `start` to `i + 1`.


```c++
class Solution {
public:
    int canCompleteCircuit(vector<int>& gas, vector<int>& cost) {
        int diff = 0;
        int currentGas = 0;
        int start = 0;
        for (int i = 0; i < gas.size(); i++) {
            diff += gas[i] - cost[i];
            currentGas += gas[i] - cost[i];
            if (currentGas < 0) {
                currentGas = 0;
                start = i + 1;
            }
        }
        if (diff < 0) return -1;
        return start;
    }
};
```

### Complexity

* **Time Complexity:** $O(n)$
* **Space Complexity:** $O(1)$