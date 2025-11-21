# 1704 Determine if String Halves Are Alike

You are given a string `s` of even length. Split this string into two halves of equal lengths, and let `a` be the first half and `b` be the second half.

Two strings are **alike** if they have the same number of vowels (`'a'`, `'e'`, `'i'`, `'o'`, `'u'`, `'A'`, `'E'`, `'I'`, `'O'`, `'U'`). Notice that `s` contains uppercase and lowercase letters.

Return `true` _if_ `a` _and_ `b` _are **alike**_. Otherwise, return `false`.

**Example 1:**

```
Input: s = "book"
Output: true
Explanation: a = "bo" and b = "ok". a has 1 vowel and b has 1 vowel. Therefore, they are alike.
```

**Example 2:**

```
Input: s = "textbook"
Output: false
Explanation: a = "text" and b = "book". a has 1 vowel whereas b has 2. Therefore, they are not alike.
Notice that the vowel o is counted twice.
```

**Example 3:**

```
Input: s = "MerryChristmas"
Output: false
```

**Example 4:**

```
Input: s = "AbCdEfGh"
Output: true
```

**Constraints:**

* `2 <= s.length <= 1000`
* `s.length` is even.
* `s` consists of **uppercase and lowercase** letters.

这题，花在把oval放进set里的时间比解题多。就是数数而已。这里再贴一个标准答案，没有用set的。T:O(n), S:O(1)

```java
public boolean halvesAreAlike(String s) {
    if (s == null || s.isEmpty()) {
        return false;
    }

    char[] chars = {'a', 'e', 'i', 'o', 'u', 'A', 'E', 'I', 'O', 'U'};
    Set<Character> ovals = new HashSet<>();
    for (char c : chars) {
        ovals.add(c);
    }

    int cntPrev = 0;
    int cntLast = 0;

    for (int i = 0, j = s.toCharArray().length - 1; i < j; i++, j--) {
        if (ovals.contains(s.charAt(i))) {
            cntPrev++;
        }

        if (ovals.contains(s.charAt(j))) {
            cntLast++;
        }
    }

    return cntPrev == cntLast;
}

// solution 
public boolean halvesAreAlike(String s) {
    int n = s.length();
    return countVowel(0, n / 2, s) == countVowel(n / 2, n, s);
}

private int countVowel(int start, int end, String s) {
    String vowels = "aeiouAEIOU";
    int answer = 0;
    for (int i = start; i < end; i++) {
        if (vowels.indexOf(s.charAt(i)) != -1) {
            answer++;
        }
    }
    return answer;
}
```
