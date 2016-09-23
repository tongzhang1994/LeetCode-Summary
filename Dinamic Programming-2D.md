# Dinamic Programming-2D

## [Minimum Path Sum](https://leetcode.com/problems/minimum-path-sum/)

This is a typical DP problem. I summarize the **sapce optimization** process as follows.

We could first come out a method using `O(n^2)` extra space (say, array `sum[][]`). 

*(code is omitted here.)*

Apparently the space could be optimized. As can be seen, each time when we update `sum[i][j]`, we only need `sum[i - 1][j]` (at the current column) and `sum[i][j - 1]` (at the left column). So we don't have to maintain the full m*n matrix. Maintaining two columns is enough and now we have the following code.

```java
	public int minPathSum(int[][] grid) {
        int m=grid.length,n=grid[0].length;
        int[] curCol=new int[m];
        int[] preCol=new int[m];
        preCol[0]=grid[0][0];
        for(int i=1;i<m;i++)//0-th col
            preCol[i]=preCol[i-1]+grid[i][0];
        for(int j=1;j<n;j++){
            curCol[0]=preCol[0]+grid[0][j];//0-th row
            for(int i=1;i<m;i++)
                curCol[i]=Math.min(curCol[i-1],preCol[i])+grid[i][j];
            preCol=curCol;
        }
        return preCol[m-1];
    }
```

Now, further inspect the code above. It can be seen that maintaining `pre` is for recovering `pre[i]`, *which is simply `cur[i]` before its update*. So it is enough to use only one array. Now the space is further optimized and the code also gets shorter.

```java
	public int minPathSum(int[][] grid) {
        int m=grid.length,n=grid[0].length;
        int[] curCol=new int[m];
        curCol[0]=grid[0][0];
        for(int i=1;i<m;i++)//0-th col
            curCol[i]=curCol[i-1]+grid[i][0];
        for(int j=1;j<n;j++){
            curCol[0]+=grid[0][j];//0-th row
            for(int i=1;i<m;i++)
                curCol[i]=Math.min(curCol[i-1],curCol[i])+grid[i][j];
        }
        return curCol[m-1];
    }
```


