# 🏆 [O(N) Time] Clear Explanation | Intuition + Approach + Code

### 🧠 Concepts Used
> Array, Two Pointers, HashMap (Replace with actual concepts)


## 💡 Intuition
When first reading the problem, the brute-force approach takes $\mathcal{O}(N^2)$ time. However, noticing that we need to find pairs efficiently, a **HashMap** (or Two Pointers) immediately comes to mind to reduce the search time.

## ⚙️ Approach
1. **Initialize** a hash map to store the values we've seen so far and their indices.
2. **Iterate** through the array. For each element `num`...
3. **Check** if the condition is met.
    - If it is, update the result.
    - If not, continue the process.
4. **Edge Cases Handled:** Mention if you accounted for empty arrays, negative numbers, etc.


## 📊 Complexity
- **Time complexity:** $\mathcal{O}(N)$
  We traverse the list containing $N$ elements exactly once. Each lookup and insertion takes $\mathcal{O}(1)$ time on average.

- **Space complexity:** $\mathcal{O}(N)$
  In the worst-case scenario, we might need to store $N-1$ elements in our data structure, requiring extra space proportional to the input size.

---

## 💻 Code
```python
class Solution:
    def yourMethodName(self, nums: List[int], target: int) -> int:
        # Step 1: Initialize your variables
        
        # Step 2: Iterate through the input
        for i, num in enumerate(nums):
            pass # Your logic here
            
        # Step 3: Return the result
        return 0