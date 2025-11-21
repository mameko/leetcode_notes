# 806 Number of Lines To Write String

You are given a string `s` of lowercase English letters and an array `widths` denoting **how many pixels wide** each lowercase English letter is. Specifically, `widths[0]` is the width of `'a'`, `widths[1]` is the width of `'b'`, and so on.

You are trying to write `s` across several lines, where **each line is no longer than** `100` **pixels**. Starting at the beginning of `s`, write as many letters on the first line such that the total width does not exceed `100` pixels. Then, from where you stopped in `s`, continue writing as many letters as you can on the second line. Continue this process until you have written all of `s`.

Return _an array_ `result` _of length 2 where:_

* `result[0]` _is the total number of lines._
* `result[1]` _is the width of the last line in pixels._

**Example 1:**

```
Input: widths = [10,10,10,10,10,10,10,10,10,10,10,10,10,10,10,10,10,10,10,10,10,10,10,10,10,10], s = "abcdefghijklmnopqrstuvwxyz"
Output: [3,60]
Explanation: You can write s as follows:
abcdefghij  // 100 pixels wide
klmnopqrst  // 100 pixels wide
uvwxyz      // 60 pixels wide
There are a total of 3 lines, and the last line is 60 pixels wide.
```

**Example 2:**

```
Input: widths = [4,10,10,10,10,10,10,10,10,10,10,10,10,10,10,10,10,10,10,10,10,10,10,10,10,10], s = "bbbcccdddaaa"
Output: [2,4]
Explanation: You can write s as follows:
bbbcccdddaa  // 98 pixels wide
a            // 4 pixels wide
There are a total of 2 lines, and the last line is 4 pixels wide.
```

**Constraints:**

* `widths.length == 26`
* `2 <= widths[i] <= 10`
* `1 <= s.length <= 1000`
* `s` contains only lowercase English letters.

看了半天才看懂了题目，差点勾起了68 [Text justification](../other/68-text-justification.md)的恐惧，毕竟是狗的题。发现狗挺喜欢考这种边界很难把控的题。这里主要难点是，什么时候到下一行。如果我们发现加的长度大于100了，就要减回来。因为过一遍，所以T:O(n), S:O(1)

```java
public int[] numberOfLines(int[] widths, String s) {
    int[] result = new int[2];
    if (widths == null || widths.length == 0 || s == null || s.isEmpty()) {
        return null;
    }

    int lastWidth = 0;
    int rowCnt = 0;

    int stringInd = 0;
    while (stringInd < s.length()) {
        lastWidth = 0;
        while (stringInd < s.length() && lastWidth < 100) {
            char curChar = s.charAt(stringInd);
            lastWidth = lastWidth + widths[curChar - 'a'];
            stringInd++;
        }

        if (lastWidth > 100) {
            stringInd--;
        }

        rowCnt++;
    }

    result[0] = rowCnt;
    result[1] = lastWidth;

    return result;
}
```

