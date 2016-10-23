# Rotation

There are several ways to deal with `String Rotation`. Now we will talk about them.

## [Rotate Array](https://leetcode.com/problems/rotate-array/)

**1.Use extra copy**

Time: O(n). Space:O(n).

```java 
	public void rotate(int[] nums, int k) {
        int[] tmp=new int[nums.length];
        for(int i=0;i<nums.length;i++)
            tmp[i]=nums[i];
        for(int i=0;i<nums.length;i++)
            nums[(i+k)%nums.length]=tmp[i];
    }
```

**2.Rotate one by one**

Time: O(n). Space: O(1)

```java
	public void rotate(int[] nums, int k) {
        int n=nums.length;
        for(int i=0;i<k;i++){//k rotations
            int tmp=nums[n-1];
            for(int j=n-1;j>0;j--)//rotate one by one
                nums[j]=nums[j-1];
            nums[0]=tmp;
        }
    }
```

**3.3 reverse thinking**

Time:O(n). Space:O(1)

```java
    public void rotate(int[] nums, int k) {
        k%=nums.length;
        reverse(nums,0,nums.length-1);
        reverse(nums,0,k-1);
        reverse(nums,k,nums.length-1);
    }
    
    private void reverse(int[] nums,int start,int end){
        while(start<end){
            int tmp=nums[start];
            nums[start++]=nums[end];
            nums[end--]=tmp;
        }
    }
```

**4.juggling algorithm**

Time: O(n). Space: O(1)

> This is an extension of `solution 2`. Instead of moving one by one, divide the array in different sets
where number of sets is equal to GCD of n and d and move the elements within sets.
If GCD is 1 as is for the above example array (n = 7 and d =2), then elements will be moved within one set only, we just start with temp = arr[0] and keep moving arr[I+d] to arr[I] and finally store temp at the right place.
> Here is an example for n =12 and d = 3. GCD is 3 and
> <div  align="center"><img src="https://github.com/TongZhangUSC/LeetCode-Summary/blob/master/pic_explanation/jumping%20algorithm.png" width = "600" height = "400" alt="jumping" align=center /></div>

*You can read my tips first:
[XOR Swapping Algorithm](https://github.com/TongZhangUSC/LeetCode-Summary/blob/master/XOR%20Swapping%20Method.md) |
[GCD](https://github.com/TongZhangUSC/LeetCode-Summary/blob/master/GCD.md)*

```java 
	public void rotate(int[] nums, int k) {
        k%=nums.length;
        int gcd=gcd(nums.length,k);
        for(int i=0;i<gcd;i++){//k rotations
            int swaps=nums.length/gcd-1,j=i;
            for(int m=0;m<swaps;m++){
                j=(j+k)%nums.length;
                nums[i]^=nums[j];//XOR swapping
                nums[j]^=nums[i];
                nums[i]^=nums[j];
            }
        }
    }   
    private int gcd(int a, int b) {
        if (b == 0)    return a;
        else    return gcd(b, a % b);
    }
```

**[EXPLANATION](https://discuss.leetcode.com/topic/11349/my-three-way-to-solve-this-problem-the-first-way-is-interesting-java/20)**

Here use a example input array \[1,2,3,4,5,6,7,8\] (n = 8) to explain:

- suppose k = 3:

> GCD = gcd(3,8) = 1, which means there is only one path.
> Count = (n / GCD) - 1 = 7, which means we need 7 swaps to finish the path. (actually for a path have x element, we need x - 1 swaps).
>
> Then we can simulate the process of the algorithm, path0(each time swap index0 element and indexPosition element):<br/>
> [1,2,3,4,5,6,7,8] <br/>
> position = 3 -> [4,2,3,1,5,6,7,8]  <br/>
> position = 6 -> [7,2,3,1,5,6,4,8] <br/>
> position = 1 -> [2,7,3,1,5,6,4,8] <br/>
> position = 4 -> [5,7,3,1,2,6,4,8] <br/>
> position = 7 -> [8,7,3,1,2,6,4,5] <br/>
> position = 2 -> [3,7,8,1,2,6,4,5] <br/>
> position = 5 -> [6,7,8,1,2,3,4,5] -> finished, total 7 times swap. <br/>
> Final result [6,7,8,1,2,3,4,5]

- suppose k = 2:

Similary, GCD = 2, which means there are 2 paths.

count = 3, which means we need 3 swaps to finish each path.

Give the process:

path0(swap index0 and position element):

[1,2,3,4,5,6,7,8] position = 2 -> [3,2,1,4,5,6,7,8] position = 4 ->[5,2,1,4,3,6,7,8] position = 6 -> [7,2,1,4,3,6,5,8] -> path0 finished

Then we continue processing path1(swap index1 and position element):

[7,2,1,4,3,6,5,8] position = 3 -> [7,4,1,2,3,6,5,8] position = 5 -> [7,6,1,2,3,4,5,8] position = 7 ->[7,8,1,2,3,4,5,6] -> path1 finished -> all path finished we get the result [7,8,1,2,3,4,5,6]

> NOTE: This algorithm is very similar to the `juggling algorithm` proposed in 'Programming Perl'. Some related links to help understand are as follows.
[http://www.geeksforgeeks.org/array-rotation/](http://www.geeksforgeeks.org/array-rotation/)<br/>
[http://stackoverflow.com/questions/23321216/rotating-an-array-using-juggling-algorithm](http://stackoverflow.com/questions/23321216/rotating-an-array-using-juggling-algorithm)
