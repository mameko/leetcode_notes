# 204 Count Prime

**Description:**

Count the number of prime numbers less than a non-negative number,**n**.

用那个啥shieve（Sieve of Eratosthenes）。要O(n) space。T:O(n log log n)

```java
public int countPrimes(int n) {
    if (n < 2) {
        return 0;
    }

    boolean[] prime = new boolean[n + 1];
    Arrays.fill(prime, true);
    prime[0] = false;
    prime[1] = false;
    for (int i = 2; i * i < n; i++) {
        if (!prime[i]) {
            continue;
        }
        for (int j = i * 2; j < n; j+=i) {
            prime[j] = false;
        }
    }

    int count = 0;
    for (int i = 0; i < prime.length - 1; i++) {
        if (prime[i]) {
            count++;
        }
    }

    return count;
}
```
