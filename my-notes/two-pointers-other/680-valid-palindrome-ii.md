# 680 Valid Palindrome II

Given a non-empty string `s`, you may delete **at most** one character. Judge whether you can make it a palindrome.

**Example 1:**

```
Input: "aba"
Output: True
```

**Example 2:**

```
Input: "abca"
Output: True
Explanation: You could delete the character 'c'.
```

**Note:**

1. The string will only contain lowercase characters a-z. The maximum length of the string is 50000.

这题，一看，bruteforce的话，可以一个一个试。然后因为parlindrom所以就想到对撞型双指针。一开始，中间那段，用了if else。譬如，left + 1 == right，就 --，left == right - 1，就++。因为是if else，所以不能两者兼得，而实际情况是，两个只要一个ok就return true。所以最后换成了递归。这里也只会递归一次，因为用了deletedOnce来控制。

后来看了solution，发现，其实，可以先对撞判断，如果不等了，就像现在这样，错开，把substring传到判断parlindrom的函数就好了。

```java
public boolean validPalindrome(String s) {
    if (s == null || s.length() == 0) {
        return false;
    }

    return parHelper(s, false);
}

private boolean parHelper(String s, boolean deletedOnce) {
    int left = 0;
    int right = s.length() - 1;

    while (left < right) {
        char leftChar = s.charAt(left);
        char rightChar = s.charAt(right);
        if (leftChar != rightChar) {
            if (!deletedOnce) {
                if (leftChar == s.charAt(right - 1) || rightChar == s.charAt(left + 1)) {
                    return parHelper(s.substring(left, right), true)
                            || parHelper(s.substring(left + 1, right + 1), true);
                } else {
                    return false;
                }
            } else {
                return false;
            }
        } else {
            left++;
            right--;
        }
    }

    return true;
}
```



<pre class="language-java"><code class="lang-java"><strong>
</strong>// 容易懂的方法
public boolean validPalindrome(String s) {
    if (s ==  null || s.isEmpty()) {
        return true;
    }

    int left = 0;
    int right = s.length() - 1;
    while (left &#x3C; right) {
        if (s.charAt(left) != s.charAt(right)) {
            return isPalindromeRange(s, left + 1, right) ||
                   isPalindromeRange(s, left, right - 1);
        }
        left++;
        right--;
    }
    
    return true;
}

private boolean isPalindromeRange(String s, int left, int right) {
    while (left &#x3C; right) {
        if (s.charAt(left) != s.charAt(right)) {
            return false;
        }
        left++;
        right--;
    }

    return true;
}

<strong>
</strong><strong>// 这个参考答案的下标比较复杂，所以还是写比较笨的方法吧
</strong><strong>public boolean isPalindromeRange(String s, int i, int j) {
</strong>    for (int k = i; k &#x3C;= i + (j - i) / 2; k++) {
        if (s.charAt(k) != s.charAt(j - k + i)) return false;
    }
    return true;
}
public boolean validPalindrome(String s) {
    for (int i = 0; i &#x3C; s.length() / 2; i++) {
        if (s.charAt(i) != s.charAt(s.length() - 1 - i)) {
            int j = s.length() - 1 - i;
            return (isPalindromeRange(s, i+1, j) ||
                    isPalindromeRange(s, i, j-1));
        }
    }
    return true;
}
</code></pre>
