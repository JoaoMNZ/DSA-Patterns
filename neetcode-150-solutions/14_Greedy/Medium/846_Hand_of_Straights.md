# <a href="https://leetcode.com/problems/hand-of-straights/" target="_blank">846. Hand of Straights</a>

### Problem Statement
Alice has some number of cards and she wants to rearrange the cards into groups so that each group is of size `groupSize`, and consists of `groupSize` consecutive cards.

Given an integer array `hand` where `hand[i]` is the value written on the $i$-th card and an integer `groupSize`, return `true` if she can rearrange the cards, or `false` otherwise.

### Approach
To determine if we can form the required consecutive groups, we can use a **Greedy Strategy** facilitated by a `TreeMap`.

1.  **Frequency Map & Sorting:** We use a `TreeMap` to store the frequency of each card. The `TreeMap` is essential because it automatically keeps the keys (card values) sorted, allowing us to efficiently access the **smallest available card**.

2.  **Greedy Group Formation:**
    * We verify if the total number of cards is divisible by `groupSize`. If not, return `false` immediately.
    * While the map is not empty, we pick the smallest card currently available (`start = counts.firstKey()`).
    * **Why start with the smallest?** The smallest card *cannot* be the 2nd, 3rd, or $k$-th card of any consecutive sequence; it *must* be the start of a sequence.
    * Let `count` be the frequency of this smallest card. We must attempt to form exactly `count` groups starting with this number.
    * We check the next `groupSize - 1` consecutive numbers. For each required number, we verify if it exists in the map and if its frequency is sufficient (at least `count`). We reduce their frequency by `count`.
    * If at any point a required card is missing or has insufficient count, it's impossible to form the groups, so we return `false`.

### Implementation
```java
class Solution {
    public boolean isNStraightHand(int[] hand, int groupSize) {
        if (hand.length % groupSize != 0) return false;

        TreeMap<Integer, Integer> counts = new TreeMap<>();
        for(int card : hand){
            counts.put(card, counts.getOrDefault(card, 0) + 1);
        }

        while(!counts.isEmpty()){
            int start = counts.firstKey();
            int count = counts.get(start);

            counts.remove(start);
            for(int i = start + 1 ; i < start + groupSize ; i++){
                Integer iCount = counts.get(i);
                if(iCount == null || iCount < count){
                    return false;
                }
                if(iCount == count){
                    counts.remove(i);
                }else{
                    counts.put(i, iCount - count);
                }
            }
        }

        return true;
    }
}
```

### Complexity Analysis
Let `N` be the number of cards in the `hand` array, and `M` be the number of unique cards.

-   **Time Complexity:** $O(N \log M)$
    -   Building the frequency map takes $O(N \log M)$ because we iterate through `N` cards, and each insertion into the `TreeMap` takes $O(\log M)$ time.
    -   During the main logic, we process the cards to form groups. In the worst case, every card counts towards an operation (checking existence, updating count, or removing) involving the map. Since map operations take $O(\log M)$, the processing step also effectively takes $O(N \log M)$.

-   **Space Complexity:** $O(M)$
    -   We use a `TreeMap` to store the counts of unique card values. In the worst case, if all cards are unique, this would be $O(N)$, but strictly speaking, it depends on the number of unique elements `M`.
