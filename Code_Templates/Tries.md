# Trie (Prefix Tree)

**Tags:** `#data-structures`, `#strings`, `#trees`

**Prerequisites:** Trees, Hash Maps / Arrays, Strings

## 💡 The Core Concept
A tree-like data structure used to efficiently store and retrieve keys in a dataset of strings. Each node represents a single character, and traversing down a path spells out a word or a prefix. Nodes share common prefixes, making it highly efficient for overlapping words.

**Use Case:** When you need to do fast prefix matching, build autocomplete features, or efficiently search a dictionary of words.

## ⏱️ Complexity
* **Time Complexity:** 
  * Insert: $O(L)$, where $L$ is the length of the word.
  * Search: $O(L)$
  * StartsWith (Prefix): $O(L)$
* **Space Complexity:** $O(N \times L \times Σ)$, where $N$ is the number of words, $L$ is the average length, and $Σ$ is the alphabet size. In the worst case (no shared prefixes), every character gets its own node.



## 💻 C++ Implementation
*Optimized for competitive programming speed. Uses an array for children, assuming a fixed lowercase English alphabet.*

```cpp
#include <string>
#include <vector>

using namespace std;

class TrieNode {
public:
    TrieNode* children[26];
    bool isEndOfWord;

    TrieNode() {
        isEndOfWord = false;
        for (int i = 0; i < 26; i++) {
            children[i] = nullptr;
        }
    }
};

class Trie {
private:
    TrieNode* root;
public:
    Trie() {
        root = new TrieNode();
    }
    
    void insert(string word) {
        TrieNode* node = root;
        for (char c : word) {
            int index = c - 'a';
            if (node->children[index] == nullptr) {
                node->children[index] = new TrieNode();
            }
            node = node->children[index];
        }
        node->isEndOfWord = true;
    }
    
    bool search(string word) {
        TrieNode* node = root;
        for (char c : word) {
            int index = c - 'a';
            if (node->children[index] == nullptr) {
                return false;
            }
            node = node->children[index];
        }
        return node->isEndOfWord; // Must be the end of an inserted word
    }
    
    bool startsWith(string prefix) {
        TrieNode* node = root;
        for (char c : prefix) {
            int index = c - 'a';
            if (node->children[index] == nullptr) {
                return false;
            }
            node = node->children[index];
        }
        return true; // We successfully traversed the whole prefix
    }
};
```

## 💻 Python Implementation
*Optimized for readability and flexibility. Uses a hash map (dict), which gracefully handles any character set (Unicode, uppercase, etc.) without wasting space.*

```Python
class TrieNode:
    def __init__(self):
        self.children = {}
        self.is_end_of_word = False

class Trie:
    def __init__(self):
        self.root = TrieNode()

    def insert(self, word: str) -> None:
        node = self.root
        for char in word:
            if char not in node.children:
                node.children[char] = TrieNode()
            node = node.children[char]
        node.is_end_of_word = True

    def search(self, word: str) -> bool:
        node = self.root
        for char in word:
            if char not in node.children:
                return False
            node = node.children[char]
        return node.is_end_of_word

    def starts_with(self, prefix: str) -> bool:
        node = self.root
        for char in prefix:
            if char not in node.children:
                return False
            node = node.children[char]
        return True
```

## ⚠️ Common Pitfalls & Edge Cases

* **Alphabet Size Limits:** The C++ boilerplate above assumes lowercase English letters (`c - 'a'`). If the problem includes uppercase, numbers, or symbols, you must either expand the array size (e.g., to 128 for ASCII) or switch to a hash map (`unordered_map<char, TrieNode*>`).  

* **Memory Limit Exceeded (MLE):** Allocating fixed-size arrays of 26 (or 128) pointers for every single node can waste a massive amount of memory if the tree is sparse. Hash maps save space but add a slight time overhead.  

* **Memory Leaks (C++):** In strict production environments, you need a destructor to traverse and delete all dynamically allocated TrieNodes. In competitive programming, memory leaks upon program exit are generally ignored.  

* **`search` vs `startsWith`:** A common bug is returning `True` in `search` just because the loop finished. You must return `node.is_end_of_word` to guarantee the sequence was inserted as a full word, not just as a prefix to a longer word.

## 🎯 Practice Problems (LeetCode)

Problem | Difficulty | Notes | 
|-------|-------|-------|
[Implement Trie (Prefix Tree)](https://leetcode.com/problems/implement-trie-prefix-tree/description/) | Medium | Implementation of a Trie and basic understanding  |
[Words Within Two Edits of Dictionary](https://leetcode.com/problems/words-within-two-edits-of-dictionary/description) | Medium | DFS + Trie
[Stream of Characters](https://leetcode.com/problems/stream-of-characters/description) | Hard | Given a stream of characters, check if a suffix of any of these words is a valid word. |
[Word Search](https://leetcode.com/problems/word-search-ii/description/) | Hard | Use Trie efficiently to search for words faster. |
[Longest Common Suffix Queries](https://leetcode.com/problems/longest-common-suffix-queries) | Hard | Memory efficient way of implementing Tries by using arrays instead of pointers. |
