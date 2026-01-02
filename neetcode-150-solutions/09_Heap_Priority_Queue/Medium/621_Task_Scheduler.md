# <a href="https://leetcode.com/problems/task-scheduler/" target="_blank">621. Task Scheduler</a>

### Problem Statement
Given a characters array `tasks`, representing the tasks a CPU needs to do, where each letter represents a different task. Tasks could be done in any order. Each task is done in one unit of time. For each unit of time, the CPU could complete either one task or just be idle.

However, there is a non-negative integer `n` that represents the cooldown period between two **same tasks** (the same letter in the array), that is that there must be at least `n` units of time between any two same tasks.

Return *the least number of units of times that the CPU will take to finish all the given tasks*.

### Approach
This is a **Greedy** problem. To minimize total time (and idle slots), we should always prioritize the task that has the highest remaining frequency.
* **Proof Intuition:** If we schedule less frequent tasks first, we might end up with a pile of the most frequent task at the end, forcing us to insert many idle slots between them. By handling the most frequent ones early, we can use the "gaps" (cooldowns) to process other less frequent tasks.

**Algorithm:**
1.  **Count Frequencies:** Calculate how many times each task appears.
2.  **Max-Heap:** Add all non-zero frequencies to a Max-Heap. This allows us to always extract the task with the highest remaining count.
3.  **Queue for Cooldown:** We use a Queue to track tasks that are currently "cooling down". Each entry in the queue stores `{remaining_frequency, time_when_available}`.
4.  **Simulation:**
    * Increment `time`.
    * **Process Task:** If the heap is not empty, poll the most frequent task. Decrement its count. If it still has occurrences left, calculate when it can be processed again (`time + n`) and add it to the cooldown Queue.
    * **Idle (Fast-Forward):** If the heap is empty but the queue is not (meaning all remaining tasks are cooling down), we don't need to increment time one by one. We can jump `time` directly to the `time_when_available` of the first task in the queue.
    * **Recover Task:** Check the front of the Queue. If a task's cooldown has expired (`time_when_available == time`), remove it from the Queue and add it back to the Max-Heap.

### Implementation
```java
import java.util.Queue;
import java.util.PriorityQueue;
import java.util.Deque;
import java.util.ArrayDeque;
import java.util.Collections;

class Solution {
    public int leastInterval(char[] tasks, int n) {
        int[] frequencies = new int[26];
        for(char task : tasks){
            frequencies[task - 'A']++;
        }

        Queue<Integer> maxHeap = new PriorityQueue(Collections.reverseOrder());
        for(int frequency : frequencies){
            if(frequency > 0){
                maxHeap.offer(frequency);
            }
        }

        Deque<int[]> queue = new ArrayDeque<>();
        int time = 0;

        while(!queue.isEmpty() || !maxHeap.isEmpty()){
            time++;

            if(!maxHeap.isEmpty()){
                int frequency = maxHeap.poll() - 1;
                if(frequency > 0){
                    queue.addLast(new int[]{frequency, time + n});
                }
            }else{
                time = queue.peekFirst()[1];
            }

            if(!queue.isEmpty() && queue.peekFirst()[1] == time){
                maxHeap.offer(queue.poll()[0]);
            }
        }

        return time;
    }
}
``` 

### Complexity Analysis
Let `N` be the total number of tasks and `K` be the number of unique tasks (at most 26).

-   **Time Complexity:** $O(N)$
    -   Counting frequencies takes $O(N)$.
    -   The simulation loop runs for the total units of time. In the worst case (lots of idle time), this might seem larger, but heap operations are bounded by constant time $O(\log 26) \approx O(1)$. The number of heap/queue operations is proportional to the number of task executions `N`.

-   **Space Complexity:** $O(1)$
    -   The frequency array is size 26.
    -   The Heap and Queue store at most 26 elements
