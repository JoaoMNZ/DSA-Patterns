# <a href="https://leetcode.com/problems/valid-anagram/description/" target="_blank">242. Valid Anagram</a>

### Problem Statement
Given two strings `s` and `t`, return `true` if `t` is an anagram of `s`, and `false` otherwise. An Anagram is a word or phrase formed by rearranging the letters of a different word or phrase, typically using all the original letters exactly once.

### Approach
An anagram must contain the exact same characters with the exact same frequencies. The approach is to build a frequency map for each string and then compare the maps.

1.  **Initial Check:** Anagrams must have the same length. If `s.length()` is not equal to `t.length()`, they cannot be anagrams, so we can return `false` immediately.
2.  **Frequency Counting:** Create a `HashMap` for each string to store character counts. Iterate through the strings (which we know are the same length) and populate both maps.
3.  **Comparison:** Use the `.equals()` method to compare the two `HashMaps`. It will return `true` only if both maps contain the exact same keys with the exact same values.

### Implementation
```java
import java.util.HashMap;

class Solution {
    public boolean isAnagram(String s, String t) {
        if (s.length() != t.length()) {
            return false;
        }

        HashMap<Character, Integer> sCounts = new HashMap<>();
        HashMap<Character, Integer> tCounts = new HashMap<>();

        for (int i = 0; i < s.length(); i++) {
            sCounts.put(s.charAt(i), sCounts.getOrDefault(s.charAt(i), 0) + 1);
            tCounts.put(t.charAt(i), tCounts.getOrDefault(t.charAt(i), 0) + 1);
        }
        
        return sCounts.equals(tCounts);
    }
}
``` 

### Complexity Analysis
- **Time Complexity:** O(n)
  - Due to the initial length check, we only iterate through the strings when their lengths are equal (`n`). The single loop takes O(n) time.

- **Space Complexity:** O(1)
  - The space required by `HashMap` is proportional to the number of unique characters in the string. Since the character set is the 26 lowercase English letters, the space complexity is constant, O(1).
