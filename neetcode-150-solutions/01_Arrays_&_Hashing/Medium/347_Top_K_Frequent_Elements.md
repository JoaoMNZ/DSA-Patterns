# <a href="https://leetcode.com/problems/top-k-frequent-elements/" target="_blank">347. Top K Frequent Elements</a>

### Problem Statement
Given an integer array `nums` and an integer `k`, return the `k` most frequent elements. You may return the answer in any order.

### Approach
This problem can be solved efficiently using a `HashMap` to count frequencies, followed by a **Bucket Sort** to organize the numbers by their counts.

1.  **Count Frequencies:** First, we loop through the input array and use a `HashMap` to store how many times each number appears.

2.  **Create Frequency Buckets:** Next, we create an array of lists, called `buckets`. The *index* of this array will represent a frequency, and the list at that index will hold all the numbers that appeared that many times. For example, if the number `7` appears 3 times, we will add `7` to the list at `buckets[3]`.

3.  **Populate the Buckets:** We go through our frequency map and place each number into its corresponding bucket based on its count.

4.  **Get the Top K Elements:** Finally, to get the most frequent elements, we iterate through our `buckets` array **backwards** (from the highest possible frequency down to 1). We add the numbers from each bucket to our result list until we have collected `k` elements.

### Implementation
```java
import java.util.Map;
import java.util.HashMap;
import java.util.List;
import java.util.ArrayList;

public class Solution {
    public int[] topKFrequent(int[] nums, int k) {
        Map<Integer, Integer> numToFrequency = new HashMap<>();
        for (int num : nums) {
            numToFrequency.put(num, numToFrequency.getOrDefault(num, 0) + 1);
        }

        List<Integer>[] frequencyBuckets = new List[nums.length + 1];
        for (int i = 0; i < frequencyBuckets.length; i++) {
            frequencyBuckets[i] = new ArrayList<>();
        }

        for (Map.Entry<Integer, Integer> entry : numToFrequency.entrySet()) {
            int number = entry.getKey();
            int frequency = entry.getValue();
            frequencyBuckets[frequency].add(number);
        }

        int[] topK = new int[k];
        int count = 0;
        for (int freq = frequencyBuckets.length - 1; freq > 0 && count < k; freq--) {
            for (int number : frequencyBuckets[freq]) {
                topK[count++] = number;
                if (count == k) break;
            }
        }

        return topK;
    }
}
``` 

### Complexity Analysis
Let `n` be the number of elements in the input array.

-   **Time Complexity:** O(n)
    -   The algorithm consists of several steps that are all linear in time: building the frequency map is O(n), populating the buckets is O(n), and collecting the final results is also O(n). Therefore, the total time complexity is linear.

-   **Space Complexity:** O(n)
    -   In the worst case (e.g., all elements are unique), both the `HashMap` and the `buckets` array will need to store `n` elements. This makes the space complexity linear.
