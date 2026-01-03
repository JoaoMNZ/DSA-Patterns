# <a href="https://leetcode.com/problems/design-twitter/" target="_blank">355. Design Twitter</a>

### Problem Statement
Design a simplified version of Twitter where users can post tweets, follow/unfollow another user, and is able to see the `10` most recent tweets in the user's news feed.

Implement the `Twitter` class:
* `Twitter()` Initializes your twitter object.
* `void postTweet(int userId, int tweetId)` Composes a new tweet with ID `tweetId` by the user `userId`. Each call to this function will be made with a unique `tweetId`.
* `List<Integer> getNewsFeed(int userId)` Retrieves the `10` most recent tweet IDs in the user's news feed. Each item in the news feed must be posted by users who the user followed or by the user themself. Tweets must be ordered from most recent to least recent.
* `void follow(int followerId, int followeeId)` The user with ID `followerId` started following the user with ID `followeeId`.
* `void unfollow(int followerId, int followeeId)` The user with ID `followerId` started unfollowing the user with ID `followeeId`.

### Approach
1.  **Data Structures:**
    * `tweetMap`: A HashMap mapping `userId` to a `Deque` of tweets.
        * **Optimization:** We only store the **10 most recent tweets** for each user. Older tweets are irrelevant for the news feed requirement, so we discard them. This keeps memory usage low and retrieval fast.
    * `followMap`: A HashMap mapping `userId` to a `HashSet` of `followeeId`s.
    * `time`: A global counter to assign a timestamp to each tweet, ensuring strict ordering.

2.  **Logic:**
    * **Post Tweet:** Create the user if necessary. Add the tweet `{id, time}` to the front of their deque. If the deque size exceeds 10, remove the oldest tweet.
    * **Follow/Unfollow:** Update the `HashSet` in `followMap`.
    * **Get News Feed:**
        * Collect all tweets from the user and everyone they follow.
        * Since each user only has 10 tweets stored, we iterate through `10 * (num_followees + 1)` tweets max.
        * Use a **Min-Heap** (PriorityQueue) of size 10 to keep track of the 10 most recent tweets seen so far. If the heap grows larger than 10, we remove the oldest (smallest time) tweet.
        * Finally, reverse the heap elements to return them from newest to oldest.

### Implementation
```java
class Twitter {
    Map<Integer, Deque<int[]>> tweetMap;
    Map<Integer, Set<Integer>> followMap;
    int time;

    public Twitter() {
        tweetMap = new HashMap<>();
        followMap = new HashMap<>();
        time = 0;
    }

    public void createUser(int userId){
        if(!tweetMap.containsKey(userId)){
            tweetMap.put(userId, new ArrayDeque<>());
            followMap.put(userId, new HashSet<>());
        }
    }
    
    public void postTweet(int userId, int tweetId) {
        createUser(userId);

        Deque<int[]> userTweets = tweetMap.get(userId);
        userTweets.addLast(new int[]{tweetId, time});
        time++;

        if(userTweets.size() > 10){
            userTweets.poll();
        }
    }

    public void follow(int followerId, int followeeId) {
        if(followerId == followeeId){
            return;
        }

        createUser(followerId);
        createUser(followeeId);

        Set<Integer> userFollows = followMap.get(followerId);
        userFollows.add(followeeId);
    }
    
    public void unfollow(int followerId, int followeeId) {
        createUser(followerId);

        Set<Integer> userFollows = followMap.get(followerId);
        userFollows.remove(followeeId);
    }
    
    public List<Integer> getNewsFeed(int userId) {
        createUser(userId);

        Queue<int[]> minHeap = new PriorityQueue<>(Comparator.comparing(t -> t[1]));
        Set<Integer> userFollows = followMap.get(userId);
        userFollows.add(userId);

        for(int followeeId : userFollows){
            Deque<int[]> followeeTweets = tweetMap.get(followeeId);
            for(int[] tweet : followeeTweets){
                minHeap.offer(tweet);
                if(minHeap.size() > 10){
                    minHeap.poll();
                }
            }
        }

        List<Integer> feed = new ArrayList<>();
        while(!minHeap.isEmpty()){
            feed.addFirst(minHeap.poll()[0]);
        }

        userFollows.remove(userId);
        return feed;
    }
}

/**
 * Your Twitter object will be instantiated and called as such:
 * Twitter obj = new Twitter();
 * obj.postTweet(userId,tweetId);
 * List<Integer> param_2 = obj.getNewsFeed(userId);
 * obj.follow(followerId,followeeId);
 * obj.unfollow(followerId,followeeId);
 */
```

### Complexity Analysis
Let `F` be the number of people a user follows.

-   **Time Complexity:**
    -   `postTweet`: $O(1)$ (deque operations are constant).
    -   `follow` / `unfollow`: $O(1)$ (HashSet operations).
    -   `getNewsFeed`: $O(F)$. We iterate through `F + 1` users. For each user, we process at most 10 tweets. The heap operations take $O(\log 10)$ which is constant. Thus, the complexity is linear relative to the number of followees.

-   **Space Complexity:** $O(U + T)$
    -   Where `U` is the number of users and `T` is the number of tweets. However, since we cap tweets at 10 per user, the space for tweets is effectively $O(U)$.
