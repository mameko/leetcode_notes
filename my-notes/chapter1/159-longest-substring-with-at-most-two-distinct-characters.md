# 159 Longest Substring with At Most Two Distinct Characters

Given a string, find the length of the longest substring T that contains at most 2 distinct characters.

For example, Given s =`“eceba”`,

T is "ece" which its length is 3.

这题没有套模板，不过还是silidng那个window。注意判断条件：left < right 和 hm.get(leftchar) > 1

```java
public int lengthOfLongestSubstringTwoDistinct(String s) {
        if (s == null || s.length() == 0) {
            return 0;
        }

        HashMap<Character, Integer> hm = new HashMap<>();
        int maxLen = 0;
        int left = 0;
        for (int right = 0; right < s.length(); right++) {
            Character rightChar = s.charAt(right);
            if (hm.containsKey(rightChar)) {//用一个hashmap来记录目前窗口的char和数目
                hm.put(rightChar, hm.get(rightChar) + 1);
            } else {
                hm.put(rightChar, 1);
            }

            if (hm.size() <= 2) {//如果发现现在窗口里有多于2个不同的字符时，更新长度。这里用了size来记录distinct chars的个数
                int curLen = right - left + 1;
                maxLen = Math.max(maxLen, curLen);
            } else {
                // 算完长度，就把窗口左边界往后移，知道当前窗口所含的distinct chars小于2
                while (left < right && hm.size() > 2) {
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

Java8写法：

```java
public int lengthOfLongestSubstringTwoDistinct(String s) {
    if (s == null || s.length() == 0) {
        return 0;
    }

    Map<Character, Integer> map = new HashMap<>();
    int max = 0;
    int left = 0;
    for (int right = 0; right < s.length(); right++) {
        map.put(s.charAt(right), map.getOrDefault(s.charAt(right), 0) + 1);           

        while (map.size() > 2) {
            int num = map.get(s.charAt(left)) - 1;
            if (num == 0) {
                map.remove(s.charAt(left));
            } else {
                map.put(s.charAt(left), num);
            }
            left++;
        }

        max = Math.max(max, right - left + 1);
    }
    return max;

}
```
