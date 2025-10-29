# <a href="https://leetcode.com/problems/decode-ways/" target="_blank">91. Decode Ways</a>

### Problem Statement
A message containing letters from `A-Z` can be **encoded** into numbers using the following mapping:
'A' -> "1"
'B' -> "2"
...
'Z' -> "26"

To **decode** an encoded message, all the digits must be grouped then mapped back into letters using the reverse of the mapping above (there may be multiple ways). For example, `"11106"` can be mapped into:
- `"AAJF"` with the grouping `(1 1 10 6)`
- `"KJF"` with the grouping `(11 10 6)`

Given a string `s` containing only digits, return *the **number** of ways to **decode** it*.

### Approach
This problem asks for the number of ways to do something, which often suggests **Dynamic Programming** or **Recursion with Memoization**. We can break the problem into smaller subproblems: the number of ways to decode a string starting from index `i`.

For any index `i`, we have two potential choices:
1.  **Decode a single digit:** We can take the digit at `s[i]` and decode it as a single letter. This is valid only if the digit is not `'0'`. The number of ways for this choice is the result of the subproblem starting at `i + 1`.
2.  **Decode two digits:** We can take the digits at `s[i]` and `s[i+1]` together and decode them as a single letter. This is valid only if the two-digit number formed is between `10` and `26` inclusive. The number of ways for this choice is the result of the subproblem starting at `i + 2`.

The total number of ways to decode from index `i` is the sum of the ways from these valid choices.

To avoid recomputing the same subproblems multiple times, we use a `cache` array (memoization).

1.  The `dfs(i)` function calculates the number of ways to decode the substring starting at index `i`.
2.  **Base Cases:**
    -   If `i` reaches the end of the string (`s.length()`), we have successfully decoded the entire string. This counts as 1 valid way.
    -   If the character at `s[i]` is `'0'`, it cannot be decoded (neither as a single digit nor as the start of a two-digit number), so there are 0 ways from here.
3.  **Memoization Check:** If `cache[i]` has already been computed, return the stored value.
4.  **Recursive Step:** Calculate the number of ways by summing the results of the recursive calls for the valid choices (decode 1 digit and potentially decode 2 digits).
5.  Store the result in `cache[i]` before returning.

### Implementation
```java
import java.util.Arrays;

class Solution {
    public int numDecodings(String s) {
        int[] cache = new int[s.length()];
        Arrays.fill(cache, -1);
        return dfs(s, cache, 0);
    }

    public int dfs(String s, int[] cache, int i){
        if (i == s.length()) 
            return 1;
        if (s.charAt(i) == '0') 
            return 0;
        if (cache[i] != -1) 
            return cache[i];

        int ways = dfs(s, cache, i + 1);

        if (i + 1 < s.length()) {
            int twoDigits = Integer.parseInt(s.substring(i, i + 2));
            if (twoDigits >= 10 && twoDigits <= 26) {
                ways += dfs(s, cache, i + 2);
            }
        }

        return cache[i] = ways;
    }
}
``` 

### Complexity Analysis
Let `n` be the length of the string `s`.

-   **Time Complexity:** O(n)
    -   Due to memoization, the `dfs` function calculates the result for each index `i` only once.

-   **Space Complexity:** O(n)
    -   The space is used by the `cache` array (size `n`) and by the recursion stack, which can go up to `n` levels deep.
