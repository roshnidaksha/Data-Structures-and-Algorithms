# 16. Roman to Integer and 17. Integer to Roman

# Roman to Integer

**Link:** [Roman to Integer](https://leetcode.com/problems/roman-to-integer)

**Difficulty:** Easy

**Topics:** Hash Table Math, String

## Problem Description

Roman numerals are represented by seven different symbols: `I`, `V`, `X`, `L`, `C`, `D` and `M`.

| Symbol | Value |
| :--- | :--- | 
| I | 1 |
| V | 5 |
| X | 10 |
| L | 50 |
| C | 100 |
| D | 500 |
| M | 1000 |

For example, `2` is written as `II` in Roman numeral, just two ones added together. `12` is written as `XII`, which is simply `X + II`. The number `27` is written as `XXVII`, which is `XX + V + II`.

Roman numerals are usually written largest to smallest from left to right. However, the numeral for four is not `IIII`. Instead, the number four is written as `IV`. Because the one is before the five we subtract it making four. The same principle applies to the number nine, which is written as `IX`. There are six instances where subtraction is used:

* `I` can be placed before `V` (5) and `X` (10) to make 4 and 9. 
* `X` can be placed before `L` (50) and `C` (100) to make 40 and 90. 
* `C` can be placed before `D` (500) and `M` (1000) to make 400 and 900.

Given a roman numeral, convert it to an integer.

### Constraints:

* $1 \leq s.length \leq 15$
* s contains only the characters ('I', 'V', 'X', 'L', 'C', 'D', 'M').
* It is guaranteed that s is a valid roman numeral in the range [1, 3999].

### Example

* **Input:** s = "III"
* **Output:** 3

## Optimal Solution

Observing roman numbers, you need to either group 1 or 2 characters to find the value. Except when there are subtraction forms present, the roman numbers are always decreasing in value. So, to determine the presence of subtraction forms accurately, we can process two characters at a time. If the current character is lesser than the next character, it indicates a subtraction form. So, we subtract the current character value. 

```c++
class Solution {
public:
    int romanToInt(string s) {
        unordered_map<char, int> roman = {
            {'I', 1},
            {'V', 5},
            {'X', 10}, 
            {'L', 50},
            {'C', 100},
            {'D', 500},
            {'M', 1000}
        };

        int res = 0;
        for (int i = 0; i < s.size() - 1; i++) {
            if (roman[s[i]] < roman[s[i + 1]]) {
                res -= roman[s[i]];
            } else {
                res += roman[s[i]];
            }
        }
        res += roman[s[s.size() - 1]];
        return res;
    }
};
```

### Complexity

* **Time Complexity:** $O(n)$ where `n` is the length of input string. 
* **Space Complexity:** $O(1)$

<br>

# Integer to Roman

**Link:** [Integer to Roman](https://leetcode.com/problems/integer-to-roman)

**Difficulty:** Medium

**Topics:** Hash Table Math, String

## Problem Description

Roman numerals are formed by appending the conversions of decimal place values from highest to lowest. Converting a decimal place value into a Roman numeral has the following rules:

* If the value does not start with 4 or 9, select the symbol of the maximal value that can be subtracted from the input, append that symbol to the result, subtract its value, and convert the remainder to a Roman numeral.

* If the value starts with 4 or 9 use the subtractive form representing one symbol subtracted from the following symbol, for example, 4 is 1 (`I`) less than 5 (`V`): `IV` and 9 is 1 (`I`) less than 10 (`X`): `IX`. Only the following subtractive forms are used: 4 (`IV`), 9 (`IX`), 40 (`XL`), 90 (`XC`), 400 (`CD`) and 900 (`CM`).

* Only powers of 10 (`I`, `X`, `C`, `M`) can be appended consecutively at most 3 times to represent multiples of 10. You cannot append 5 (`V`), 50 (`L`), or 500 (`D`) multiple times. If you need to append a symbol 4 times use the subtractive form.

Given an integer, convert it to a Roman numeral.

### Constraints:

* $1 \leq num \leq 3999$

### Example

* **Input:** 3749
* **Output:** MMMDCCXLIX
* **Explanation:** <br>
3000 = MMM as 1000 (M) + 1000 (M) + 1000 (M) <br>
700 = DCC as 500 (D) + 100 (C) + 100 (C) <br>
40 = XL as 10 (X) less of 50 (L) <br>
9 = IX as 1 (I) less of 10 (X) <br>
Note: 49 is not 1 (I) less of 50 (L) because the conversion is based on decimal places

## Optimal Solution

There are a fixed number of roman numerals. For certain values like 4, 9, 40, 90, 400, 900 we need to use subtractive forms. And for values like 5, 50, 500, the numerals cannot be repeated multiple times. 

Now, writing all the values in decending order:

1000, 900, 500, 400, 100, 90, 50, 40, 10, 9, 5, 4, 1

If we traverse through the above list one by one add keep adding its corresponding numeral until the number is greater than the value, we can get the roman numeral. 

The approach also guarantees that 5, 50, 500 are not repeated and subtraction forms are correctly used. 

```c++
class Solution {
public:
    string intToRoman(int num) {
        const vector<pair<int, string>> valueSymbols {
            {1000, "M"}, {900, "CM"}, {500, "D"}, {400, "CD"}, 
            {100, "C"}, {90, "XC"}, {50, "L"}, {40, "XL"}, 
            {10, "X"}, {9, "IX"}, {5, "V"}, {4, "IV"}, {1, "I"}
        };

        string res = "";
        for (const auto& [value, symbol] : valueSymbols) {
            if (num == 0) break;
            while (num >= value) {
                res += symbol;
                num -= value;
            }
        }
        return res;
    }
};
```

### Complexity

The time complexity is $O(1)$ as the input range is small, and there is a constant number of roman numerals. 
* **Time Complexity:** $O(1)$
* **Space Complexity:** $O(1)$