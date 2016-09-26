# GCD

There are many algorithms to calculate the Greatest Common Divisor. Here is a famous and easy-to-understand one.

Please refer to: [Euclidean algorithm](https://en.wikipedia.org/wiki/Euclidean_algorithm)

A java version.

```java
	int gcd(int a, int b) {
        if (b == 0)    return a;
        else    return gcd(b, a % b);
    }
```