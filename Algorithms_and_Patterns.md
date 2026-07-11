# Named Algorithms Glossary

A centralized registry of formal, named algorithms required for optimal solutions, their core mechanics, and the specific problems where they apply.

---

| Algorithm | Core Intuition & Invariant | Complexity | Reference Problem(s) |
| :--- | :--- | :--- | :--- |
| **Boyer-Moore Majority Vote** | Cancels out non-majority elements. Works strictly when a majority element (> N/2) is guaranteed to exist. <br><br> *Invariant:* A candidate's count drops to 0 *only* if the elements seen so far cannot determine the final majority. | Time: O(N) <br> Space: O(1) | - [5. Majority Element](./LeetCode_Top_Interview_150/1_Array_or_String/5_Majority_Element.md) |
| **Kadane's Algorithm** | Finds the maximum contiguous subarray sum by deciding whether to append the current element to the existing sum or start a new sum. <br><br> *Invariant:* The maximum sum ending at index `i` is either the element at `i` itself, or the element at `i` plus the maximum sum ending at `i-1`. | Time: O(N) <br> Space: O(1) | *To be added (e.g., Maximum Subarray)* |
| **Floyd's Cycle Detection (Tortoise & Hare)** | Detects cycles in a sequence or linked list using two pointers moving at different speeds (1 step vs 2 steps). If there is a cycle, they are mathematically guaranteed to meet. | Time: O(N) <br> Space: O(1) | *To be added (e.g., Linked List Cycle)* |
| **Knuth-Morris-Pratt (KMP)** | String matching algorithm that builds an LPS (Longest Prefix Suffix) array to bypass re-evaluating known matching characters upon a mismatch. | Time: O(N+M) <br> Space: O(M) | *To be added (e.g., Find Index in String)* |
| **Dutch National Flag (Dijkstra)** | Sorts an array of three distinct values (e.g., 0, 1, 2) in a single pass by maintaining three boundaries. | Time: O(N) <br> Space: O(1) | *To be added (e.g., Sort Colors)* |