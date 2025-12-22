# <a href="https://leetcode.com/problems/longest-repeating-character-replacement/" target="_blank">424. Longest Repeating Character Replacement</a>

### Problem Statement
You are given a string `s` and an integer `k`. You can choose any character of the string and change it to any other uppercase English character. You can perform this operation at most `k` times.

Return *the length of the longest substring containing the same letter you can get after performing the above operations*.

### Approach
The key insight is that to maximize the length of a substring with the same letter, we should convert all characters in that substring to match the **most frequent character** already present in it.

We use a **Sliding Window** technique:
1.  **Expand Window:** We iterate with a `right` pointer to expand our window. For each character, we increment its count in a frequency map.
2.  **Track Max Frequency:** We maintain a variable `maxFrequency` that holds the count of the most frequent character seen *within the current window*.
3.  **Check Validity:** A window is valid if the number of characters that need to be replaced is less than or equal to `k`.
    -   Formula: `(Window Size) - (Count of Most Frequent Char) <= k`.
    -   If this condition fails (i.e., we need more than `k` replacements), the window is invalid.
4.  **Shrink Window:** When the window becomes invalid, we increment the `left` pointer to shrink it from the start until it becomes valid again.
5.  **Result:** At every step (when the window is valid), we update our result with the maximum window size seen so far (`right - left + 1`).

### Implementation
```java
class Solution {
    public int characterReplacement(String s, int k) {
        int[] frequency = new int[26];
        int maxFrequency = 0;
        int left = 0;
        int result = 0;

        for(int right = 0 ; right < s.length() ; right++){
            int index = s.charAt(right) - 'A';
            frequency[index]++;
            maxFrequency = Math.max(maxFrequency, frequency[index]);
            while((right - left + 1) - maxFrequency > k){
                index = s.charAt(left) - 'A';
                frequency[index]--;
                left++;
            }
            result = Math.max(result, right - left + 1);
        }

        return result;
    }
}
``` 

### Complexity Analysis
Let `n` be the length of string `s`.

-   **Time Complexity:** $O(n)$
    -   The `right` pointer iterates through the string once. The `left` pointer also moves at most `n` times. Thus, the total operations are proportional to `n`.

-   **Space Complexity:** $O(1)$
    -   We use an integer array of size 26 to store frequencies, which is constant space regardless of the input string length.
