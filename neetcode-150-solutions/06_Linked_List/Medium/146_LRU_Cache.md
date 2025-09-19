# <a href="https://leetcode.com/problems/lru-cache/" target="_blank">146. LRU Cache</a>

### Problem Statement
Design a data structure that follows the constraints of a **L**east **R**ecently **U**sed (LRU) cache. The functions `get` and `put` must each run in `O(1)` average time complexity.

- `LRUCache(int capacity)`: Initializes the cache with a positive size `capacity`.
- `int get(int key)`: Returns the value of the key if it exists, otherwise returns -1.
- `void put(int key, int value)`: Updates the key's value if it exists. Otherwise, adds the key-value pair. If the cache exceeds capacity, evicts the least recently used key.

### Approach
The `O(1)` time complexity requirement for both `get` and `put` guides our choice of data structures.

1.  **For Fast Lookups:** The `get` operation needs to be `O(1)`. This immediately points to using a **`HashMap`**, which allows us to find any value by its key in constant time on average.

2.  **For Tracking Usage Order:** A `HashMap` alone doesn't keep track of which items were used recently. We need a second data structure to manage the "Least Recently Used" (LRU) and "Most Recently Used" (MRU) logic. An array or a standard list would be too slow, as moving an element to the front (to mark it as MRU) would take `O(n)` time.

The perfect structure for this is a **Doubly Linked List**. It allows us to add or remove a node in `O(1)` time, provided we have a reference to that node.

**Combining Both:**
We use both structures together. The `HashMap` stores the `key` and a direct reference to the `Node` in the Doubly Linked List. The list itself organizes the nodes by usage:
-   The **head** of the list is the **Most Recently Used** item.
-   The **tail** of the list is the **Least Recently Used** item.

When an item is accessed (`get`) or updated (`put`), we use the map to find it in `O(1)` and then move its node to the head of the list in `O(1)`, making it the new MRU. When the cache is full, we know the LRU item is at the tail, so we can remove it in `O(1)`.

### Implementation
```java
import java.util.Map;
import java.util.HashMap;

/**
 * Your LRUCache object will be instantiated and called as such:
 * LRUCache obj = new LRUCache(capacity);
 * int param_1 = obj.get(key);
 * obj.put(key,value);
 */

class LRUCache {
    private Map<Integer, Node> map;
    private DoublyLinkedList list;
    private int capacity;
    
    public LRUCache(int capacity) {
        map = new HashMap<>(capacity);
        list = new DoublyLinkedList();
        this.capacity = capacity;
    }
    
    public int get(int key) {
        Node node = map.get(key);
        if(node == null){
            return -1;
        }
        list.moveNodeToHead(node);
        return node.value;
    }
    
    public void put(int key, int value) {
        Node node = map.get(key);
        if(node == null){
            Node newNode = new Node(key, value);
            if(map.size() == capacity){
                int keyToDelete = list.removeNodeAtTail();
                map.remove(keyToDelete);
            }
            list.insertNodeAtHead(newNode);
            map.put(key, newNode);
        }else{
            node.value = value;
            list.moveNodeToHead(node);
        }
    }
}

class DoublyLinkedList {
    public Node head;
    public Node tail;

    public DoublyLinkedList(){

    }   

    public void insertNodeAtHead(Node node){
        node.next = head;
        if(head != null) {
            head.prev = node;
        }
        head = node;
        if(tail == null) {
            tail = node;
        }
    }

    public int removeNodeAtTail(){
        Node prev = tail.prev;
        if(prev == null){
            head = null;
        }else{
            prev.next = null;
        }
        int keyToRemove = tail.key;
        tail = prev;
        return keyToRemove;
    }

    public void moveNodeToHead(Node node){
        if(node == head){
            return;
        }
        Node prev = node.prev;
        Node next = node.next;
        if(prev != null){
            prev.next = next;
        }
        if(next != null){
            next.prev = prev;
        }
        if(node == tail){
            tail = prev;
        }
        head.prev = node;
        node.next = head;
        node.prev = null;
        head = node;
    }
}

class Node {
    public int key;
    public int value;
    public Node prev;
    public Node next;

    public Node(int key, int value) {
        this.key = key;
        this.value = value;
    }
}
``` 

### Complexity Analysis
-   **Time Complexity:** O(1)
    -   Both `get` and `put` functions consist of a `HashMap` access (O(1) on average) and a few pointer updates in the Doubly Linked List, which are also O(1) operations.

-   **Space Complexity:** O(n)
    -   Let `n` be the capacity of the cache. The `HashMap` and the Doubly Linked List will store up to `n` nodes, making the space complexity linear with respect to the capacity.
