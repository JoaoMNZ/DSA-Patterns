# <a href="https://leetcode.com/problems/palindrome-partitioning/" target="_blank">131. Palindrome Partitioning</a>

### Problem Statement
Given a string `s`, partition `s` such that every substring of the partition is a **palindrome**. Return all possible palindrome partitionings of `s`.

### Approach
This problem asks for all possible valid partitions, which is a strong signal to use a **Backtracking** algorithm. We will build a valid partition step by step.

The core idea is to try every possible "cut" in the string.
1.  Our recursive function will keep track of the `beginIndex`, which is the starting point of the substring we are currently trying to partition.
2.  From `beginIndex`, we loop forward with an `endIndex` to generate every possible substring (`s.substring(beginIndex, endIndex)`).
3.  For each substring, we check if it's a palindrome.
    -   If it is, we add it to our `currentPartition` and make a recursive call for the rest of the string, starting from our `endIndex`.
    -   If it's not a palindrome, we discard it and try the next, longer substring.
4.  After a recursive call returns, we backtrack by removing the substring we just added. This allows us to explore other possible partitions (e.g., trying `"aa"` after trying `"a"` for the string `"aab"`).
5.  The **base case** is when our `beginIndex` reaches the end of the string. This means we have successfully partitioned the entire string into palindromes, so we add a copy of the `currentPartition` to our final results.

### Implementation
```java
import java.util.List;
import java.util.ArrayList;

class Solution {
    private List<List<String>> partitions = new ArrayList<>();

    public List<List<String>> partition(String s) {
        backtrack(s, new ArrayList<String>(), 0);
        return partitions;
    }

    private void backtrack(String s, List<String> currentPartition, int beginIndex) {
        if (beginIndex == s.length()) {
            partitions.add(new ArrayList<>(currentPartition));
            return;
        }

        for (int endIndex = beginIndex + 1; endIndex <= s.length(); endIndex++) {
            String substring = s.substring(beginIndex, endIndex);
            if (isPalindrome(substring)) {
                currentPartition.add(substring);
                backtrack(s, currentPartition, endIndex);
                currentPartition.remove(currentPartition.size() - 1);
            }
        }
    }

    private boolean isPalindrome(String substring) {
        int left = 0;
        int right = substring.length() - 1;
        while (left < right) {
            if (substring.charAt(left) != substring.charAt(right)) {
                return false;
            }
            left++;
            right--;
        }
        return true;
    }
}
``` 

### Complexity Analysis
Let `n` be the length of the string `s`.

-   **Time Complexity:** O(n * 2ⁿ)
    -   In the worst case (a string of all the same characters like "aaaa"), every possible substring is a palindrome. The algorithm generates `2ⁿ` possible partitions. For each partition found, creating the substring and checking if it's a palindrome takes `O(n)` time.

-   **Space Complexity:** O(n)
    -   The auxiliary space is determined by the maximum depth of the recursion stack, which can be up to `n` in the worst case.
