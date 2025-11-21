# 76 Minimum Window Substring

Given a string S and a string T, find the minimum window in S which will contain all the characters in T in complexity O(n).

For example,\
**S**=`"ADOBECODEBANC"`\
**T**=`"ABC"`

Minimum window is`"BANC"`.

**Note:**\
If there is no such window in S that covers all characters in T, return the empty string`""`.

If there are multiple such windows, you are guaranteed that there will always be only one unique minimum window in S.

方法一：这题也可以套模板，T:O(n), S:O(n) 41ms

```java
public String minWindow(String s, String t) {
        if (s == null || t == null || s.isEmpty() || t.isEmpty()) {
            return "";
        }

        HashMap<Character, Integer> targetMap = new HashMap<>();
        for (int i = 0; i < t.length(); i++) {
            Character c = t.charAt(i);
            if (targetMap.containsKey(c)) {
                targetMap.put(c, targetMap.get(c) + 1);
            } else {
                targetMap.put(c, 1);
            }
        }

        int cnt = t.length();
        int start = 0;
        int min = s.length() + 1;
        String result = "";
        // loop througth the string and find min length
        for (int end = 0; end < s.length(); end++) {
            Character c = s.charAt(end);
            // expand end pointer to include all char in target
            if (targetMap.containsKey(c)) {
                if (targetMap.get(c) >= 1) {
                    cnt--;    
                } 
                targetMap.put(c, targetMap.get(c) - 1);
            }


            // shrink start
            while (cnt == 0) {// 这里因为窗口大小不定，所以要一直往左缩到最小
                int curLen = end - start + 1;
                if (curLen < min) {
                    min = curLen;
                    result = s.substring(start, end + 1);
                }

                Character startChar = s.charAt(start);
                if (targetMap.containsKey(startChar)) {
                    if (targetMap.get(startChar) >= 0) {
                        cnt++;
                    }
                    targetMap.put(startChar, targetMap.get(startChar) + 1);
                }
                start++;
            }
        }

        return result;
    }
```

方法二：用两个HashMap，一个存target另一个用来数数

```java
public String minWindow(String s, String t) {
        if (s == null || t == null || s.isEmpty() || t.isEmpty()) {
            return "";
        }

        // collect target string character & it's number
        HashMap<Character, Integer> targetMap = new HashMap<>();
        for (int i = 0; i < t.length(); i++) {
            Character c = t.charAt(i);
            if (targetMap.containsKey(c)) {
                targetMap.put(c, targetMap.get(c) + 1);
            } else {
                targetMap.put(c, 1);
            }
        }

        int cnt = 0; // counter for if current we include all char in target array
        HashMap<Character, Integer> currentWindow = new HashMap<>();
        int start = 0;
        int min = s.length() + 1;
        String result = "";
        // loop througth the string and find min length
        for (int end = 0; end < s.length(); end++) {
            Character c = s.charAt(end);
            // expand end pointer to include all char in target
            if (targetMap.containsKey(c)) {
                if (currentWindow.containsKey(c)) {
                    if (currentWindow.get(c) < targetMap.get(c)) {
                        cnt++;
                    }
                    currentWindow.put(c, currentWindow.get(c) + 1);
                } else {
                    currentWindow.put(c, 1);
                    cnt++;
                }
            } 

            // shrink start
            if (cnt == t.length()) {
                Character startChar = s.charAt(start);
                while (!currentWindow.containsKey(startChar) || currentWindow.get(startChar) > targetMap.get(startChar)) {
                    if (currentWindow.containsKey(startChar) && currentWindow.get(startChar) > targetMap.get(startChar)) {
                        currentWindow.put(startChar, currentWindow.get(startChar) - 1);
                    }
                    start++;
                    startChar = s.charAt(start);
                }

                int curLen = end - start + 1;
                if (curLen < min) {
                    min = curLen;
                    result = s.substring(start, end + 1);
                }
            }
        }

        return result;
    }
```

```java
public String minWindow(String s, String t) {
    if (s == null || t == null) {
        return "";
    }

    HashMap<Character, Integer> target = new HashMap<>();
    for (int i = 0; i < t.length(); i++) {
        char cur = t.charAt(i);
        if (target.containsKey(cur)) {
            target.put(cur, target.get(cur) + 1);
        } else {
            target.put(cur, 1);
        }
    }

    int cnt = t.length();
    int min = s.length() + 1;
    String res = "";

    int start = 0;
    for (int end = 0; end < s.length(); end++) {
        char endChar = s.charAt(end);
        if (target.containsKey(endChar)) {
            if (target.get(endChar) > 0) {
                cnt--;
            }
            target.put(endChar, target.get(endChar) - 1);
        }

        while (cnt == 0) {
            int curLen = end - start + 1;
            if (curLen < min) {
                min = curLen;
                res = s.substring(start, end + 1);
            }

            char startChar = s.charAt(start);
            if (target.containsKey(startChar)) {
                target.put(startChar, target.get(startChar) + 1);
                if (target.get(startChar) > 0) {
                    cnt++;
                }
            }
            start++;
        }
    }

    return res;
}
```

换个int数组快好多，5ms

```java
public String minWindow(String s, String t) {
    if (s == null || s.length() == 0 || t == null || t.length() == 0) {
        return "";
    }

    int[] target = new int[256];    
    for (int i = 0; i < t.length(); i++) {
        target[t.charAt(i)]++;
    }

    int left = 0;
    int min = s.length() + 1;
    String res = "";
    int cnt = t.length();
    for (int right = 0; right < s.length(); right++) {
        char rc = s.charAt(right);
        if (target[rc] > 0) {
            cnt--;
        }
        target[rc]--;

        while (cnt == 0) {
            if (right - left + 1 < min) {
                min = right - left + 1;
                res = s.substring(left, right + 1);
            }
            char lc = s.charAt(left);
            if (target[lc] >= 0) {
                cnt++;
            }
            target[lc]++;
            left++;
        }
    }


    return res;
}
```
