# XOR Swapping Method

Using XOR can swap two elements without using extra memory. Look at the following code with comments and you will understand how it works.

```java
void swap(int a, int b){  
 a ^= b;  
 b ^= a;  //b=b^(a^b)=a
 a ^= b;  //a=(a^b)^(b^a^b)=b  
}  
```
