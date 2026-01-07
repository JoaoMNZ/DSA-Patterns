# <a href="https://leetcode.com/problems/word-ladder/" target="_blank">127. Word Ladder</a>

### Problem Statement
A **transformation sequence** from word `beginWord` to word `endWord` using a dictionary `wordList` is a sequence of words `beginWord -> s1 -> s2 -> ... -> sk` such that:
1.  Every adjacent pair of words differs by a single letter.
2.  Every `si` for `1 <= i <= k` is in `wordList`. Note that `beginWord` does not need to be in `wordList`.
3.  `sk == endWord`.

Given two words, `beginWord` and `endWord`, and a dictionary `wordList`, return *the number of words in the shortest transformation sequence from `beginWord` to `endWord`, or `0` if no such sequence exists.*

### Approach
We need to find the **shortest path** in an unweighted graph where words are nodes and edges exist between words differing by exactly one character. The standard algorithm for shortest path in an unweighted graph is **Breadth-First Search (BFS)**.

The main challenge is finding neighbors efficiently. Comparing every word pair takes $O(N^2 \cdot L)$, which is too slow.
Your optimized approach uses **Intermediate Patterns**:
1.  **Preprocessing:** For every word (e.g., "hot"), generate all generic patterns by replacing one character with `*`.
2.  **Adjacency List:** Use a `HashMap` to map each pattern to a list of words that match it.
    * e.g., `*ot` -> `[hot, dot, lot]`
3.  **Graph Traversal (BFS):**
    * Start with `beginWord`.
    * For the current word, generate its patterns.
    * Look up the patterns in the Map to find all neighbors.
    * Add unvisited neighbors to the queue and mark them as visited.
    * Repeat level by level until `endWord` is found.



### Implementation
```java
class Solution {
    public int ladderLength(String beginWord, String endWord, List<String> wordList) {
        if (!wordList.contains(endWord)) 
            return 0;
        
        wordList.add(beginWord);

        Map<String, List<String>> patterns = new HashMap<>();
        for(String word : wordList){
            StringBuilder sb = new StringBuilder(word);
            for(int pos = 0 ; pos < word.length() ; pos++){
                sb.setCharAt(pos, '*');
                patterns.computeIfAbsent(sb.toString() , n -> new ArrayList<>()).add(word);
                sb.setCharAt(pos, word.charAt(pos));
            }
        }
        
        Set<String> visit = new HashSet<>();
        Deque<String> queue = new ArrayDeque<>();
        queue.add(beginWord);
        visit.add(beginWord);

        int res = 0;
        while(!queue.isEmpty()){
            res++;
            int size = queue.size();
            for(int i = 0 ; i < size ; i++){
                String u = queue.poll();

                if(u.equals(endWord)){
                    return res;
                }

                StringBuilder sb = new StringBuilder(u);
                for(int pos = 0 ; pos < u.length() ; pos++){
                    sb.setCharAt(pos, '*');
                    for(String v : patterns.getOrDefault(sb.toString(), Collections.emptyList())){
                        if(!visit.contains(v)){
                            queue.addLast(v);
                            visit.add(v);
                        }
                    }
                    sb.setCharAt(pos, u.charAt(pos));
                }
            }
        }

        return 0;
    }
}
``` 

### Complexity Analysis
Let `N` be the number of words in `wordList` and `L` be the length of each word.

-   **Time Complexity:** $O(N \cdot L^2)$
    -   **Preprocessing:** We iterate through `N` words. For each word, we generate `L` patterns. Creating each pattern string takes $O(L)$ (hashing/copying). Total: $O(N \cdot L^2)$.
    -   **BFS:** In the worst case, we visit every word once. For each word, we generate `L` patterns ($O(L^2)$). We then iterate over neighbors. Since the number of edges is bounded by the pattern generation logic, the total BFS time is also bounded by $O(N \cdot L^2)$.

-   **Space Complexity:** $O(N \cdot L^2)$
    -   The `patterns` map stores `N` words. Each word generates `L` keys. Each key takes `L` space. Thus, the map stores roughly $N \cdot L$ patterns of length $L$, resulting in $O(N \cdot L^2)$.
