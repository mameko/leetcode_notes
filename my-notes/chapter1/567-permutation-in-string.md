# 567 Permutation in String

Given two strings **s1**and **s2**, write a function to return true if **s2** contains the permutation of **s1**. In other words, one of the first string's permutations is the **substring** of the second string.

**Example 1:**

```
Input:s1 = "ab" s2 = "eidbaooo"
Output:True

Explanation: s2 contains one permutation of s1 ("ba").
```

**Example 2:**

```
Input:s1= "ab" s2 = "eidboaoo"
Output: False
```

**Note:**

1. The input strings only contain lower case letters.
2. The length of both given strings is in range \[1, 10,000].

这题一看就是[438](438-find-all-anagrams-in-a-string.md)简化，一开始还以为有什么其他的解法，后来看了答案发现还是sliding window。72ms

```java
public boolean checkInclusion(String s1, String s2) {
    if (s1 == null || s2 == null || s1.isEmpty() || s2.isEmpty()) {
        return false;
    }

    HashMap<Character, Integer> target = new HashMap<>();
    for (int i = 0; i < s1.length(); i++) {
        char cur = s1.charAt(i);
        if (target.containsKey(cur)) {
            target.put(cur, target.get(cur) + 1);
        } else {
            target.put(cur, 1);
        }
    }

    int cnt = s1.length();
    int start = 0;
    for (int end = 0; end < s2.length(); end++) {
        char endChar = s2.charAt(end);
        if (target.containsKey(endChar)) {
            if (target.get(endChar) > 0) {
                cnt--;
            }
            target.put(endChar, target.get(endChar) - 1);
        }

        if (cnt == 0) {
            return true;
        }

        if (end - start + 1 == s1.length()) {
            char startChar = s2.charAt(start);
            if (target.containsKey(startChar)) {
                target.put(startChar, target.get(startChar) + 1);
                if (target.get(startChar) > 0) {
                    cnt++;
                }
            }
            start++;
        }
    }
    return false;
}
```

换个int数组，好快9ms

```java
public boolean checkInclusion(String s1, String s2) {
    if (s1 == null || s1.length() == 0 || s2 == null || s2.length() == 0) {
        return false;
    }

    int[] target = new int[256];
    for (int i = 0; i < s1.length(); i++) {
        target[s1.charAt(i)]++;        
    }

    int cnt = s1.length();
    int left = 0;
    for (int right = 0; right < s2.length(); right++) {
        char rc = s2.charAt(right);
        if (target[rc] > 0) {
            cnt--;
        }
        target[rc]--;

        if (cnt == 0) {
            return true;
        }

        if (right - left + 1 >= s1.length()) {
            char lc = s2.charAt(left);
            if (target[lc] >= 0) {
                cnt++;
            }
            target[lc]++;
            left++;
        }
    }

    return false;
}
```
