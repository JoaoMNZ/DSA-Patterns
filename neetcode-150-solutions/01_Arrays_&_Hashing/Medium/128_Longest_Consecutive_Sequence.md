# <a href="https://leetcode.com/problems/longest-consecutive-sequence/" target="_blank">128. Longest Consecutive Sequence</a>

### Problem Statement
Given an unsorted array of integers `nums`, return the length of the longest consecutive elements sequence.

You must write an algorithm that runs in `O(n)` time.

### Approach
A brute-force solution would be too slow. A more efficient approach uses a `HashSet` for fast lookups, combined with a clever way to avoid redundant work.

The key insight is that we only need to count a sequence when we find its **starting number**. A number `x` is the start of a sequence if `x - 1` does not exist in the array.

1.  First, we add all numbers from the input array into a `HashSet` to allow for quick `O(1)` checks.
2.  We then iterate through each number in our set.
3.  For each number, we check if it is the start of a sequence by seeing if the set contains `number - 1`.
    - If it does, we ignore this number and move to the next, because it's part of a sequence we've already counted or will count later.
    - If it does **not**, we've found the start of a new sequence. We then use a simple loop to count how long this sequence is by checking for `number + 1`, `number + 2`, and so on, until the next number is not found.
4.  We keep track of the longest sequence found so far and return it after checking all the numbers.

### Implementation
```java
import java.util.HashSet;
import java.util.Set;

class Solution {
    public int longestConsecutive(int[] nums) {
        Set<Integer> numbers = new HashSet<>();
        for (int num : nums) {
            numbers.add(num);
        }
        
        int longestStreak = 0;
        
        for (int current : numbers) {
            if (!numbers.contains(current - 1)) {
                int streakLength = 1;
                
                while (numbers.contains(current + streakLength)) {
                    streakLength++;
                }
                
                longestStreak = Math.max(longestStreak, streakLength);
            }
        }
        
        return longestStreak;
    }
}
``` 

### Complexity Analysis
- **Time Complexity:** O(n)
  - The first loop to build the `HashSet` is O(n). The second `for` loop seems like it could be O(n²), but it's not. The inner `while` loop only executes for numbers that are the start of a sequence. This means that each number in the array is visited by the `while` loop at most once across all iterations. Therefore, the total time complexity remains O(n).

- **Space Complexity:** O(n)
  - We store all `n` numbers from the input array in a `HashSet`. The space required grows linearly with the size of the input.
