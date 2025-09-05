# <a href="https://leetcode.com/problems/longest-substring-without-repeating-characters/" target="_blank">3. Longest Substring Without Repeating Characters</a>

### Problem Statement
Given a string `s`, find the length of the longest substring without repeating characters.

### Approach
This problem can be solved efficiently using the **Sliding Window** technique. A "window" is a substring defined by a `windowStart` (left) pointer and a `windowEnd` (right) pointer.

The core idea is to expand this window with the `windowEnd` pointer and shrink it with the `windowStart` pointer whenever we find a duplicate character.

1.  We use a `HashMap` to store the most recent index of each character we've seen.
2.  We iterate through the string with our `windowEnd` pointer to expand the window.
3.  For each character, we check if it's already in our map and if its last seen index is inside our current window (`>= windowStart`).
    -   If it is, we have a duplicate inside our window. To fix this, we shrink the window by moving our `windowStart` pointer to the position right after the last occurrence of that character.
4.  We then update the map with the current character's new index.
5.  In every step, we calculate the length of the current valid window (`windowEnd - windowStart + 1`) and keep track of the maximum length found so far.

After the loop finishes, we will have found the length of the longest substring.

### Implementation
```java
import java.util.Map;
import java.util.HashMap;

class Solution {
    public int lengthOfLongestSubstring(String s) {
        Map<Character, Integer> lastSeenIndex = new HashMap<>();
        int maxLength = 0;
        int windowStart = 0;

        for (int windowEnd = 0; windowEnd < s.length(); windowEnd++) {
            char currentChar = s.charAt(windowEnd);
            
            if (lastSeenIndex.containsKey(currentChar) && lastSeenIndex.get(currentChar) >= windowStart) {
                windowStart = lastSeenIndex.get(currentChar) + 1;
            }
            
            lastSeenIndex.put(currentChar, windowEnd);
            maxLength = Math.max(maxLength, windowEnd - windowStart + 1);
        }

        return maxLength;
    }
}
``` 

### Complexity Analysis
-   **Time Complexity:** O(n)
    -   We iterate through the string with the `windowEnd` pointer exactly once. Each character is processed a constant number of times.

-   **Space Complexity:** O(n)
    -   In the worst-case scenario, where the string has all unique characters, the `HashMap` will grow to a size of `n`.
