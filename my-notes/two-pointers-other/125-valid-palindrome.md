# 125 Valid Palindrome

Given a string, determine if it is a palindrome, considering only alphanumeric characters and ignoring cases.

For example,\
`"A man, a plan, a canal: Panama"`is a palindrome.\
`"race a car"`is not a palindrome.

**Note:**\
Have you consider that the string might be empty? This is a good question to ask during an interview.

For the purpose of this problem, we define empty string as valid palindrome.

这题在跳过非法字符时有点像partition。T:O(n)，S:O(1)

```java
public boolean isPalindrome(String s) {
    if (s == null || s.length() == 0) {
        return true;
    }

    String s2 = s.toUpperCase();
    char[] cs = s2.toCharArray();

    int left = 0; 
    int right = cs.length - 1;
    // 这里只用<, 估计是因为我们最后要的不是left跟right错开。
    // 所以小于才继续。等于就返回true了。eg：“a“
    while (left < right) { 
        while (left < right && !Character.isLetterOrDigit(cs[left])) {
            left++;
        }

        while (left < right && !Character.isLetterOrDigit(cs[right])) {
            right--;
        }

        if (cs[left] != cs[right]) {
            return false;
        } else {
            left++;
            right--;
        }
    }

    return true;
}
```
