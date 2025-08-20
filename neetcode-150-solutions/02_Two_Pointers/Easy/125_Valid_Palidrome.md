# <a href="https://leetcode.com/problems/valid-palindrome/" target="_blank">125. Valid Palindrome</a>

### Problem Statement
A phrase is a **palindrome** if, after converting all uppercase letters into lowercase letters and removing all non-alphanumeric characters, it reads the same forward and backward. Alphanumeric characters include letters and numbers. Given a string `s`, return `true` if it is a **palindrome**, or `false` otherwise.

### Approach
First, the input string must be sanitized. This means creating a new version of the string that is entirely lowercase and contains only alphanumeric characters.

After this sanitization, the problem can be solved with a **Two Pointers** approach. One pointer, `left`, starts at the beginning of the string, and another, `right`, starts at the end.

A loop runs as long as the `left` pointer is less than the `right` pointer. Inside the loop, we check if the character at `left` is the same as the character at `right`.
- If they are not equal, the string is not a palindrome, and we can immediately return `false`.
- If they are equal, we move the pointers one step closer to the center (`left++` and `right--`) and continue the check.

If the loop completes without returning `false`, it means every character matched its counterpart, confirming the string is a palindrome. In this case, we return `true`.

### Implementation
```java
class Solution {
    public boolean isPalindrome(String s) {
        s = s.toLowerCase();
        s = s.replaceAll("[^a-z0-9]", "");

        for (int left = 0, right = s.length() - 1; left < right; left++, right--) {
            if (s.charAt(left) != s.charAt(right)) {
                return false;
            }
        }
        
        return true;
    }
}
``` 

### Complexity Analysis
- **Time Complexity:** O(n)
  - The string sanitization methods (`toLowerCase`, `replaceAll`) take O(n) time. The subsequent `for` loop iterates through half of the sanitized string, which is also proportional to `n`.

- **Space Complexity:** O(n)
  - A new string is created in memory to store the sanitized version of `s`. In the worst case, the size of this new string is proportional to the original string's length, `n`.
