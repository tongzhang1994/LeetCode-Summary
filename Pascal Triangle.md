# Pascal Triangle

## [Pascal Triangle II](https://leetcode.com/problems/pascals-triangle-ii/)

I think most people are familiar with Pascal Triangles and it might be easy for most people to construct it from row\_0 to row\_n. But this problem gives us an insight on how to construct *a single line* of Pascal Triangle `with only O(n) space`.

```java
	public List<Integer> getRow(int rowIndex) {
        Integer[] res = new Integer[rowIndex + 1];
        Arrays.fill(res,1);
        for(int row=0;row<rowIndex;row++)
            for(int col=row;col>0;col--)
                res[col]+=res[col-1];
        return Arrays.asList(res);
	}
```

Here is a picture version explanation to the solution above. Writing down each step on the paper, We can see that we are actually constructing the traigle in-place!  

![pic](https://github.com/TongZhangUSC/LeetCode-Summary/blob/master/pic_explanation/pascal.jpg)

*NOTE: we must construct the line from right to left to assure that the value we used is calculated during last iteration/line (not the current one).*