# L235 Prime Factorization

Prime factorize a given integer.

Example

**Example 1:**

```
Input: 10Output: [2, 5]
```

**Example 2:**

```
Input: 660Output: [2, 2, 3, 5, 11]
```

这题，基本看答案的，需要背呀。除了记得找质因子只需要找到sqrt（num）之外，其他的都不是特别特别懂。

```java
public List<Integer> primeFactorization(int num) {
    if (num < 0) {
        return null;
    }

    List<Integer> res = new ArrayList<>();
    // 从2开始找，找到sqrt(num)
    for (int i = 2; i * i <= num; i++) {
        // 找到一个因数就加结果，但为什么加的都是质因子呢？不懂
        while (num % i == 0) {
            num = num / i;
            res.add(i);
        }
    }

    // 如果最后没除到1，这个num是剩下的那个质数
    if (num != 1) {
        res.add(num);
    }

    return res;
}
```
