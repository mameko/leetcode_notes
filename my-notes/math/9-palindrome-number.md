# 9 Palindrome Number

Determine whether an integer is a palindrome. Do this without extra space.

[click to show spoilers.](https://leetcode.com/problems/palindrome-number/tabs/description/)

**Some hints:**

Could negative integers be palindromes? (ie, -1)

If you are thinking of converting the integer to string, note the restriction of using extra space.

You could also try reversing an integer. However, if you have solved the problem "Reverse Integer", you know that the reversed integer might overflow. How would you handle such case?

There is a more generic way of solving this problem.

这题，感觉不太简单。我用了比较笨的方法，而且调了好就才调通。基本方法是取前后对称的那两位，如果不同的话，就返回false。最后全比较完还没发现不同，就返回true。数学上算得有点复杂。还得注意overflow。后来看到discuss里的答案，才发现可以用[7 Reverse Integer](7-reverse-integer.md)的思路来做。把后半翻转，然后跟前半比较。这样不用判断overflow而且写得还简洁。不知道为毛方法一快一点点.

方法二：

```java
public boolean isPalindrome(int x) {
    if (x < 0 || (x != 0 && x % 10 == 0)) {// x = 10
        return false;
    }

    int rev = 0;
    while (x > rev) {
        rev = rev * 10 + x % 10;
        x = x / 10;
    }

    return (x == rev) || (x == rev / 10);
}
```

方法一：

```java
public boolean isPalindrome(int x) {
    if (x < 0) {
        return false;
    }

    int baseL = 10;
    boolean flag = false;
    while (x / baseL > 0) {
        if (baseL > Integer.MAX_VALUE / 10) {
            flag = true;
            break;
        } else {
            baseL = baseL * 10;
        }
    }

    if (!flag) {
        baseL = baseL / 10;
    }
    int baseR = 10;
    while (baseL >= baseR) {
        int right = x % baseR / (baseR / 10);
        int left = x / baseL % 10;

        if (right != left) {
            return false;
        }

        baseL = baseL / 10;
        baseR = baseR * 10;
    }

    return true;
}
```
