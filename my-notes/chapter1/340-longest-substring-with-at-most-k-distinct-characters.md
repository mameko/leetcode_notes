# 340 Longest Substring with At Most K Distinct Characters

Given a string, find the length of the longest substring T that contains at mostkdistinct characters.

For example, Given s =`“eceba”`and k = 2,

T is "ece" which its length is 3.

跟上题一样，把2换成k

```java
public int lengthOfLongestSubstringKDistinct(String s, int k) {
        if (s == null || s.length() == 0 || k < 1) {
            return 0;
        }

        HashMap<Character, Integer> hm = new HashMap<>();
        int maxLen = 0;
        int left = 0;
        for (int right = 0; right < s.length(); right++) {
            Character rightChar = s.charAt(right);
            if (hm.containsKey(rightChar)) {
                hm.put(rightChar, hm.get(rightChar) + 1);
            } else {
                hm.put(rightChar, 1);
            }

            if (hm.size() <= k) {
                int curLen = right - left + 1;
                maxLen = Math.max(maxLen, curLen);
            } else {
                while (left < right && hm.size() > k) {
                    Character leftChar = s.charAt(left);
                    if (hm.get(leftChar) > 1) {
                        hm.put(leftChar, hm.get(leftChar) - 1);
                    } else {
                        hm.remove(leftChar);
                    }
                    left++;
                }
            }
        }

        return maxLen;
    }
```

Java 8 写法：

```java
public int lengthOfLongestSubstringKDistinct(String s, int k) {
    if (s == null || s.length() == 0) {
        return 0;
    }

    Map<Character, Integer> map = new HashMap<>();
    int left = 0;
    int max = 0;
    for (int right = 0; right < s.length(); right++) {
        map.put(s.charAt(right), map.getOrDefault(s.charAt(right), 0) + 1);

        while (map.size() > k) {
            char rc = s.charAt(left);
            if (map.containsKey(rc)) {
                int num = map.get(rc) - 1;
                if (num == 0) {
                    map.remove(rc);
                } else {
                    map.put(rc, num);
                }
            }

            left++;
        }

        max = Math.max(max, right - left + 1);
    }

    return max;
}
```
