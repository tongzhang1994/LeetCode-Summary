# Contains Duplicate

## [Contains Duplicate I](https://leetcode.com/problems/contains-duplicate/)

```java
	public boolean containsDuplicate(int[] nums) {
        Set<Integer> res=new HashSet<>();
        for(int n:nums)
            if(!res.add(n))    return true;
        return false;
    }
```

The logic is simple. But, 

*NOTICE that we used `if(!res.add(n))` to check whether the 'n' is already in the set instead of `set.contains(n)`.*

Because **`res.add(n)` returns a `boolean` type, it would get a slight performance improvement over using `set.contains()`.**

> As a bonus in `Java 8`, using streams is O(n). For those who are not familiar with Java 8, please refer to [this Chinese webpage](http://ifeve.com/stream/), or you can google it yourself.

```java
public boolean containsDuplicate(int[] nums) {
    Set<Integer> seen = new HashSet<>();
    return Arrays.stream(nums).anyMatch(num -> !seen.add(num));
}
```
A more concise version is to only use the following line of code. But it gets a TLE error in Leetcode. And I can't find out the reason yet. T_T
`return nums.length != Arrays.stream(nums).distinct().count();`


## [Contains Duplicate II](https://leetcode.com/problems/contains-duplicate-ii/)

A less efficient way is to use HashMap. 

But here we just use a HashSet and a sliding window. It is the `sliding window` logic that is more space efficient than plain hash map method. Running time complexity is similar anyways. 

```java
	public boolean containsNearbyDuplicate(int[] nums, int k) {
        //sliding window
        Set<Integer> set=new HashSet<Integer>();
        for(int i=0;i<nums.length;i++){
            if(i>k) set.remove(nums[i-k-1]);//magic as it is.
            if(!set.add(nums[i]))   return true;
        }
        return false;
    }
```






