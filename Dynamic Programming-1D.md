# Dynamic Programming-1D

## [Triangle](https://leetcode.com/problems/triangle/)

> thoughs and ideas are borrowed from [this post by stellari](https://discuss.leetcode.com/topic/1669/dp-solution-for-triangle).

The traingle has a tree like tructure, which would lead us think about traversal algorithm DFS. However, if you look closely, you would notice that the ajacent nodes always share a branch. In other words, there are **`overlapping subproblems`** in this problem. Meanwhile, suppose p,q are children of x, once the min path from p,q to the bottom is known, the min path starting from x can be calculated in O(1). That is **`optimal substructure`**. Therefore, **DP is the best solution to this problem in terms of time complexity.**

> Bottom-up or Top-down ?
> <br/><br/>
> **For 'top-down' DP**, starting from the node on the very top, we recursively find the minimum path sum of each node. When a path sum is calculated, we store it in an array (`memoization`); the next time we need to calculate the path sum of the same node, just retrieve it from the array. 
> 
> However, you will need a cache that is at least the same size as the input triangle itself to store the pathsum, which takes `O(n^2) space`. With some clever thinking, it might be possible to release some of the memory that will never be used after a particular point, but the order of the nodes being processed is not straightforwardly seen in a recursive solution, so deciding which part of the cache to discard can be a hard job.
> <br/><br/>
> **For 'Bottom-up' DP**, on the other hand, is very straightforward: we start from the nodes on the bottom row; the min pathsums for these nodes are the values of the nodes themselves. From there, the min pathsum at the ith node on the kth row would be the lesser of the pathsums of its two children plus the value of itself, i.e.:
> 
`OPT(i,j) = min( OPT(i+1,j) , OPT(i+1,j+1) ) + triangle[i][j]`
> Since OPT(i+1,?) would be useless after calculating OPT(i,?) is computed, we can simply set OPT(i,j) as a 1D array OPT(i), and iteratively update itself:
> 
> `OPT(j) = min( OPT(j) , OPT(j+1) ) + triangle[i][j]`

Bottom-up

```java
	public int minimumTotal(List<List<Integer>> triangle) {
        //bottom-up
        int[] dp=new int[triangle.size()];
        for(int i=0;i<dp.length;i++){
            dp[i]=triangle.get(dp.length-1).get(i);
        }
        for(int i=triangle.size()-2;i>=0;i--)
            for(int j=0;j<=i;j++)
                dp[j]=Math.min(dp[j],dp[j+1])+triangle.get(i).get(j);
        return dp[0];
    }
```

Top-down

```java
	public int minimumTotal(List<List<Integer>> triangle) {
		int rowNum = triangle.get(triangle.size() - 1).size();
    	int colNum = triangle.size();
    	int[][] dp = new int[rowNum][colNum];
    	int i = 0;
    	for (Integer n : triangle.get(colNum - 1)) {
    		dp[rowNum - 1][i++] = n;
    	}
    	for (int row = rowNum - 2, m = 0; row >= 0; row--, m++) {
    		for (int col = 0; col <= colNum - 2 - m; col++) {
    			dp[row][col] = Math.min(dp[row + 1][col], dp[row + 1][col + 1])
    					+ triangle.get(row).get(col);
    		}
    	}
    	return dp[0][0];
    }
```
