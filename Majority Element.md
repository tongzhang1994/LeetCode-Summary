# Majority Element

## [Majority Element](https://leetcode.com/problems/majority-element/)

There are two ways to find the `majority element (which appears more than ⌊ n/2 ⌋ times)` in an array. One is simply sort the array and return the middle element, which takes O(nlogn) time. The other one is the famous [`Boyer-Moore Majority Vote Algorithm`](http://www.cs.utexas.edu/~moore/best-ideas/mjrty/index.html), which takes linear time. 

```java
	public int majorityElement(int[] nums) {
        Arrays.sort(nums);//O(nlogn)
        return nums[nums.length/2];
    }
```

Please refer to the link above before you see the following code.

```java
	public int majorityElement(int[] nums) {
        //O(n) Boyer-Moore Majority Vote Algorithm
        int count=1,maj=nums[0];
        for(int i=1;i<nums.length;i++)
            if(count==0){
                maj=nums[i];
                count++;
            }else if(maj==nums[i])  count++;
            else    count--;
        return maj;
    }
```

> *NOTE: Boyer-Moore Majority Vote Algorithm does not garruntee a majority. But* ***if you must confirm that the chosen element is indeed the majority element, you must take one more linear pass through the data to do it***.