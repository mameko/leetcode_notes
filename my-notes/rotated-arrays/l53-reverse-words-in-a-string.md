# L53 Reverse Words in a String

Given an input string, reverse the string word by word.

For example,\
Given s = "`the sky is blue`",\
return "`blue is sky the`".

**Clarification**

*   What constitutes a word?

    A sequence of non-space characters constitutes a word.
*   Could the input string contain leading or trailing spaces?

    Yes. However, your reversed string should not contain leading or trailing spaces.
*   How about multiple spaces between two words?

    Reduce them to a single space in the reversed string.

这题要求是，空格有多少个，返回多少个。

```java
    public String reverseWords(String s) {
        if (s == null || s.isEmpty()) {
            return "";
        }

        String[] words = s.split(" ");
        StringBuilder sb = new StringBuilder();
        for (int i = words.length - 1; i >= 0; i--) {
            sb.append(words[i]);
            if (i != 0) {
                sb.append(" ");
            }
        }

        return sb.toString();
    }
```
