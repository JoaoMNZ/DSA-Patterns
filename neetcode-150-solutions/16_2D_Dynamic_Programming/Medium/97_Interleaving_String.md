# <a href="https://leetcode.com/problems/interleaving-string/" target="_blank">97. Interleaving String</a>

### Problem Statement
Given strings `s1`, `s2`, and `s3`, find whether `s3` is formed by an interleaving of `s1` and `s2`.

An **interleaving** of two strings `s1` and `s2` is a configuration where `s3` is formed by the characters of `s1` and `s2` combined while maintaining the relative order of the characters from each string.

### Approach
We can use a 2D boolean grid, `cache[i][j]`, to store the answers to subproblems.

In this approach, `cache[i][j]` will represent the answer to the question: "Can the end of `s1` (starting from index `i`) and the end of `s2` (starting from index `j`) successfully interleave to form the end of `s3` (starting from index `i + j`)?"

1.  **Base Case:** We start at the end of all three strings. `cache[s1.length()][s2.length()]` is set to `true`, because two empty strings can perfectly form one empty string.
2.  **DP Logic:** We fill the grid from the bottom-right corner up to the top-left (`cache[0][0]`). For any cell `cache[i][j]`, it can become `true` if at least one of two conditions is met:
    -   The character `s1[i]` matches the corresponding character `s3[i+j]`, AND the subproblem *below* it (`cache[i+1][j]`) is `true`.
    -   The character `s2[j]` matches the corresponding character `s3[i+j]`, AND the subproblem to its *right* (`cache[i][j+1]`) is `true`.
3.  **Final Answer:** The solution to the original problem will be stored in `cache[0][0]`, which asks if the *full* `s1` and `s2` can form the *full* `s3`.

### Implementation
```java
class Solution {
    public boolean isInterleave(String s1, String s2, String s3) {
        int n = s1.length();
        int m = s2.length();
        if(s3.length() != n + m)
            return false;
        
        boolean[][] cache = new boolean[n + 1][m + 1];
        cache[n][m] = true;

        for(int i = n ; i >= 0 ; i--){
            for(int j = m ; j >= 0 ; j--){
                if(i < n && s1.charAt(i) == s3.charAt(i + j) && cache[i + 1][j]){
                    cache[i][j] = true;
                }
                if(j < m && s2.charAt(j) == s3.charAt(i + j) && cache[i][j + 1]){
                    cache[i][j] = true;
                }
            }
        } 

        return cache[0][0];
    }
}
``` 

### Complexity Analysis
Let `m` be the length of `s1` and `n` be the length of `s2`.

-   **Time Complexity:** O(m * n)
    -   We must visit and perform a constant-time check for each cell in the `(m+1) x (n+1)` grid.

-   **Space Complexity:** O(m * n)
    -   We create a 2D `cache` array of size `(m+1) x (n+1)` to store the results of all subproblems.
