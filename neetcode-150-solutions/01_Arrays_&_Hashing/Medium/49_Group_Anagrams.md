# <a href="https://leetcode.com/problems/group-anagrams/" target="_blank">49. Group Anagrams</a>

### Problem Statement
Given an array of strings `strs`, group the anagrams together. You can return the answer in any order. An Anagram is a word or phrase formed by rearranging the letters of a different word or phrase, typically using all the original letters exactly once.

### Approach
The key to this problem is finding a unique "signature" or "key" for each group of anagrams. Since anagrams always have the exact same characters with the same frequencies, we can use a character count as this unique key.

The plan is as follows:
1.  We will use a `HashMap` where the key represents an anagram group, and the value is a `List` of all the strings that belong to that group.
2.  We iterate through each string in the input array.
3.  For each string, we create a character count key. A simple way to do this is with an integer array of size 26 (for each letter 'a' through 'z'). We count the letters in the current string and store them in this array.
4.  We convert this count array into a string to use it as a key for our `HashMap`.
5.  We then add the original string to the list associated with its key in the map. If the key doesn't exist yet, we create a new list for it first.
6.  After checking all the strings, the `HashMap`'s values will be the lists of grouped anagrams.

### Implementation
```java
import java.util.ArrayList;
import java.util.Arrays;
import java.util.HashMap;
import java.util.List;

class Solution {
    public List<List<String>> groupAnagrams(String[] strs) {
        HashMap<String, List<String>> result = new HashMap<>();
        
        for (int i = 0; i < strs.length; i++) {
            int[] count = new int[26];

            for (int j = 0; j < strs[i].length(); j++) {
                count[strs[i].charAt(j) - 'a']++;
            }

            String key = Arrays.toString(count);
            result.putIfAbsent(key, new ArrayList<>());
            result.get(key).add(strs[i]);
        }
        
        return new ArrayList<>(result.values());
    }
}
``` 

### Complexity Analysis
- **Time Complexity:** O(n * m)
  - Let `n` be the number of strings and `m` be the average length of a string. We must iterate through each of the `n` strings. For each string, we then iterate through its `m` characters to build the count array. This results in a total time complexity of `O(n * m)`.

- **Space Complexity:** O(n * m)
  - The total information we need to store is all the characters from every string. The space required for the `HashMap` will be proportional to the total number of characters in the input, which is `n * m` on average.
