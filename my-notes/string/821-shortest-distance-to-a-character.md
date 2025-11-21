# 821 Shortest Distance to a Character

Given a string `s` and a character `c` that occurs in `s`, return _an array of integers `answer` where_ `answer.length == s.length` _and_ `answer[i]` _is the shortest distance from_ `s[i]` _to the character_ `c` _in_ `s`.

**Example 1:**

```
Input: s = "loveleetcode", c = "e"
Output: [3,2,1,0,1,0,0,1,2,2,1,0]
```

**Example 2:**

```
Input: s = "aaab", c = "b"
Output: [3,2,1,0]
```

**Constraints:**

* `1 <= s.length <= 104`
* `s[i]` and `c` are lowercase English letters.
* `c` occurs at least once in `s`.

这道简单题，我竟然用了T:O(kn)的解法。看了答案，发现，其实不用每次找char的时候都把数组扫一遍，只要左到右来一遍，右到左来一遍就ok了。T:(n)。这里巧妙地运用了max和min来把该从另一侧开始算的数字在第一轮loop中设为很大的数。然后第二次循环，我们取min就能得到答案。

```java
public int[] shortestToChar(String s, char c) {
    if (s == null || s.isEmpty()) {
        return null;
    }

    int n = s.length();
    int[] res = new int[n];
    Arrays.fill(res, Integer.MAX_VALUE);
    List<Integer> loc = new ArrayList<>();
    for (int i = 0; i < n; i++) {
        if (s.charAt(i) == c) {
            loc.add(i);
        }
    }

    for (int i = 0; i < loc.size(); i++) {
        int curLoc = loc.get(i);
        for (int j = 0; j < n; j++) {
            res[j] = Math.min(Math.abs(curLoc - j), res[j]);
        }
    }

    return res;
}

// solution
public int[] shortestToChar(String s, char c) {
    if (s == null || s.isEmpty()) {
        return null;
    }
    
    int n = s.length();
    int[] res = new int[n];
    
    int prev = Integer.MIN_VALUE / 2; 
    // left to right
    for (int i = 0; i < n; i++) {
        if (s.charAt(i) == c) {
            prev = i;
        }
        
        // we are minusing MIN, so will get a really big num here
        res[i] = i - prev; 
    }
    
    prev = Integer.MAX_VALUE / 2;
    // right to left
    for (int i = n - 1; i >= 0; i--) {
        if (s.charAt(i) == c) {
            prev = i;
        }
        // we are using MAX to minus, so will get a really big num here
        // them compare with the left_to_right result, take min
        res[i] = Math.min(res[i], prev - i); 
    }
    
    return res;
}
```

