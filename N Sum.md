# 2Sum, 3Sum, 4Sum

## [2Sum](http://oj.leetcode.com/problems/two-sum/)

This is a quite classical problem, the `brute-force ` approach runs in `O(n^2)` time to compare pairs with target one by one. 

However, we can optimize this problem by the following two solutions. 

**1.HashMap**

The first one is to use a `HashMap`, which runs in `O(n)` time and `O(n)` space.

```  
public int[] twoSum(int[] nums, int target) {
	HashMap<Integer,Integer> map=new HashMap<>();
	for(int i=0;i<nums.length;i++){
		if(map.containsKey(target-nums[i])) return new int[]{map.get(target-nums[i]),i};
		map.put(nums[i],i);    
	}    
	return new int[]{0,0};
}
```

**2.Two pointers squeezing**

First we have to sort the array. Then we `squeeze two pointers` from each end of the array to find the target pair.

```
public int[] twoSum(int[] numbers, int target) {    
    Arrays.sort(numbers);  
    int l = 0,r = numbers.length-1;  
    while(l<r)    
        if(numbers[l]+numbers[r]==target) 
            return new int[]{nums[l],nums[r]};  
        else if(numbers[l]+numbers[r]>target)    r--;   
        else   l++;    
    return new int[]{0,0};  
}  
```
*NOTE: Here we output the two numbers int the target pair instead of the two indices.* 

Since we have to sort the array, in order to output the correct indices we should construct a new data structure and store the number and its original index into it. And then we sort this data structure. So from this perspective, this optimization is not as good as the first one. It takes `O(nlogn+n)=Q(nlogn)` time and the space complexity depends on the sorting algorithm.




