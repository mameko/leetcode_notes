# 358 Rearrange String k Distance Apart

Given a non-empty string**s**and an integer**k**, rearrange the string such that the same characters are at least distance**k**from each other.

All input strings are given in lowercase letters. If it is not possible to rearrange the string, return an empty string`""`.

**Example 1:**

```
s = "aabbcc", k = 3

Result: "abcabc"

The same letters are at least distance 3 from each other.
```

**Example 2:**

```
s = "aaabc", k = 3 

Answer: ""

It is not possible to rearrange the string.
```

**Example 3:**

```
s = "aaadbbcc", k = 2

Answer: "abacabcd"

Another possible answer is: "abcabcda"

The same letters are at least distance 2 from each other.
```

这题，不看答案不会做。自己只想到数频率这一步，数完以后，用另外一条array来存下一个可以放的位置。一开始所有字母都能从0开始。然后一边填一边更新这个位置信息（+k）。填的时候每次多把出现频率较大的那个拿出来填，填完以后更新下一个能填的位置和把频率减一表示已经填好一个了。然后下一个循环找下一个填的字母。

```java
public String rearrangeString(String s, int k) {
    if (s == null || s.isEmpty() || k < 0) {
        return "";
    }

    int[] count = new int[26];
    int[] nextPos = new int[26];

    // count occurrance of each char
    for (int i = 0; i < s.length(); i++) {
        int loc = s.charAt(i) - 'a';
        count[loc]++;
    }

    StringBuilder sb = new StringBuilder();
    for (int i = 0; i < s.length(); i++) {
        int validPos = getValidPos(count, nextPos, i);
        if (validPos == -1) {
            return "";
        }
        count[validPos]--;
        nextPos[validPos] = i + k;
        sb.append((char)('a' + validPos));
    }

    return sb.toString();
}

private int getValidPos(int[] count, int[] nextPos, int cur) {// 听说这个函数可以用priority queue来代替
    int result = -1;
    int max = Integer.MIN_VALUE;
    for (int i = 0; i < 26; i++) {
        if (count[i] > 0 && count[i] > max && cur >= nextPos[i]) {
            result = i;
            max = count[i];
        }
    }
    return result;
}
```
