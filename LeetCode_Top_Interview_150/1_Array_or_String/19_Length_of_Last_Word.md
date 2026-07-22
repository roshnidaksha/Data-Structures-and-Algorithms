# 19. Length of Last Word

**Link:** [Length of Last Word](https://leetcode.com/problems/length-of-last-word)

**Difficulty:** Easy

**Topics:** String

## Problem Description

Given a string `s` consisting of words and spaces, return the **length of the last word** in the string.

A word is a maximal substring consisting of non-space characters only.

### Constraints:

* $1 \leq s.length \leq 104$
* `s` consists of only English letters and spaces `' '`.
* There will be at least one word in `s`.

### Example

* **Input:** s = "Hello World"
* **Output:** 5
* **Explanation:** The last word is "World" with length 5.

## Optimal Solution

The optimal solution would be a one pass traversal through the string. To avoid unnecessary traversal of the entire string, we traverse the given string backwards. Since there might be leading spaces which are counted as a valid word, we need to handle that case as well. 

```c++
class Solution {
public:
    int lengthOfLastWord(string s) {
        int count = 0;
        int started = false;
        for (int i = s.size() - 1; i >= 0; i--) {
            if (s[i] == ' ' && started) break;
            if (s[i] != ' ') {
                started = true;
                count++;
            }
        }
        return count;
    }
};
```

### Complexity

* **Time Complexity:** $O(n)$
* **Space Complexity:** $O(1)$