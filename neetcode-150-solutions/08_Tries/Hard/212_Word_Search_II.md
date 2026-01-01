# <a href="https://leetcode.com/problems/word-search-ii/" target="_blank">212. Word Search II</a>

### Problem Statement
Given an `m x n` board of characters `board` and a list of strings `words`, return *all words on the board*.

Each word must be constructed from letters of sequentially adjacent cells, where adjacent cells are horizontally or vertically neighboring. The same letter cell may not be used more than once in a word.

### Approach
A naive approach would be to run a DFS on the board for every single word in the list.

The optimal approach combines **Backtracking (DFS)** with a **Trie (Prefix Tree)**.

1.  **Build the Trie:** Insert all words from the list into a Trie.
    * **Optimization:** Store the full string in the Trie node where the word ends (`curr.word = "apple"`). This avoids needing to reconstruct the string during the DFS traversal.
2.  **DFS on Board:** Iterate through every cell `(i, j)` in the board.
3.  **Pruning with Trie:**
    * Start the DFS at the Trie root.
    * As we move to a neighbor cell on the board, we simultaneously move to the corresponding child in the Trie.
    * **Crucial Step:** If the Trie node does not have a child corresponding to the letter on the board, we stop immediately (pruning). We don't need to search further down this path because no word in our list starts with this sequence.
4.  **Found a Word:** If we reach a Trie node where `word` is not null, we add it to the result.
    * **Avoid Duplicates:** Immediately set `curr.word = null` after adding it. This ensures that if we encounter the same word again via a different path on the board, we don't add it twice.
5.  **Backtracking:** Mark the current cell as visited (e.g., using `'*'`) before exploring neighbors, and restore the original character after returning.



### Implementation
```java
import java.util.Map;
import java.util.HashMap;
import java.util.List;
import java.util.ArrayList;

class Solution {
    List<String> result = new ArrayList<>();
    int[][] directions = new int[][]{ {-1, 0}, {1,0}, {0, -1}, {0, 1} };
    int n;
    int m;

    public List<String> findWords(char[][] board, String[] words) {
        Trie trie = new Trie();
        for(String word : words){
            trie.insert(word);
        }

        n = board.length;
        m = board[0].length;
        for(int i = 0 ; i < n ; i++){
            for(int j = 0 ; j < m ; j++){
                dfs(board, i, j, trie.root);
            }
        }
        
        return result;
    }

    public void dfs(char[][] board, int i, int j, Node curr){
        if(i < 0 || i >= n || j < 0 || j >= m || !curr.children.containsKey(board[i][j])){
            return;
        }
        
        curr = curr.children.get(board[i][j]);
        char temp = board[i][j];
        board[i][j] = '*';
                
        if(curr.word != null){
            result.add(curr.word);
            curr.word = null;
        }

        for(int[] direction : directions){
            dfs(board, i + direction[0], j + direction[1], curr);
        }

        board[i][j] = temp;
    }
}

class Trie{
    Node root;

    public Trie(){
        root = new Node();
    }

    public void insert(String word){
        Node curr = root;

        for(char letter : word.toCharArray()){
            curr = curr.children.computeIfAbsent(letter, n -> new Node());
        }

        curr.word = word;
    }
}

class Node{
    Map<Character, Node> children;
    String word;

    public Node(){
        children = new HashMap<>();
        word = null;
    }
}
```

### Complexity Analysis
Let `M` and `N` be the dimensions of the board, `K` be the number of words, and `L` be the maximum length of a word.

-   **Time Complexity:** $O(M \cdot N \cdot 4^L + K \cdot L)$
    -   **Building Trie:** Takes $O(K \cdot L)$.
    -   **DFS:** In the worst case, for each of the `M * N` cells, we might explore paths of length `L`. Since there are 4 directions, this is $O(M \cdot N \cdot 4^L)$. However, the Trie effectively prunes this search space significantly in practice.

-   **Space Complexity:** $O(K \cdot L)$
    -   The Trie stores all characters of all words, taking $O(K \cdot L)$ space.
    -   The recursion stack for DFS takes at most $O(L)$ space.
