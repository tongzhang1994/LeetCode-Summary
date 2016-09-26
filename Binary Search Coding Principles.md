# *Binary Search Coding Principles*

Binary Search is an efficient `O(logn)` algorithm to find a target element. However, the loop termination condition and the calculation of the middle element are such stuff that makes it tricky for beginners.

Today I'd like to generalize a series of universal principles wich I learnt from somewhere else. 

> When we are writing Binary Search, we should:
> 
> 1. **When shrinking the region, guaruntee that the target element is always in [left,right]**. Then write down the left and right border of the region. (In other words, whether to use mid+-1 or mid as the new left/right border.)
> 
> 2. Determine how to calculate `mid` based on how we shrinked our searching region. **If we wrote `left=mid` then we should use `mid=left+(right-left+1)/2`, otherwise `mid=left+(right-left)/2`.** (Since the later 'mid' rounds down to the left border. When `right=left+1`, `left` itself equals to `mid`. So if we previously wrote `left=mid`, the region wouldn't shrink in this step.)
> 
> 3. Loop termination condition would be either `left<high` or `left<=high` based on Principle 1&2. **If the last element we found when 'left=right' is the target one, then we don't need to check anymore (eliminate '='). Else when we have to check the last element, we should add '='.** The best way is to imagine there is only 2 and 3 elements left and examine this principle.
> 
> 4. The inner-loop returning statement should always be `return mid`. Returning statement outside the loop should always be `return left`.

A simple example: [First Bad Version](https://leetcode.com/problems/first-bad-version/)

```java
	public int firstBadVersion(int n) {
        int low=1,high=n;
        while(low<high){//Principle3:the last element must be badversion, no need to check '='.
            int mid=low+(high-low)/2;//Principle2: since low=mid+1, then we use mid=low+(high-low)/2
            if(isBadVersion(mid))   high=mid;//Principle1
            else    low=mid+1;//Principle1
        }
        return low;//Principle4
    }
```
<br/>
<br/>
---
Reference Link: [问一个关于binary search循环跳出条件的问题](http://www.1point3acres.com/bbs/forum.php?mod=redirect&goto=findpost&ptid=161799&pid=2141418&fromuid=96813)