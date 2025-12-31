# <a href="https://leetcode.com/problems/design-add-and-search-words-data-structure/" target="_blank">211. Design Add and Search Words Data Structure</a>

### Problem Statement
Design a data structure that supports adding new words and finding if a string matches any previously added string.

Implement the `WordDictionary` class:
* `WordDictionary()` Initializes the object.
* `void addWord(word)` Adds `word` to the data structure, it can be matched later.
* `bool search(word)` Returns `true` if there is any string in the data structure that matches `word` or `false` otherwise. `word` may contain dots `'.'` where dots can be matched with any letter.

### Approach
This problem is an extension of the standard **Trie (Prefix Tree)**. The `addWord` operation is identical to a standard Trie insertion. The core challenge is the `search` operation, specifically handling the `'.'` wildcard.



1.  **Data Structure:** We use the standard Trie node structure with a `children` map and an `endOfWord` boolean.
2.  **Search Logic (DFS):**
    * We iterate through the characters of the word.
    * **Regular Character:** If the character is a specific letter (e.g., 'a'), we simply check if the current node has that specific child. If not, the search fails immediately.
    * **Wildcard (`.`):** If the character is a dot, it can match **any** of the current node's children. We must attempt to traverse **all** existing branches from the current node.
        * We use **Recursion (DFS)** to try every child node.
        * If *any* of these recursive calls return `true`, then the match is successful.
        * If we try all children and none lead to a match, the current path fails.

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

class WordDictionary {
    Node root;

    public WordDictionary() {
        root = new Node();
    }
    
    public void addWord(String word) {
        Node curr = root;
        for(char letter : word.toCharArray()){
            curr = curr.children.computeIfAbsent(letter, n -> new Node());
        }
        curr.endOfWord = true;
    }
    
    public boolean search(String word) {
        return dfs(root, word, 0);
    }

    public boolean dfs(Node curr, String word, int start){
        for(int i = start ; i < word.length() ; i++){
            char letter = word.charAt(i);
            if(letter == '.'){
                for(Node child : curr.children.values()){
                    if(dfs(child, word, i + 1)){
                        return true;
                    }
                }
                return false;
            }else{
                if(!curr.children.containsKey(letter)){
                    return false;
                }
                curr = curr.children.get(letter);
            }
        }

        return curr.endOfWord;
    }
}

/**
 * Your WordDictionary object will be instantiated and called as such:
 * WordDictionary obj = new WordDictionary();
 * obj.addWord(word);
 * boolean param_2 = obj.search(word);
 */
``` 

### Complexity Analysis
Let `L` be the length of the word and `M` be the size of the alphabet (26 for English).

-   **Time Complexity:**
    -   **AddWord:** $O(L)$. We traverse down the tree one level per character.
    -   **Search:**
        -   **Best Case (No dots):** $O(L)$. Acts like a standard Trie search.
        -   **Worst Case (All dots):** $O(M^L)$. If the word consists entirely of dots (e.g., `"..."`), we must explore every single branch of the Trie. This effectively visits every node in the data structure.

-   **Space Complexity:** $O(T)$
    -   Where `T` is the total number of characters in all inserted words. The DFS recursion stack for search takes $O(L)$ space.
