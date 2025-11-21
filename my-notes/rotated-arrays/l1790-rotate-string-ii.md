# L1790 Rotate String II

Description

Given a string(Given in the way of char array), a right offset and a left offset, move the string according to the given offset and save it in a new result set. (left offest represents the offset of a string to the left,right offest represents the offset of a string to the right,the total offset is calculated from the left offset and the right offset, split two strings at the total offset and swap positions)。



Example

**Example 1:**

```
Input：str ="abcdefg", left = 3, right = 1Output："cdefgab"Explanation：The left offset is 3, the right offset is 1, and the total offset is left 2. Therefore, the original string moves to the left and becomes "cdefg"+ "ab".
```

**Example 2:**

```
Input：str="abcdefg", left = 0, right = 0Output："abcdefg"Explanation：The left offset is 0, the right offset is 0, and the total offset is 0. So the string remains unchanged.
```

**Example 3:**

```
Input：str = "abcdefg",left = 1, right = 2Output："gabcdef"Explanation：The left offset is 1, the right offset is 2, and the total offset is right 1. Therefore, the original string moves to the left and becomes "g" + "abcdef".
```

这题，好眼熟，3步翻转

<pre class="language-java"><code class="lang-java"><strong>public String rotateString2(String str, int left, int right) {
</strong>    if (str == null || str.isEmpty()) {
        return str;
    }

    int len = str.length();
    // 难点在于算这个offset，这里减完先mod是怕负得太多，然后加len是变正数。再mod一遍以免超出
    int offset = ((left - right - 1) % len + len) % len;

    char[] chars = str.toCharArray();
    swap(chars, 0, offset);
    swap(chars, offset + 1, len - 1);
    swap(chars, 0, len - 1);

    return new String(chars);
}

private void swap(char[] str, int left, int right) {
    for (int i = left, j = right; left &#x3C; right; left++, right--) {
        char tmp = str[left];
        str[left] = str[right];
        str[right] = tmp;
    }
}
</code></pre>
