# <a href="https://leetcode.com/problems/word-break/" target="_blank">139. Word Break</a>

### Problem Statement
Given a string `s` and a dictionary of strings `wordDict`, return `true` if `s` can be segmented into a space-separated sequence of one or more dictionary words.

Note that the same word in the dictionary may be reused multiple times in the segmentation.

### Approach
We can use a boolean `cache` array where `cache[i]` will be `true` if the substring of `s` starting from index `i` all the way to the end can be segmented using words from the dictionary.

Our goal is to find the value of `cache[0]`.

1.  **Base Case:** We initialize `cache[s.length()] = true`. This represents an empty string (the end of the string), which is always "breakable."
2.  **DP Logic:** We iterate backwards through the string `s`, from the end to the beginning (index `i`). For each `i`, we try to find a `word` in the `wordDict` that starts at that position.
    -   If we find a `word` that matches `s.substring(i, i + word.length())`, we then check if the *rest* of the string is *also* breakable by looking at our cache: `cache[i + word.length()]`.
    -   If the rest of the string is breakable (`cache[i + word.length()]` is `true`), then we know `cache[i]` is also `true`. We can mark it and break from the inner loop (since we only need one valid break).
3.  **Final Answer:** After filling the array, `cache[0]` will tell us if the entire string (starting from index 0) can be segmented.

### Implementation
```java
class Solution {
    public boolean wordBreak(String s, List<String> wordDict) {
        boolean[] cache = new boolean[s.length() + 1];
        cache[s.length()] = true;

        for(int begin = s.length() - 1 ; begin >= 0 ; begin--){
            for(String word : wordDict){
                if(begin + word.length() <= s.length() && s.substring(begin, begin + word.length()).equals(word)){
                    cache[begin] = cache[begin + word.length()];

                    if(cache[begin])
                        break;
                }
            }
        }

        return cache[0];
    }
}
``` 

### Complexity Analysis
Let `n` be the length of the string `s` and `m` be the number of words in the `wordDict`.

-   **Time Complexity:** O(n * m * t)
    -   We have a loop that runs `n` times (for each `begin` index). Inside that, we loop `m` times (for each `word`). The `substring` and `equals` operations take `O(t)` time, where `t` is the length of the word.

-   **Space Complexity:** O(n)
    -   We create a `cache` array of size `n + 1`.
