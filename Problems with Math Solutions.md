# Problems with Math Solutions

## [Rotate Function](https://leetcode.com/problems/rotate-function/)

We can see at first glance that this problem give us a formula of F(n) and Bk[n]. This immediately let us realize that maybe a math solution is suitable for this problem. 

Write down and observe F(0) to F(n).

```
F(k) = 0*Bk[0] + 1*Bk[1] + ... + (n-1)*Bk[n-1]
F(k-1) = 0*Bk-1[0] + 1*Bk-1[1] + ... + (n-1)*Bk-1[n-1]
       = 0*Bk[1] + 1*Bk[2] + ... + (n-2)*Bk[n-1] + (n-1)*Bk[0]
```
It turns out that:

```
F(k) - F(k-1) = Bk[1] + Bk[2] + ... + Bk[n-1] + (1-n)Bk[0]
              = (Bk[0] + ... + Bk[n-1]) - nBk[0]
              = sum - nBk[0]
```

Then,

```
F(k) = F(k-1) + sum - nBk[0]
```
So, What is Bk[0]?

```
k = 0; B[0] = A[0];
k = 1; B[0] = A[len-1];
k = 2; B[0] = A[len-2];
...
```
Thus, we can write down our solution easily.

```java
public int maxRotateFunction(int[] A) {
        int f=0,sum=0;
        for(int i=0;i<A.length;i++){
            f+=i*A[i];
            sum+=A[i];
        }
        int max=f;
        for(int i=A.length-1;i>=1;i--){
            f=f+sum-A.length*A[i];
            max=Math.max(max,f);
        }
        return max;
    }
```