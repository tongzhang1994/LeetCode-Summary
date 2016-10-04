# Heap

## [K-th Smallest Element in a Sorted Matrix](https://leetcode.com/problems/kth-smallest-element-in-a-sorted-matrix/)

Inspired by [this post](https://discuss.leetcode.com/topic/52948/share-my-thoughts-and-clean-java-code), the following problem in my class CSCI 570 came to my mind.

> In the MERGE-SORT algorithm we merge two sorted lists into one sorted list in O(n) time. Describe an O(n log k)-time algorithm to merge k sorted lists into one sorted list, where n is the total number of elements in all the input lists. Be sure to explain why your algorithm runs in O(n log k)-time.
> <br/><br/>
> **SOLUTION:**
> 
> First we remove the smallest element from each sorted list and we build a min-priority queue(using a min-heap) out of these elements in O(k) time. Then we repeat the following steps: weextract the minimum from the min-priority queue (in O(log k) time) and this will be the nextelement in the sorted order. From the original sorted list where this element came from weremove the next smallest element (if it exists) and insert it to the min-priority queue (in O(log k)time) . We are done when the queue becomes empty and at this point we have all the numbersin our sorted list. The total running time is O(k + n log k) = O(n log k).

I think this leetcode problem is a variant of the problem above.

Similarly, we can use a min-heap implemented by `PriorityQueue`(PQ). In detail, we need a Data Structure `Tuple` to store elements, which contains the row/column number and value of the element.

First we insert all `Tuples` in the 1st row to the heap. Then repeat the following steps `k-1` times: 

1.Extract the min element in PQ. 

2.From the original `row` in sorted matrix where this element came from we remove the next element (if it exists) and insert it to PQ. 

Return the element we found.

```java
	public int kthSmallest(int[][] matrix, int k) {
        PriorityQueue<Tuple> pq=new PriorityQueue<>();
        for(int i=0;i<matrix.length;i++)    pq.offer(new Tuple(i,0,matrix[i][0]));
        for(int j=0;j<k-1;j++){
            Tuple t=pq.poll();
            if(t.j!=matrix[0].length-1)
                pq.offer(new Tuple(t.i,t.j+1,matrix[t.i][t.j+1]));
        }
        return pq.poll().val;
    }
    
    class Tuple implements Comparable<Tuple>{
        int i,j,val;
        public Tuple(int i,int j,int val){
            this.i=i;
            this.j=j;
            this.val=val;
        }
        public int compareTo(Tuple that){
            return this.val-that.val;
        }
    }
```

TIPS: Pay attention that (from [Java API](http://docs.oracle.com/javase/8/docs/api/java/util/PriorityQueue.html))

>the elements of `PriorityQueue` data structure are ordered according to their `natural ordering`, or by a `Comparator` provided at queue construction time, depending on which constructor is used. A priority queue does not permit null elements. A priority queue relying on natural ordering also does not permit insertion of non-comparable objects (doing so may result in ClassCastException).

ALSO, 'The head of this queue is the least element with respect to the specified ordering.' This means that whether the heap is a minHeap or maxHeap depends on the `ordering` we defined in `Comparable` or `Comparator`.


## [Find K Pairs with Smallest Sums](https://leetcode.com/problems/find-k-pairs-with-smallest-sums/)

Similar as the first problem.


```java
	public List<int[]> kSmallestPairs(int[] nums1, int[] nums2, int k) {
        PriorityQueue<Tuple> pq=new PriorityQueue<>();
        List<int[]> res=new ArrayList<>();
        if(nums1 == null || nums1.length == 0 || nums2 == null || nums2.length == 0 || k <= 0) return res;
        for(int i=0;i<nums2.length;i++) pq.offer(new Tuple(0,i,nums1[0]+nums2[i]));
        for(int i=0;i<Math.min(k,nums1.length*nums2.length);i++){
            Tuple t=pq.poll();
            res.add(new int[]{nums1[t.i1],nums2[t.i2]});
            if(t.i1!=nums1.length-1)
                pq.offer(new Tuple(t.i1+1,t.i2,nums1[t.i1+1]+nums2[t.i2]));
        }
        return res;
    }
    
    class Tuple implements Comparable<Tuple>{
        int i1,i2,val;
        public Tuple(int i1,int i2,int val){
            this.i1=i1;
            this.i2=i2;
            this.val=val;
        }
        public int compareTo(Tuple that){
            return this.val-that.val;
        }
    }
```




