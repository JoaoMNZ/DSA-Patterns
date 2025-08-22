# <a href="https://leetcode.com/problems/min-stack/description/" target="_blank">155. Min Stack</a>

### Problem Statement
Design a stack that supports `push`, `pop`, `top`, and retrieving the minimum element in constant time.

Implement the `MinStack` class:
- `MinStack()` initializes the stack object.
- `void push(int val)` pushes the element `val` onto the stack.
- `void pop()` removes the element on the top of the stack.
- `int top()` gets the top element of the stack.
- `int getMin()` retrieves the minimum element in the stack.

You must implement a solution with O(1) time complexity for each function.

### Approach
The main challenge is getting the minimum element in `O(1)` time. A regular stack is a **LIFO** (Last-In, First-Out) structure, and if we pop the current minimum value, we lose track of what the *next* minimum value is without searching the rest of the stack.

The solution is to have each node in the stack store not just its own value, but also a "snapshot" of the minimum value in the stack at the moment it was pushed.

Here's how it works:
1.  We'll build our own stack using linked `Node` objects. Each `Node` will hold three pieces of information: its own `value`, a pointer to the `nextNode` below it, and the minimum value found so far (`minSoFar`).
2.  When we **push** a new value, we compare it with the minimum of the current top node. The smaller of the two is stored as the `minSoFar` for our new node. This way, the top node always knows the overall minimum.
3.  **`getMin()`** becomes very easy: we just return the `minSoFar` value from the current top node.
4.  When we **pop**, the old top is removed. The new top node that is revealed already contains the correct minimum value from the previous state.

### Implementation
```java
class MinStack {
    private Node top;

    // Node class to store value, link to the next node, and the minimum so far
    private class Node {
        public int value;
        public Node nextNode;
        public int minSoFar;

        public Node(int value, Node nextNode, int minSoFar) {
            this.value = value;
            this.nextNode = nextNode;
            this.minSoFar = minSoFar;
        }
    }

    public MinStack() {
        this.top = null;
    }
    
    public void push(int val) {
        if (top == null) {
            // If the stack is empty, the first node's min is its own value
            top = new Node(val, null, val);
        } else {
            // Compare val with the current min to find the new min
            int newMin = Math.min(val, top.minSoFar);
            Node newNode = new Node(val, top, newMin);
            top = newNode;
        }
    }
    
    public void pop() {
        // The new top becomes the node that was previously pointed to
        top = top.nextNode;
    }
    
    public int top() {
        return top.value;
    }
    
    public int getMin() {
        return top.minSoFar;
    }
}
``` 

### Complexity Analysis
- **Time Complexity:** O(1)
  - Each function (`push`, `pop`, `top`, `getMin`) performs a fixed number of simple operations. The time taken does not change based on the number of elements in the stack.

- **Space Complexity:** O(n)
  - The amount of memory used grows directly with the number of elements (`n`) pushed onto the stack, as each element is stored in its own `Node` object.
