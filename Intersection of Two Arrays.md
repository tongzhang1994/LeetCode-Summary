# Intersection of Two Arrays

## [Intersection of Two Arrays I](https://leetcode.com/problems/intersection-of-two-arrays/)

1.Use two HashSets, Time complexity: O(n)

```java
	public int[] intersection(int[] nums1, int[] nums2) {
        Set<Integer> set=new HashSet<>();
        Set<Integer> intersect=new HashSet<>();
        for(int n:nums1)    set.add(n);
        for(int n:nums2)
            if(set.contains(n)) intersect.add(n);
        int[] res=new int[intersect.size()];
        int i=0;
        for(Integer n:intersect)    res[i++]=n;
        return res;
    }
```

***NOTICE: The time complexity of HashSet.contains() is [O(1)](http://stackoverflow.com/questions/25247854/hashset-contains-performance).***


2.We can also use the new features in Java 8.

```java
	public int[] intersection(int[] nums1, int[] nums2) {
        Set<Integer> set = Arrays.stream(nums2).boxed().collect(Collectors.toSet());
return Arrays.stream(nums1).distinct().filter(e-> set.contains(e)).toArray();
    }
```


Reference: [Java8初体验](http://ifeve.com/stream/)
<br/>

P.S. There are more solutions to this problem. You can find them at [https://discuss.leetcode.com/topic/45685/three-java-solutions](https://discuss.leetcode.com/topic/45685/three-java-solutions)