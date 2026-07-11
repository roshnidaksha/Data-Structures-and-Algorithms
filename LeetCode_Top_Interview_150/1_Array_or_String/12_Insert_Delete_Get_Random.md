# 12. Insert Delete GetRandom O(1)

**Link:** [Insert Delete GetRandom O(1)](https://leetcode.com/problems/insert-delete-getrandom-o1)

**Difficulty:** Medium

**Topics:** Array, Hash Table, Math, Design, Randomized

## Problem Description

Implement the `RandomizedSet` class:

* `RandomizedSet()` Initializes the `RandomizedSet` object.
* `bool insert(int val)` Inserts an item `val` into the set if not present. Returns `true` if the item was not present, `false` otherwise.
* `bool remove(int val)` Removes an item `val` from the set if present. Returns `true` if the item was present, `false` otherwise.
* `int getRandom()` Returns a random element from the current set of elements (it's guaranteed that at least one element exists when this method is called). Each element must have the same probability of being returned.

You must implement the functions of the class such that each function works in **average** `O(1)` time complexity.

### Constraints:

* $-2^{31} \leq val \leq 2^{31} - 1$
* At most $2 \times 10^5$ calls will be made to insert, remove, and getRandom.
* There will be at least one element in the data structure when getRandom is called.

### Example

* **Input:** ["RandomizedSet", "insert", "remove", "insert", "getRandom", "remove", "insert", "getRandom"]

    [[], [1], [2], [2], [], [1], [2], []]
* **Output:** [null, true, false, true, 2, true, false, 2]
* **Explanation:** RandomizedSet randomizedSet = new RandomizedSet(); <br>
randomizedSet.insert(1); // Inserts 1 to the set. Returns true as 1 was inserted successfully. <br>
randomizedSet.remove(2); // Returns false as 2 does not exist in the set. <br>
randomizedSet.insert(2); // Inserts 2 to the set, returns true. Set now contains [1,2]. <br>
randomizedSet.getRandom(); // getRandom() should return either 1 or 2 randomly. <br>
randomizedSet.remove(1); // Removes 1 from the set, returns true. Set now contains [2]. <br>
randomizedSet.insert(2); // 2 was already in the set, so return false. <br>
randomizedSet.getRandom(); // Since 2 is the only number in the set, getRandom() will always return 2.


## Optimal Solution

To ensure insert and remove operations are done in $O(1)$ time, we need to avoid traversing through the entire array when removing. To achieve this, we use extra space, a map, to store the index in which an element is located.

Insert: To insert, we insert at the end of the array. So, the new index is the current size of array before insertion. This is O(1).

Remove: To remove an existing element, we first find the index of that element in the current array. Then we swap the element to remove with the last element of array. Then delete the last element. This way, we ensure that the operation is O(1).

getRandom: To get a random element, we generate a random integer of the valid indices.

```c++
class RandomizedSet {
    unordered_map<int, int> mp;
    vector<int> values; 
public:
    RandomizedSet() {
        
    }
    
    bool insert(int val) {
        if (mp.find(val) != mp.end()) {
            return false;
        }
        mp[val] = values.size();
        values.push_back(val);
        return true;
    }
    
    bool remove(int val) {
        if (mp.find(val) == mp.end()) {
            return false;
        }
        int idx = mp[val];
        mp[values.back()] = idx;
        mp.erase(val);
        values[idx] = values.back();
        values.pop_back();
        return true;
    }
    
    int getRandom() {
        int idx = rand() % values.size();
        return values[idx];
    }
};
```

### Complexity

* **Time Complexity:** $O(1)$
* **Space Complexity:** $O(n)$