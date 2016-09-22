# Best Time to Buy and Sell Stock

## [Best Time to Buy and Sell Stock I](https://leetcode.com/problems/best-time-to-buy-and-sell-stock/)

Two method. The first one is to find the current min price then current maxProfit=pirce[i]-min. The second one uses [`Kadane's Algorithm`](https://en.wikipedia.org/wiki/Maximum_subarray_problem) on the 'difference' array.

```java
public int maxProfit(int[] prices) {
    if(prices == null || prices.length < 2) return 0;      
    int maxProfit = 0, minPrice = prices[0];
    
    for(int i = 1; i < prices.length; i++) {
        if(prices[i] > prices[i - 1]) {
            maxProfit = Math.max(maxProfit, prices[i] - minPrice);       
        } else {
             minPrice = Math.min(minPrice, prices[i]);
        }
    }

    return maxProfit;
}
```

The second solution makes sense when the interviewer twists the question slightly by giving the difference array of prices.

>Ex: for {1, 7, 4, 11}, if he gives {0, 6, -3, 7}, you might end up being confused.

Here, the logic is to calculate the difference (maxCur += prices[i] - prices[i-1]) of the original array, and find a contiguous subarray giving maximum profit. If the difference falls below 0, reset it to zero.

`maxCur = current maximum value`

`maxSoFar = maximum value found so far`

```java
	public int maxProfit(int[] prices) {
        int maxCur=0,maxSoFar=0;
        for(int i=1;i<prices.length;i++){
            maxCur=Math.max(0,maxCur+prices[i]-prices[i-1]);
            maxSoFar=Math.max(maxSoFar,maxCur);
        }
        return maxSoFar;
    }
```

## [Best Time to Buy and Sell Stock II](https://leetcode.com/problems/best-time-to-buy-and-sell-stock-ii/)

This problem is not tricky either.

```java
	public int maxProfit(int[] prices) {
        int max=0;
        for(int i=1;i<prices.length;i++)
            max+=Math.max(0,prices[i]-prices[i-1]);
        return max;
    }
```

## [Best Time to Buy and Sell Stock with Cooldown](https://leetcode.com/problems/best-time-to-buy-and-sell-stock-with-cooldown/)

This problem is a little tricky since we have to come up with the best state equation of `DP algorithm`.

I'll share my thoughts on it tomorrow...



```java
	public int maxProfit(int[] prices) {
        int buy=Integer.MIN_VALUE,prevBuy=0,sell=0,prevSell=0;
        for(int p:prices){
            prevBuy=buy;
            buy=Math.max(prevBuy,prevSell-p);
            prevSell=sell;
            sell=Math.max(prevSell,prevBuy+p);
        }
        return sell;//the last transaction must be sell.
    }
```




