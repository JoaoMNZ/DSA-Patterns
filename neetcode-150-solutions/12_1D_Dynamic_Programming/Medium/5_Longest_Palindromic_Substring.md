# <a href="https://leetcode.com/problems/longest-palindromic-substring/" target="_blank">5. Longest Palindromic Substring</a>

### Problem Statement
Given a string `s`, return the longest palindromic substring in `s`.

A string is a **palindrome** when it reads the same backward as forward. A **substring** is a contiguous sequence of characters within the string.

### Approach
We can use a 2D boolean grid, `cache[n][n]`, where `cache[row][column]` will be `true` if the substring from `row` to `column` is a palindrome, and `false` otherwise.

The key is to fill this grid diagonally. We iterate through the string based on the *length* of the substring (using an `offset`). We start with substrings of length 1, then length 2, and so on.

A substring is a palindrome if two conditions are met:

1. The first and last characters are the same (`s.charAt(row) == s.charAt(column)`).

2. The inner substring (from `row + 1` to `column - 1`) is also a palindrome.

Because we are iterating by increasing length, the check for the inner substring (`cache[row + 1][column - 1]`) will have always been computed in a previous, smaller step.

The base case for the inner check is when the substring has a length of 1 or 2 (`column - row <= 1`). In this case, we only need to check if the first and last characters are equal.

As we fill the grid, every time we mark `cache[row][col]` as `true`, we check if this new palindrome is longer than the one we've stored so far and update our `begin` and `end` pointers if it is.

### Implementation
```java
class Solution {
    public String longestPalindrome(String s) {
        int n = s.length();
        int begin = 0;
        int end = 0;

        boolean cache[][] = new boolean[n][n];
        
        for(int offset = 0 ; offset < n  ; offset++){
            for(int row = 0 ; (row + offset) < n ; row++){
                int column = row + offset;
                if(s.charAt(row) == s.charAt(column) && (column - row <= 1 || cache[row + 1][column - 1])){
                    cache[row][column] = true;
                    if(column - row > end - begin){
                        begin = row;
                        end = column;
                    }
                }
            }
        }
        return s.substring(begin, end + 1);
    }
}
``` 

### Complexity Analysis
Let `n` be the length of the string `s`.

-   **Time Complexity:** O(n²)
    -   We iterate through the upper half of the `n x n` grid

-   **Space Complexity:** O(n²)
    -   We create a boolean grid of size `n x n` to store the results of our subproblems.
