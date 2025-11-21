# 409 Longest Palindrome

Given a string `s` which consists of lowercase or uppercase letters, return _the length of the **longest palindrome**_ that can be built with those letters.

Letters are **case sensitive**, for example, `"Aa"` is not considered a palindrome here.

&#x20;

**Example 1:**

<pre><code><strong>Input: s = "abccccdd"
</strong><strong>Output: 7
</strong><strong>Explanation: One longest palindrome that can be built is "dccaccd", whose length is 7.
</strong></code></pre>

**Example 2:**

<pre><code><strong>Input: s = "a"
</strong><strong>Output: 1
</strong><strong>Explanation: The longest palindrome that can be built is "a", whose length is 1.
</strong></code></pre>

&#x20;

**Constraints:**

* `1 <= s.length <= 2000`
* `s` consists of lowercase **and/or** uppercase English letters only.

一道简单题想了我半天没想出来。原来是贪心。一开始以为只是判断偶数的个数，然后最后如果有基数的，就result + 1。后来看了九章的答案才知道原来是用set来做。

```java
public int longestPalindrome(String s) {
    if (s == null || s.isEmpty()) {
        return 0;
    }

    Set<Character> remaining = new HashSet<>();
    for (Character c : s.toCharArray()) {
        if (remaining.contains(c)) {
            remaining.remove(c);
        } else {
            remaining.add(c);
        }
    }

    int numOfCharsNotAbleToFormParlindrome = remaining.size();
    if (numOfCharsNotAbleToFormParlindrome > 0) {
         // Because one of them can put in the middle and form parlindrome with other chars
        numOfCharsNotAbleToFormParlindrome--;
    }

    return s.length() - numOfCharsNotAbleToFormParlindrome;
}
```
