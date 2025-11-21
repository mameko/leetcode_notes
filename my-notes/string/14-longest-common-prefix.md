# 14 Longest Common Prefix

Write a function to find the longest common prefix string amongst an array of strings.

这题很straight forward，主要注意的是控制边界条件，什么时候加字母，什么时候跳出返回。

```java
public String longestCommonPrefix(String[] strs) {
    if (strs == null || strs.length == 0) {
        return "";
    }

    int j = 0;
    StringBuilder sb = new StringBuilder();
    while (j < strs[0].length()) {
        char last = strs[0].charAt(j);            
        for (int i = 1; i < strs.length; i++) {
            if (j >= strs[i].length()) {
                return sb.toString();
            }

            if (last != strs[i].charAt(j)) {
                return sb.toString();
            }             
        }
        sb.append(last);
        j++;
    }

    return sb.toString();
}
```

另外一种容易点控制的方式是，先loop一次找到size，然后再加到结果里。

```java
public String longestCommonPrefix(String[] strs) {
    if (strs == null || strs.length == 0) {
        return "";
    }

    int size = Integer.MAX_VALUE;
    for (int i = 0; i < strs.length; i++) {            
        size = Math.min(size, strs[i].length());
    }        

    StringBuilder sb = new StringBuilder();
    for (int j = 0; j < size; j++) {
        char pre = strs[0].charAt(j);
        for (int i = 1; i < strs.length; i++) {            
            if (strs[i].charAt(j) != pre) {
                return sb.toString();
            }
        }
        sb.append(pre);
    }

    return sb.toString();
}
```
