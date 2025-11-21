# 186 Reverse Words in a String II

Given an input string, reverse the string word by word. A word is defined as a sequence of non-space characters.

The input string does not contain leading or trailing spaces and the words are always separated by a single space.

For example,\
Given s = "`the sky is blue`",\
return "`blue is sky the`".

Could you do it in-place without allocating extra space?

这题没什么区别，就是要求inplace。三部翻转法。T:O(n), S：O(1)

```java
    public void reverseWords(char[] s) {
        if (s == null || s.length == 0) {
            return;
        }

        int start = 0;
        for (int end = 1; end <= s.length; end++) {
            if (end == s.length || s[end] == ' ') {
                reverse(start, end - 1, s);
                start = end + 1;
            }
        }

        reverse(0, s.length - 1, s);
    }

    private char[] reverse (int i, int j, char[] cs) {
        while (i < j) {
            char tmp = cs[i];
            cs[i] = cs[j];
            cs[j] = tmp;
            i++;
            j--;
        }

        return cs;
    }
```
