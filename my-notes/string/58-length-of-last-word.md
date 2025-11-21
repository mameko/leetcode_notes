# 58 Length of Last Word

Given a string `s` consisting of words and spaces, return _the length of the **last** word in the string._

A **word** is a maximal substring consisting of non-space characters only.

&#x20;

**Example 1:**

<pre><code>Input: s = "Hello World"
<strong>Output:
</strong> 5
<strong>Explanation:
</strong> The last word is "World" with length 5.
</code></pre>

**Example 2:**

<pre><code>Input: s = "   fly me   to   the moon  "
<strong>Output:
</strong> 4
<strong>Explanation:
</strong> The last word is "moon" with length 4.
</code></pre>

**Example 3:**

<pre><code>Input: s = "luffy is still joyboy"
<strong>Output:
</strong> 6
<strong>Explanation:
</strong> The last word is "joyboy" with length 6.
</code></pre>

&#x20;

**Constraints:**

* `1 <= s.length <= 104`
* `s` consists of only English letters and spaces `' '`.
* There will be at least one word in `s`.

这题，就做来恢复一下信心。其实我这里用了S：O（N），如果用chatAt()可以O(1)。

```java
public int lengthOfLastWord(String s) {
    if (s == null || s.length() == 0) {
        return 0;
    }
    
    char[] cs = s.toCharArray();
    int result = 0;
    boolean foundWord = false;
    for (int i = cs.length - 1; i >= 0; i--) {
        char curChar = cs[i];
        if (curChar == ' ') {
            if (!foundWord) {
                continue;
            } else {
                return result;    
            }                
        } else {
            foundWord = true;
            result++;
        }
    }
    
    return result;
}   
```
