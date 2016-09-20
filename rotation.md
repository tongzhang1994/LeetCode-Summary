# Rotation

There are several ways to deal with `String Rotation`. Now we will talk about them.

## [Rotate Array](https://leetcode.com/problems/rotate-array/)

**1.Use extra copy**

Time: O(n). Space:O(n).

```
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

*I haven't solved it yet. I'll come back as soon as I fix my bug...*



**3.juggling algorithm**

Time: O(n). Space: O(1)

```
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

***XOR Swapping Algorithm***

Using XOR can swap two elements without using extra memory. Look at the following code with comments and you will understand how it works.

```
void swap(int a, int b){  
 a ^= b;  
 b ^= a;  //b=b^(a^b)=a
 a ^= b;  //a=(a^b)^(b^a^b)=b  
}  
```

**[EXPLANATION](https://discuss.leetcode.com/topic/11349/my-three-way-to-solve-this-problem-the-first-way-is-interesting-java/20)**

Here use a example input array [1,2,3,4,5,6,7,8] (n = 8) to explain:

- suppose k = 3:

GCD = gcd(3,8) = 1, which means there is only one path.

Count = (n / GCD) - 1 = 7, which means we need 7 swaps to finish the path. (actually for a path have x element, we need x - 1 swaps).

Then we can simulate the process of the algorithm,

path0(each time swap index0 element and indexPosition element):

[1,2,3,4,5,6,7,8] position = 3 -> [4,2,3,1,5,6,7,8]  position = 6 -> [7,2,3,1,5,6,4,8] position = 1 -> [2,7,3,1,5,6,4,8] position = 4 -> [5,7,3,1,2,6,4,8] position = 7 -> [8,7,3,1,2,6,4,5] position = 2 -> [3,7,8,1,2,6,4,5] position = 5 -> [6,7,8,1,2,3,4,5] -> finished, total 7 times swap. Final result [6,7,8,1,2,3,4,5]

- suppose k = 2:

Similary, GCD = 2, which means there are 2 paths.

count = 3, which means we need 3 swaps to finish each path.

Give the process:

path0(swap index0 and position element):

[1,2,3,4,5,6,7,8] position = 2 -> [3,2,1,4,5,6,7,8] position = 4 ->[5,2,1,4,3,6,7,8] position = 6 -> [7,2,1,4,3,6,5,8] -> path0 finished

Then we continue processing path1(swap index1 and position element):

[7,2,1,4,3,6,5,8] position = 3 -> [7,4,1,2,3,6,5,8] position = 5 -> [7,6,1,2,3,4,5,8] position = 7 ->[7,8,1,2,3,4,5,6] -> path1 finished -> all path finished we get the result [7,8,1,2,3,4,5,6]

>NOTE: This algorithm is very similar to the `juggling algorithm` proposed in 'Programming Perl'. Some related links to help understand are as follows.

[http://www.geeksforgeeks.org/array-rotation/](http://www.geeksforgeeks.org/array-rotation/)

[http://stackoverflow.com/questions/23321216/rotating-an-array-using-juggling-algorithm](http://stackoverflow.com/questions/23321216/rotating-an-array-using-juggling-algorithm)
