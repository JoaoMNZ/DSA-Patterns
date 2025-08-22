# <a href="https://leetcode.com/problems/evaluate-reverse-polish-notation/" target="_blank">150. Evaluate Reverse Polish Notation</a>

### Problem Statement
You are given an array of strings `tokens` that represents an arithmetic expression in a Reverse Polish Notation. Evaluate the expression and return an integer that represents its value.

The valid operators are `+`, `-`, `*`, and `/`. Division truncates toward zero. The input is always a valid expression, and there is no division by zero.

### Approach
Reverse Polish Notation (RPN) is a way to write math where the operator comes *after* the numbers it applies to (e.g., `5 10 +` is the same as `5 + 10`).

A **Stack** is the perfect tool for this because we only ever need to work with the last two numbers we've seen. A stack follows the "Last-In, First-Out" (LIFO) principle, which gives us easy access to these numbers.

The plan is simple:
1.  Read each token in the input array one by one.
2.  If the token is a **number**, we push it onto the top of the stack.
3.  If the token is an **operator** (`+`, `-`, `*`, `/`):
    - We pop the last two numbers from the stack. Let's call them `n2` (popped first) and `n1` (popped second).
    - We perform the operation (like `n1 - n2`). **Note:** The order matters for subtraction and division.
    - We push the result of the calculation back onto the stack.
4.  After we've gone through all the tokens, the stack will have only one number left, which is our final answer.

### Implementation
```java
import java.util.Stack;

class Solution {    
    public int evalRPN(String[] tokens) {
        Stack<Integer> stack = new Stack<>();
        
        for (String s : tokens) {
            if (s.equals("+")) {
                int n2 = stack.pop();
                int n1 = stack.pop();
                stack.push(n1 + n2);
            } else if (s.equals("-")) {
                int n2 = stack.pop();
                int n1 = stack.pop();
                stack.push(n1 - n2);
            } else if (s.equals("*")) {
                int n2 = stack.pop();
                int n1 = stack.pop();
                stack.push(n1 * n2);
            } else if (s.equals("/")) {
                int n2 = stack.pop();
                int n1 = stack.pop();
                stack.push(n1 / n2);
            } else {
                stack.push(Integer.parseInt(s));
            }
        }
        return stack.peek();
    }
}
``` 

### Complexity Analysis
- **Time Complexity:** O(n)
  - We need to look at each of the `n` tokens in the input array exactly one time.

- **Space Complexity:** O(n)
  - In the worst case, we might have to store many numbers on the stack before we find operators. For an input like `["10", "6", "9", ..., "+", "-"]`, the stack's size will grow with the number of operands. Therefore, the space needed depends on the input size `n`.
