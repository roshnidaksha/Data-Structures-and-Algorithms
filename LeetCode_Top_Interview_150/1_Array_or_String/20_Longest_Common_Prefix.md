# 20. Longest Common Prefix

**Link:** [Longest Common Prefix](https://leetcode.com/problems/longest-common-prefix)

**Difficulty:** Easy

**Topics:** Array, String, Trie

## Problem Description

Write a function to find the **longest common prefix** string amongst an array of strings.

If there is no common prefix, return an empty string `""`.

### Constraints:

* $1 \leq strs.length \leq 200$
* $0 \leq strs[i].length \leq 200$
* `strs[i]` consists of only lowercase English letters if it is non-empty.

### Example

* **Input:** strs = ["flower","flow","flight"]
* **Output:** "fl"

## Horizontal Scanning

To find the prefix of all strings, we can use a horizontal scanning approach. First we find the prefix of the first two strings. Then find the prefix of the third string and the previous prefix and so on. You can eventually find the prefix of all the strings after traversing the entire array. 

```c++
class Solution {
public:
    string longestCommonPrefix(vector<string>& strs) {
        if (strs.size() == 0) return "";
        string prefix = strs[0];
        for (int i = 1; i < strs.size(); i++) {
            while (strs[i].find(prefix) != 0) {
                prefix = prefix.substr(0, prefix.size() - 1);
                if (prefix.empty()) return "";
            }
        }
        return prefix;
    }
};
```

### Complexity

* **Time Complexity:** $O(S)$ where `S` is the sum of all characters in all strings.
* **Space Complexity:** $O(1)$

## Vertical Scanning

A better way would be to iterate through indices one by one and check if every string in the word has the same character at that position. We stop when there is at least one string that does not match. 

```c++
class Solution {
public:
    string longestCommonPrefix(vector<string>& strs) {
        if (strs.size() == 0) return "";
        for (int i = 0; i < strs[0].size(); i++) {
            char c = strs[0][i];
            for (int j = 1; j < strs.size(); j++) {
                if (i == strs[j].size() || strs[j][i] != c)
                    return strs[0].substr(0, i);
            }
        }
        return strs[0];
    }
};
```

To improve, instead of iterating through the characters of `strs[0]` blindly, we can find the min length string. Although this does not improve the worst case time complexity, the best case time is improved. 

### Complexity

* **Time Complexity:** $O(S)$ where `S` is the sum of all characters in all strings. But this is better than the Horizontal Scanning approach as in the best case, it would only take $O(n \times minLen)$ 
* **Space Complexity:** $O(1)$

