# <a href="https://leetcode.com/problems/best-time-to-buy-and-sell-stock/" target="_blank">121. Best Time to Buy and Sell Stock</a>

### Problem Statement
You are given an array `prices` where `prices[i]` is the price of a given stock on the `i`th day. You want to maximize your profit by choosing a **single day** to buy one stock and choosing a **different day in the future** to sell that stock.

Return the maximum profit you can achieve from this transaction. If you cannot achieve any profit, return 0.

### Approach
This problem can be solved efficiently using a **Sliding Window** approach.

1.  We use two pointers: `left` (representing the buy day) and `right` (representing the sell day). We initialize `left` to day 0 and `right` to day 1.
2.  We iterate through the prices with the `right` pointer.
3.  In each iteration, we calculate the potential profit: `profit = prices[right] - prices[left]`.
4.  We update our `maxProfit` if the current `profit` is greater.
5.  **Crucially:** If we find a day where the price `prices[right]` is *lower* than our current buy price `prices[left]`, it means we've found a potentially better day to buy. We update our `left` pointer to this new `right` index. This effectively slides our "buy" window forward to the lowest price encountered so far.
6.  We continue moving the `right` pointer until we reach the end of the prices.

### Implementation
```java
class Solution {
    public int maxProfit(int[] prices) {
        int maxProfit = 0;
        int left = 0;
        
        for (int right = 1; right < prices.length; right++) {
            if (prices[right] > prices[left]) {
                maxProfit = Math.max(maxProfit, prices[right] - prices[left]);
            } else {
                left = right;
            }
        }
        
        return maxProfit;
    }
}
``` 

### Complexity Analysis
Let `n` be the number of days (length of the `prices` array).

-   **Time Complexity:** O(n)
    -   We iterate through the `prices` array exactly once with the `right` pointer.

-   **Space Complexity:** O(1)
    -   We only use a few variables (`maxProfit`, `left`, `right`) to keep track of the state. The space required is constant.
