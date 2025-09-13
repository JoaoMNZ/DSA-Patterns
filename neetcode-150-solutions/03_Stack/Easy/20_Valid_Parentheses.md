# <a href="https://leetcode.com/problems/valid-parentheses/" target="_blank">20. Valid Parentheses</a>

### Problem Statement
Given a string `s` containing just the characters `(`, `)`, `{`, `}`, `[` and `]`, determine if the input string is valid.

An input string is valid if:
1.  Open brackets must be closed by the same type of brackets.
2.  Open brackets must be closed in the correct order.
3.  Every close bracket has a corresponding open bracket of the same type.

### Approach
This problem is a classic use case for a **Stack** data structure. A stack follows the "Last-In, First-Out" (LIFO) principle, which perfectly matches how nested parentheses must be closed: the last open bracket we see must be the first one to be closed.

The algorithm is as follows:
1.  We iterate through the input string, character by character.
2.  When we see an **open bracket** (`(`, `[`, or `{`), we push its corresponding **close bracket** (`)`, `]`, or `}`) onto the stack. This sets an expectation for what closing character we need to find next.
3.  When we see a **close bracket**, we check the stack:
    -   If the stack is empty, it means we have a closing bracket with no matching open one, so the string is invalid.
    -   If the stack is not empty, we pop the character from the top and compare it to the current closing bracket. If they don't match, the brackets are not in the correct order, and the string is invalid.
4.  After the loop finishes, the stack must be empty. If it's not, it means there are unclosed open brackets, and the string is also invalid.

### Implementation
```java
import java.util.Deque;
import java.util.ArrayDeque;

class Solution {
    public boolean isValid(String s) {
        Deque<Character> stack = new ArrayDeque<>();
        for (char c : s.toCharArray()) {
            if (c == '(') {
                stack.push(')');
            } else if (c == '[') {
                stack.push(']');
            } else if (c == '{') {
                stack.push('}');
            } else {
                if(stack.isEmpty()){
                   return false;
                }else{
                    if(stack.peek() != c){
                        return false;
                    }
                    stack.pop();
                }
            }
        }
        return stack.isEmpty();
    }
}
```

### Complexity Analysis
-   **Time Complexity:** O(n)
    -   We iterate through the input string of length `n` exactly once.

-   **Space Complexity:** O(n)
    -   In the worst-case scenario (e.g., a string of all open brackets like `"((("` ), the stack will grow to a size of `n`.
