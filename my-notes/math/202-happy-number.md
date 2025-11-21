# 202 Happy Number

Write an algorithm to determine if a number i&#x73;_&#x68;appy_.

A happy number is a number defined by the following process: Starting with any positive integer, replace the number by the sum of the squares of its digits, and repeat the process until the number equals 1 (where it will stay), or it loops endlessly in a cycle which does not include 1. Those numbers for which this process ends in 1 are happy numbers.

**Example**

19 is a happy number

```
1^2 + 9^2 = 82
8^2 + 2^2 = 68
6^2 + 8^2 = 100
1^2 + 0^2 + 0^2 = 1
```

通过计算发现，如果不是happy number的数，算着算着会出现重复数字，所以可以用一个hashset来保存出现过的数，如果发现，就表明这个不是happy number了。

2刷重写的时候，好像找到更容易理解/记住的方法：

```java
public boolean isHappy(int n) {
    if (n < 1) {
        return false;
    }

    Set<Integer> set = new HashSet<>();
    while (n != 1) {
        if (set.contains(n)) {
            return false;
        }
        set.add(n);
        int newNum = 0;
        while (n > 0) {
            newNum += (n % 10) * (n % 10);
            n /= 10;
        }            
        n = newNum;
    }

    return true;
}
```

实现时要注意代码写法，不知为毛得这样写才不会死循环。

```java
public boolean isHappy(int n) {
    if (n < 0) {
        return false;
    }

    HashSet<Integer> set = new HashSet<>();
    while (n != 1) {
        int res = 0;
        while (n != 0) {
            res += (n % 10) * (n % 10);
            n = n / 10;
        }

        n = res;
        if (set.contains(res)) {
            break;
        } else {
            set.add(res);
        }
    }
    return n == 1;
}
```
