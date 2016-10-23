# 2Sum, 3Sum, 4Sum

## [2 Sum](http://oj.leetcode.com/problems/two-sum/)

This is a quite classical problem, the `brute-force ` approach runs in `O(n^2)` time to compare pairs with target one by one. 

However, we can optimize this problem by the following two solutions. 

**1.HashMap**

The first one is using a `HashMap` to memorize the elements appeared and check if the new element can pair with a previous one in the map and add up to 'target'. This algorithm will run in `O(n)` time and `O(n)` space when accessing the hashmap takes constant time.

```java  
public int[] twoSum(int[] nums, int target) {
	HashMap<Integer,Integer> map=new HashMap<>();
	for(int i=0;i<nums.length;i++){
		if(map.containsKey(target-nums[i])) return new int[]{map.get(target-nums[i]),i};
		map.put(nums[i],i);    
	}    
	return new int[]{0,0};
}
```

**2.Two pointers squeezing(not suitable for this problem)**

First we have to sort the array. Then we `squeeze two pointers` from each end of the array to find the target pair.

```java 
public int[] twoSum(int[] nums, int target) {    
    Arrays.sort(nums);  
    int l = 0,r = nums.length-1;  
    while(l<r)    
        if(nums[l]+nums[r]==target)     return new int[]{nums[l],nums[r]};  
        else if(nums[l]+nums[r]>target)    r--;   
        else   l++;    
    return new int[]{0,0};  
}  
```
*NOTE: Here we output the two numbers int the target pair instead of the two indices.*

Since we have to sort the array, in order to output the correct indices we should construct a new data structure and store the number and its original index into it. And then we sort this data structure. So from this perspective, this optimization is not as good as the first one. It takes O(nlogn+n)=`O(nlogn)` time and the space complexity depends on the sorting algorithm.

*In [Two Sum II - Input array is sorted](https://leetcode.com/problems/two-sum-ii-input-array-is-sorted/), we can use this solution and output the indices with only `O(n)` time because the array is sorted.*

>The problem suggest that there is exactly one solution. However the interviewers might ask you to output all possible solutions. In this case we have to consider removing duplicate pairs. [see @3 Sum]


## [3 Sum](https://leetcode.com/problems/3sum/)

This is an extension of `2Sum` and the `brute force` solution takes `O(n^3)` time.

In this problem, there may contain duplicates in the input set, so the `hashmap` solution might not work. It could be easier to skip duplicate triplets when the array is sorted. Thus we should consider sorting the array and use `two pointers`.

```java 
public List<List<Integer>> threeSum(int[] nums) {
        List<List<Integer>> res=new ArrayList<List<Integer>>();
        Arrays.sort(nums);//makes it easier to skip duplicates
        for(int i=0;i<nums.length-2;i++)
            if(i==0||(i>0&&nums[i]!=nums[i-1])){//avoid duplicate pairs in result.
                int l=i+1,h=nums.length-1,sum=0-nums[i];
                while(l<h)
                    if(nums[l]+nums[h]==sum){
                        res.add(Arrays.asList(nums[i],nums[l],nums[h]));
                        while(l<h&&nums[l]==nums[l+1])  l++;//avoid duplicate pairs in result.
                        while(l<h&&nums[h]==nums[h-1])  h--;//avoid duplicate pairs in result.
                        h--;
                        l++;
                    }else if(nums[l]+nums[h]<sum){
                        while(l<h&&nums[l]==nums[l+1])  l++;//improve:avoid duplicate pairs in result.
                        l++;
                    }else{
                        while(l<h&&nums[h]==nums[h-1])  h--;//improve:avoid duplicate pairs in result.
                        h--;
                    }
            }
        return res;
    }
```

You see, sorting the array makes it easier to avoid duplicates by skipping the number we have already checked. The code runs in O(nlogn+n^2)=`O(n^2)` time. Space complexity is `O(n)`.


## [3 Sum Closest](https://leetcode.com/problems/3sum-closest/)

This is also an extension of `2Sum`. And we can't use the `HashMap` approach since it looks for the closest answer instead of the exact one. Similarly we use `two pointers` and this time we should also keep the sum of the triplet closest to 'target'.

```java 
public int threeSumClosest(int[] nums, int target) {
        int res=nums[0]+nums[1]+nums[nums.length-1];
        Arrays.sort(nums);
        for(int i=0;i<nums.length-2;i++){
            int l=i+1,h=nums.length-1;
            while(l<h){
                int sum=nums[i]+nums[l]+nums[h];
                if(sum<=target)    l++;
                else    h--;
                if( Math.abs(sum-target) < Math.abs(res-target) )   res=sum;
            }
        }
        return res;
    }
```
The code runs in O(nlogn+n^2)=`O(n^2)` time. Space complexity is `O(n)`.


## [4 Sum](https://leetcode.com/problems/4sum/)

This problem can be solved in `O(n^3)` apparently. Similar as before, the code is as follows.

Some modifications:

- To avoid duplicate list items, I skip unnecessary indices at two locations:
- one at the end of the outer loop (i-loop)
- the other at the end of the inner loop (j-loop).

To avoid useless computations, the following is kind of critical:

- the function return immediately when nums[i]*4 > target
- the inner loop break immediately when nums[j]*4 < target.

These two lines save quite some time due to the set up of the test cases in OJ.

```java 
    public List<List<Integer>> fourSum(int[] nums, int target) {
        List<List<Integer>> list = new ArrayList<List<Integer>>();
        Arrays.sort(nums);
        int second = 0, third = 0, nexti = 0, nextj = 0;
        for(int i=0, L=nums.length; i<L-3; i++) {
            if(nums[i]<<2 > target) return list; // return immediately
            for(int j=L-1; j>i+2; j--) {
                if(nums[j]<<2 < target) break; // break immediately
                int rem = target-nums[i]-nums[j];
                int lo = i+1, hi=j-1;
                while(lo<hi) {
                    int sum = nums[lo] + nums[hi];
                    if(sum>rem) --hi;
                    else if(sum<rem) ++lo;
                    else {
                        list.add(Arrays.asList(nums[i],nums[lo],nums[hi],nums[j]));
                        while(++lo<=hi && nums[lo-1]==nums[lo]) continue; // avoid duplicate results
                        while(--hi>=lo && nums[hi]==nums[hi+1]) continue; // avoid duplicate results
                    }
                }
                while(j>=1 && nums[j]==nums[j-1]) --j; // skip inner loop
            }
            while(i<L-1 && nums[i]==nums[i+1]) ++i; // skip outer loop
        }
        return list;
    }
```

However, if we think more, we can still optimize this solution.

We can calculate all two-tuples and do `Two Sum` algorithm on all these two-tuples.  Recall that `Two Sum` algorithm is a sort plus a linear iterate. Here we have total O((n-1)+(n-2)+...+1)=O(n(n-1)/2)=`O(n^2)` two-tuples and the sorting for these pairs would run in O(n^2\*log(n^2))=O(n^2\*2logn)=`O(n^2*logn)`, which would be faster than O(n^3).

But to do this, we have to deal with the following details.

First, we need a data structure to store pairs and their indices. This is because we have to examine whether two found pairs which can add up to 'target' are the same one by checking their indices.

Second, we need to define `comparable() method` on 'pairs' since we have to sort these pairs.

Third, we need to define `hashcode()` and `equals()` method on 'Tuples' to store current sum in a `hashset` because we have to check all matched and cannot skip duplicate values as the 2Sum algorithm.

```java 
private ArrayList<ArrayList<Integer>> twoSum(ArrayList<Pair> pairs, int target){  
    HashSet<Tuple> hashSet = new HashSet<Tuple>();  
    int l = 0;  
    int r = pairs.size()-1;  
    ArrayList<ArrayList<Integer>> res = new ArrayList<ArrayList<Integer>>();  
    while(l<r){  
        if(pairs.get(l).getSum()+pairs.get(r).getSum()==target)  
        {  
            int lEnd = l;  
            int rEnd = r;  
            while(lEnd<rEnd && pairs.get(lEnd).getSum()==pairs.get(lEnd+1).getSum())  
            {  
                lEnd++;  
            }  
            while(lEnd<rEnd && pairs.get(rEnd).getSum()==pairs.get(rEnd-1).getSum())  
            {  
                rEnd--;  
            }  
            for(int i=l;i<=lEnd;i++)  
            {  
                for(int j=r;j>=rEnd;j--)  
                {  
                    if(check(pairs.get(i),pairs.get(j)))  
                    {  
                        ArrayList<Integer> item = new ArrayList<Integer>();  
                        item.add(pairs.get(i).nodes[0].value);  
                        item.add(pairs.get(i).nodes[1].value);  
                        item.add(pairs.get(j).nodes[0].value);  
                        item.add(pairs.get(j).nodes[1].value);  
                        //Collections.sort(item);  
                        Tuple tuple = new Tuple(item);  
                        if(!hashSet.contains(tuple))  
                        {  
                            hashSet.add(tuple);  
                            res.add(tuple.num);  
                        }  
                    }                          
                }  
            }  
            l = lEnd+1;  
            r = rEnd-1;  
        }  
        else if(pairs.get(l).getSum()+pairs.get(r).getSum()>target)  
        {  
            r--;  
        }  
        else{  
            l++;  
        }  
    }  
    return res;  
}  
private boolean check(Pair p1, Pair p2)  
{  
    if(p1.nodes[0].index == p2.nodes[0].index || p1.nodes[0].index == p2.nodes[1].index)    return false;  
    if(p1.nodes[1].index == p2.nodes[0].index || p1.nodes[1].index == p2.nodes[1].index)    return false;  
    return true;  
}  
```

[This solution](http://blog.csdn.net/linhuanmars/article/details/24826871) is faster and can be extended to all k Sum problems but would be quite complicated. So this is not very suitable to be asked in an interview. Just remembering the detailed thinking method is enough.

