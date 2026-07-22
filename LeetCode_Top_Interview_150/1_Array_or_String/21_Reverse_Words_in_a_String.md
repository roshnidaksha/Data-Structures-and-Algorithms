# 21. Reverse Words in a String

**Link:** [Reverse Words in a String](https://leetcode.com/problems/reverse-words-in-a-string)

**Difficulty:** Medium

**Topics:** Two Pointers, String

## Problem Description

Given an input string `s`, reverse the order of the **words**.

A **word** is defined as a sequence of non-space characters. The words in `s` will be separated by at least one space.

Return a *string of the words in reverse order concatenated by a single space*.

Note that `s` may contain leading or trailing spaces or multiple spaces between two words. The returned string should only have a single space separating the words. Do not include any extra spaces.

### Constraints:

* $1 \leq s.length \leq 104$
* `s` contains English letters (upper-case and lower-case), digits, and spaces `' '`.
* There is **at least one word** in `s`.

### Example

* **Input:** s = "the sky is blue"
* **Output:** "blue is sky the"

## Brute Force Solution

The most simple solution which directly translates the requirements would be to traverse through the array until you see a space. Once you reach a space, it indicates the end of current word. So, add the string to an array. After traversing the entire string, reverse the array, then add the strings in the array one by one separated by a space. 

This would take $O(n)$ time and $O(n)$ space to store the intermediate array of strings. 

## Optimal Solution

The above approach can be improved by using $O(1)$ extra space and directly modifying the input string. So, we first reverse the current string. Then traverse through the string similar to the brute force solution. But instead of adding the current word to an array, we just reverse it in place.

Now, the next step is to ensure that all leading and trailing spaces plus multiple spaces in between two words are correctly removed. We keep track of the point when the actual word starts and copy each character to the right position if there are leading spaces. Finally, all the extra spaces would be moved to the end. Then we can just resize the string. 

```c++
class Solution {
public:
    string reverseWords(string s) {
        int n = s.size();

        reverse(s.begin(), s.end());
        int left = 0;
        int right = 0;
        int i = 0;

        while (i < n) {
            while (i < n && s[i] == ' ') i++;
            if (i == n) break;
            while (i < n && s[i] != ' ') {
                s[right] = s[i];
                right++;
                i++;
            }

            reverse(s.begin() + left, s.begin() + right);
            s[right] = ' ';
            right++;
            left = right;
        }
        s.resize(right - 1);
        return s;
    }
};
```

### Complexity

* **Time Complexity:** $O(n)$
* **Space Complexity:** $O(1)$