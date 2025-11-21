# 3 Longest Substring Without Repeating Characters

Given a string, find the length of the **longest substring** without repeating characters.

**Examples:**

Given`"abcabcbb"`, the answer is`"abc"`, which the length is 3.

Given`"bbbbb"`, the answer is`"b"`, with the length of 1.

Given`"pwwkew"`, the answer is`"wke"`, with the length of 3. Note that the answer must be a **substring**,`"pwke"`is asubsequenceand not a substring.

两年之后二刷发现其实我们可以只用set不用map，因为right的位置一直往右挪，所以不用记录

```java
public int lengthOfLongestSubstring(String s) {
    if (s == null || s.length() == 0) {
        return 0;
    }        

    Set<Character> set = new HashSet<>();
    int max = 0;
    int left = 0;
    for (int right = 0; right < s.length(); right++) {
        char rc = s.charAt(right);                             
        while (set.contains(rc)) {
            set.remove(s.charAt(left++));
        }             
        max = Math.max(max, right - left + 1);
        set.add(rc);            
    }

    return max;
}
```

两年之后的版本：

```java
public int lengthOfLongestSubstring(String s) {
    if (s == null || s.length() == 0) {
        return 0;
    }

    Map<Character, Integer> hm = new HashMap<>();
    int max = 0;
    int left = 0;
    for (int right = 0; right < s.length(); right++) {            
        char rc = s.charAt(right);
        while (hm.containsKey(rc)) { //这里要while，把左下标移到位                
            hm.remove(s.charAt(left++));
        } 
        hm.put(rc, right);                          
        max = Math.max(max, right - left + 1);
    }

    return max;
}
```

其实，用hashmap可以做这一题的一个follow up，如过每个字母允许出现最多k次，那么我们就把数量记录在value里，key还是那个字母，如果小于k我们就继续，大于等于，我们就开始踢数。这里贴一个9章的伪代码：

```java
public int lenghtOfLongestSubstring(String s) {
    int[] map = new int[256];// 如果是原题，用boolean
    
    int i = 0;
    int j = 0;
    int ans = 0;
    for (int i = 0; i < s.length; i++) {
        while (j < s.length() && map[s.charAt(j)] < k) {
            map[s.charAt(j)]++;//原题 = true
            ans = Math.max(ans, j - i + 1);
            j++;
        }
        map[s.charAt(i)]--;// 原题=false
    }
    
    return ans;
}
```



need to update left to the right value.

eg. abba

at line 14, left will be update to 2 when the 2nd b appear

when we encounter the last a, left will be update to 1 if we don't take max (which is less than left's current value 2) we should move the whole window to the right so we should not let the left bound "go backward"

```java
public int lengthOfLongestSubstring(String s) {
        if (s == null || s.length() == 0) {
            return 0;
        }

        // <char, location>，每次记录位置
        HashMap<Character, Integer> hm = new HashMap<>();
        int maxLen = 1;
        int left = 0;
        for (int right = 0; right < s.length(); right++) {
            Character rightChar = s.charAt(right);
            if (hm.containsKey(rightChar)) {//一直往右移，如果发现出现了用hashmap的内容更新左边界
                left = Math.max(hm.get(rightChar) + 1, left);//不过得注意一下，因为有时候更新的左坐标会比现在窗口的小
            }
            int curLen = right - left + 1;
            maxLen = Math.max(maxLen, curLen);
            hm.put(rightChar, right);//把当前的char和坐标放进hashmap里
        }


        return maxLen;
    }
```

也可以一段一段地找，先派j往前走，再把i跟上。这样就不用max来判断了。

```java
public int lengthOfLongestSubstring(String s) {
    if (s == null || s.length() == 0) {
        return 0;
    }

    char[] cs = s.toCharArray();
    int max = 1;
    int i = 0;
    int j = 0;
    int len = cs.length;
    while (i < len) {
        j = i;
        int curlen = 0;
        HashMap<Character, Integer> map = new HashMap<>();
        while (j < len) {
            if (!map.containsKey(cs[j])) {
                map.put(cs[j], j);
                curlen++;
                j++;
            } else {
                i = map.get(cs[j]);
                break;
            }
        }
        max = Math.max(max, curlen);
        i++;
    }

    return max;
}
```
