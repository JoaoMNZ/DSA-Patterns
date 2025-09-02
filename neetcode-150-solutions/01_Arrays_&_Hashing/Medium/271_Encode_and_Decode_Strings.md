# <a href="https://leetcode.com/problems/encode-and-decode-strings/" target="_blank">271. Encode and Decode Strings</a>

### Problem Statement
Design an algorithm to encode a list of strings to a single string. The encoded string is then decoded back to the original list of strings. Please implement `encode` and `decode`.

### Approach
The main challenge is how to combine multiple strings into one and still know where each individual string begins and ends. Simply joining them isn't enough (e.g., `["a", "b"]` and `["ab"]` would both become `"ab"`).

A reliable solution is to create a simple protocol. Before each word, we'll add a header with its length, followed by a special separator character like `#`.

**Encoding Process:**
For each word in the input list, we create a chunk in the format `[length]#[word]`.
- For example, the list `["hello", "world"]` would be encoded by taking "hello" (length 5) to make `"5#hello"`, and "world" (length 5) to make `"5#world"`. The final combined string is `"5#hello5#world"`.

**Decoding Process:**
We read the encoded string from left to right.
1.  Find the first `#` separator. The number before it tells us the length of the upcoming word.
2.  We parse that number and then read exactly that many characters from the string to reconstruct the original word.
3.  We add the word to our result list and move our pointer to the start of the next chunk.
4.  This process is repeated until the entire encoded string is consumed.

### Implementation
```java
import java.util.List;
import java.util.LinkedList;
import java.util.ArrayList;

public class Solution {

    public String encode(List<String> words) {
        StringBuilder encoded = new StringBuilder();
        for (String word : words) {
            encoded.append(word.length()).append("#").append(word);
        }
        return encoded.toString();
    }

    public List<String> decode(String encodedStr) {
        List<String> result = new LinkedList<>();
        int currentIndex = 0;

        while (currentIndex < encodedStr.length()) {
            int separatorIndex = encodedStr.indexOf("#", currentIndex);
            int wordLength = Integer.parseInt(encodedStr.substring(currentIndex, separatorIndex));
            int wordStart = separatorIndex + 1;
            int wordEnd = wordStart + wordLength;
            result.add(encodedStr.substring(wordStart, wordEnd));
            currentIndex = wordEnd;
        }

        return result;
    }
}
``` 

### Complexity Analysis
Let `L` be the total number of characters across all strings in the list.

- **Time Complexity:** O(L)
    - Both the `encode` and `decode` methods need to process each character from the input strings exactly once. Therefore, the time complexity is linear in relation to the total number of characters.

- **Space Complexity:** O(L)
  - The `encode` method creates a new string whose size is proportional to `L`. The `decode` method creates a list of strings that, combined, also have a total size proportional to `L`.
