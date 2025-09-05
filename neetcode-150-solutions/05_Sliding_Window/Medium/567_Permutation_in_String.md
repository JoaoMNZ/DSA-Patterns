# <a href="https://leetcode.com/problems/permutation-in-string/" target="_blank">567. Permutation in String</a>

### Problem Statement
Given two strings `s1` and `s2`, return `true` if `s2` contains a permutation of `s1`, or `false` otherwise.

In other words, return `true` if one of `s1`'s permutations is a substring of `s2`.

### Approach
This problem can be solved efficiently using the **Sliding Window** technique. The core idea is that a permutation of `s1` is a string of the same length with the exact same character counts. Our goal is to see if any substring in `s2` matches this character count.

1.  **Frequency Map:** First, we create a frequency map of `s1`. Since the problem deals with lowercase English letters, an integer array of size 26 is a simple and fast way to do this.
2.  **Sliding Window:** We then iterate through `s2`, maintaining a "window" of the same size as `s1`. We use a second frequency map array to keep track of the character counts within our current window.
3.  **Comparison:** Once the window is full (i.e., we have processed `s1.length()` characters), we compare the window's frequency map with `s1`'s frequency map. If they are identical, we have found a permutation and return `true`.
4.  **Sliding:** If they are not identical, we slide the window one position to the right. We do this by decrementing the count of the character that is leaving the window on the left. In the next iteration of the loop, the new character on the right will be added, and the check will be performed again.

### Implementation
```java
import java.util.Arrays;

class Solution {
    public boolean checkInclusion(String s1, String s2) {
        if (s1.length() > s2.length()) {
            return false;
        }

        int[] s1Freq = new int[26];
        int[] windowFreq = new int[26];
        for (char c : s1.toCharArray()) {
            s1Freq[c - 'a']++;
        }

        for (int i = 0; i < s2.length(); i++) {
            windowFreq[s2.charAt(i) - 'a']++;
            if (i >= s1.length() - 1) {
                if(Arrays.equals(s1Freq, windowFreq)) {
                    return true;
                }
                windowFreq[s2.charAt(i - s1.length() + 1) - 'a']--;
            }
        }
        
        return false;
    }
}
``` 

### Complexity Analysis
Let `n` be the length of `s2`.

-   **Time Complexity:** O(n)
    -   The main loop iterates through `s2` once. `Arrays.equals` compares the two frequency arrays in a constant amount of time (26 operations). Therefore, the total time complexity is `O(26 * n)`, which simplifies to `O(n)`.

-   **Space Complexity:** O(1)
    -   We use two integer arrays of a fixed size (26) to store character frequencies. The space required is constant and does not depend on the length of the input strings.
