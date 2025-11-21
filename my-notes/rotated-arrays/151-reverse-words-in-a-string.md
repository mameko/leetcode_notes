# 151 Reverse Words in a String

Given an input string, reverse the string word by word.

For example,\
Given s = "`the sky is blue`",\
return "`blue is sky the`".

**Update (2015-02-12):**\
For C programmers: Try to solve it in-place in O(1) space.

这题要求是，有多少个空格都返回一个。

```java
public String reverseWords(String s) {
    if (s == null || s.isEmpty()) {
        return "";
    }

    s = s.trim();

    String[] words = s.split("\\s+");
    StringBuilder sb = new StringBuilder();
    for (int i = words.length - 1; i >= 0; i--) {
        sb.append(words[i]);
        if (i != 0) {
            sb.append(" ");
        }
    }

    return sb.toString();

}

// 直接loop着做
public String reverseWords(String s) {
    if (s == null || s.length() == 0) {
        return s;
    }

    StringBuilder sb = new StringBuilder();
    int end = s.length() - 1;
    int start = end;

    while (end >= 0) {
        if (s.charAt(end) == ' ') {
            end--;
        } else {                
            start = end;
            while (start >= 0 && s.charAt(start) != ' ') {
                start--;
            }

            sb.append(s.substring(start + 1, end + 1));
            sb.append(' ');
            end = start;
        }
    }        

    if (sb.length() > 0) {
      sb.deleteCharAt(sb.length() - 1);
    }
    return sb.toString();
}
```
