# 709 To Lower Case

Implement function ToLowerCase() that has a string parameter str, and returns the same string in lowercase.

**Example 1:**

```
Input: "Hello"
Output: "hello"
```

**Example 2:**

```
Input: "here"
Output: "here"
```

**Example 3:**

```
Input: "LOVELY"
Output: "lovely"
```

这个很简单的简单题，查了半天google，自己还不能一次AC。惭愧。把答案也贴上来参考参考

```java
public String toLowerCase(String str) {
    if (str == null || str.length() == 0) {
        return null;
    }

    // <uppercase, lowerCase>
    Map<Character, Character> map = new HashMap<>();
    for (char i = 0; i < 26; i++) {
        char upperCase = (char)('A' + i);
        char lowerCase = (char)('A' + i + 32);
        map.put(upperCase, lowerCase);            
    }

    StringBuilder sb = new StringBuilder();
    for (int i = 0; i < str.length(); i++) {
        char curChar = str.charAt(i);
        if (map.containsKey(curChar)) {
            curChar = map.get(curChar);
        }
        sb.append(curChar);
    }

    return sb.toString();
}

// 参考答案
  public String toLowerCase(String str) {
    Map<Character, Character> h = new HashMap();
    String upper = "ABCDEFGHIJKLMNOPQRSTUVWXYZ";
    String lower = "abcdefghijklmnopqrstuvwxyz";
    for (int i = 0; i < 26; ++i) {
      h.put(upper.charAt(i), lower.charAt(i));
    }

    StringBuilder sb = new StringBuilder();
    for (char x : str.toCharArray()) {
      sb.append(h.containsKey(x) ? h.get(x) : x);
    }
    return sb.toString();
  }
```

