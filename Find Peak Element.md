# [Find Peak Element](https://leetcode.com/problems/find-peak-element/)

The problem discription says that 'Your solution should be in logarithmic complexity'. This explicitly indicates a Binary Search Algorithm, which has `O(logn)` complexity.

NOTE: `A peak element is an element that is greater than its neighbors.` If the last element we found has only one neighber, namely the first or last element of the initial array, then it is the peak. (the array is in increasing/decreasing order)

```java
	public int findPeakElement(int[] nums) {
        int low=0,high=nums.length-1;
        while(low<high){
            int mid=low+(high-low)/2;
            if(nums[mid]>nums[mid+1])   high=mid;
            else    low=mid+1;
        }
        return low;
    }
```

Then you may ask that why is using binary searching on unsorted array reasonable in this problem. That is because the problem said that `The array may contain multiple peaks, in that case return the index to any one of the peaks is fine.` Thus, we can just **use BinarySearch to shrink confirmed searching region** iterately and finally get the result.

> In a nutshell, Binary Search can be applied to the following 2 senarios.
> 
> 1. find a target element in a sorted array.
> 
> 2. there is a function f(x), find a (the first) element x such that f(x) is true.


---
Reference Link: [问一个关于binary search循环跳出条件的问题](http://www.1point3acres.com/bbs/forum.php?mod=redirect&goto=findpost&ptid=161799&pid=2139522&fromuid=96813)