# <a href="https://leetcode.com/problems/minimum-window-substring/" target="_blank">76. Minimum Window Substring</a>

### Problem Statement
Given two strings `s` and `t` of lengths `m` and `n` respectively, return *the minimum window substring of `s` such that every character in `t` (including duplicates) is included in the window*. If there is no such substring, return the empty string `""`.

The test cases will be generated such that the answer is unique.

### Approach
We need to find the smallest substring in `s` that contains all the characters (including frequency) of `t`. This can be solved efficiently using the **Sliding Window** technique combined with a **Frequency Map**.



1.  **Setup:**
    -   We use a `HashMap` to store the count of each character in `t`. This represents our "target" requirements.
    -   We maintain a variable `have` to track how many required characters we have currently collected in our window, and `need` which is the total number of characters in `t`.
    
2.  **Expand (Right Pointer):**
    -   We iterate through `s` with the `right` pointer.
    -   If the current character exists in our map, we decrement its count.
    -   **Crucial Step:** If the count in the map becomes non-negative (`>= 0`), it means this specific instance of the character was *needed*. We increment `have`.

3.  **Shrink (Left Pointer):**
    -   Once `have == need`, it means the current window `[left, right]` is valid (contains all characters of `t`).
    -   **Update Result:** We check if this window is smaller than the smallest we've seen so far (`end - start`). If so, we record the new coordinates.
    -   **Optimization:** We try to make the window smaller by moving the `left` pointer to the right.
        -   If the character at `left` is in the map, we increment its count back.
        -   If the count becomes positive (`> 0`), it means we have lost a *required* character. We decrement `have`.
    -   We repeat the shrinking until `have < need`.

### Implementation
```java
import java.util.Map;
import java.util.HashMap;

class Solution {
    public String minWindow(String s, String t) {
        Map<Character, Integer> map = new HashMap<>();
        for(char letter : t.toCharArray()){
            map.put(letter, map.getOrDefault(letter, 0) + 1);
        }

        int start = 0, end = Integer.MAX_VALUE;
        int have = 0, need = t.length();
        int left = 0;

        for(int right = 0 ; right < s.length() ; right++){
            char rightLetter = s.charAt(right);
            if(map.containsKey(rightLetter)){
                map.put(rightLetter, map.get(rightLetter) - 1);
                if(map.get(rightLetter) >= 0){
                    have++;
                }
            }

            while(have == need){
                if(right - left < end - start){
                    start = left;
                    end = right;
                }

                char leftLetter = s.charAt(left);
                if(map.containsKey(leftLetter)){
                    map.put(leftLetter, map.get(leftLetter) + 1);
                    if(map.get(leftLetter) > 0){
                        have--;
                    }
                }
                left++;
            }
        }
        
        return end == Integer.MAX_VALUE ? "" : s.substring(start, end + 1);
    }
}
``` 

### Complexity Analysis
Let `n` be the length of string `s` and `m` is the lenght of string `t`.

-   **Time Complexity:** $O(n + m)$
    -   We iterate through `t` once to build the map ($O(m)$).
    -   We iterate through `s` ($O(n)$). The `left` and `right` pointers each move at most `n` steps.

-   **Space Complexity:** $O(m)$
    -   The `HashMap` stores only unique characters, in the worst case (all characters in `t` are unique) it can store `m` characters.
