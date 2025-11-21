# 557 Reverse Words in a String III

Given a string, you need to reverse the order of characters in each word within a sentence while still preserving white space and initial word order.

**Example 1:**

```
Input: "Let's take LeetCode contest"
Output: "s'teL ekat edoCteeL tsetnoc"
```

**Note:**&#x49;n the string, each word is separated by single space and there will not be any extra space in the string.

什么鬼，就是三部翻转少翻最后一步...

<pre class="language-java"><code class="lang-java"><strong>// 2023又刷了一遍，还不能一次AC
</strong>public String reverseWords(String s) {
    if (s == null || s == "") {
        return s;
    }

    char[] chars = s.toCharArray();        
    int j = 0;
    for (int i = 0; i &#x3C; chars.length; i++) {
        if (chars[i] != ' ') {                
            continue;
        } else {
            swap(j, i - 1, chars);
            j = i + 1;
        }
    }

    swap(j, chars.length - 1, chars);
    return new String(chars);
}

private void swap(int start, int end, char[] chars) {
    for (int s = start, e = end; s &#x3C; e; s++, e--) {
        char tmp = chars[s];
        chars[s] = chars[e];
        chars[e] = tmp;
    }
}

<strong>public String reverseWords(String s) {        
</strong>    if (s == null || s.length() == 0) {
        return s;
    }

    s = s.trim();
    char[] cs = s.toCharArray();

    int j = 0;
    int i = 0;
    while (j &#x3C;= s.length()) {
        if (j &#x3C; s.length() &#x26;&#x26; s.charAt(j) != ' ') {                
            j++;
        } else {
            cs = rev(cs, i, j - 1);
            i = j;
            j++;
            i++;
            continue;
        }
    }

    return new String(cs);
}

private char[] rev(char[] cs, int i, int j) {
    while (i &#x3C; j) {
        char tmp = cs[i];
        cs[i] = cs[j];
        cs[j] = tmp;
        i++;
        j--;
    }

    return cs;
}
</code></pre>
