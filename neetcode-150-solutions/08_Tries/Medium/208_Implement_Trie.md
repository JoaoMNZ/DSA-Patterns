# <a href="https://leetcode.com/problems/implement-trie-prefix-tree/" target="_blank">208. Implement Trie (Prefix Tree)</a>

### Problem Statement
A **Trie** (pronounced as "try") or **prefix tree** is a tree data structure used to efficiently store and retrieve keys in a dataset of strings. There are various applications of this data structure, such as autocomplete and spellchecker.

Implement the `Trie` class:
* `Trie()` Initializes the trie object.
* `void insert(String word)` Inserts the string `word` into the trie.
* `boolean search(String word)` Returns `true` if the string `word` is in the trie (i.e., was previously inserted), and `false` otherwise.
* `boolean startsWith(String prefix)` Returns `true` if there is a previously inserted string `word` that has the prefix `prefix`, and `false` otherwise.

### Approach
A Trie is a specialized tree where each node represents a single character. Unlike a binary tree, a node can have multiple children.

1.  **Node Structure:**
    * **Children:** We use a `Map<Character, Node>` to store links to the next characters. This allows $O(1)$ access to check if a specific character exists as a child.
    * **EndOfWord:** A boolean flag `endOfWord` is crucial. It marks whether the sequence of characters ending at this node represents a complete word inserted into the Trie, or just a prefix of another longer word.

2.  **Insert (`word`):**
    * Start at the `root`.
    * Iterate through each character of the `word`.
    * Check if the current node has a child for that character.
        * If **yes**: Move to that child node.
        * If **no**: Create a new node, link it, and move to it.
    * After processing the last character, mark the final node as `endOfWord = true`.

3.  **Search (`word`):**
    * Start at the `root`.
    * Traverse strictly following the characters of the `word`.
    * If at any point a child node for the required character is missing, return `false`.
    * If we successfully traverse the whole word, check the `endOfWord` flag of the final node. Return `true` only if it is marked as a word end.

4.  **StartsWith (`prefix`):**
    * Logic is identical to `search`.
    * The only difference is the return condition: if we successfully traverse the whole prefix, we immediately return `true`.

### Implementation
```java
import java.util.Map;
import java.util.HashMap;

class Node{
    Map<Character, Node> children;
    boolean endOfWord;

    public Node(){
        children = new HashMap<>();
        endOfWord = false;
    }
}

class Trie {
    Node root;

    public Trie() {
        root = new Node();
    }
    
    public void insert(String word) {
        Node curr = root;
        for(char letter : word.toCharArray()){
            curr = curr.children.computeIfAbsent(letter, n -> new Node());
        }
        curr.endOfWord = true;
    }
    
    public boolean search(String word) {
        Node curr = root;
        for(char letter : word.toCharArray()){
            if(!curr.children.containsKey(letter)){
                return false;
            }
            curr = curr.children.get(letter);
        }
        return curr.endOfWord;
    }
    
    public boolean startsWith(String prefix) {
        Node curr = root;
        for(char letter : prefix.toCharArray()){
            if(!curr.children.containsKey(letter)){
                return false;
            }
            curr = curr.children.get(letter);
        }
        return true;
    }
}

/**
 * Your Trie object will be instantiated and called as such:
 * Trie obj = new Trie();
 * obj.insert(word);
 * boolean param_2 = obj.search(word);
 * boolean param_3 = obj.startsWith(prefix);
 */
``` 

### Complexity Analysis
Let `L` be the length of the string (word or prefix) being processed.

-   **Time Complexity:** $O(L)$
    -   **Insert:** We process each character exactly once.
    -   **Search / StartsWith:** We traverse down the tree one level per character. In the worst case, we check `L` nodes.

-   **Space Complexity:** $O(T)$
    -   Here, `T` is the total number of characters in all words inserted.
    -   In the worst case (no common prefixes), we create a new node for every character in every word.
