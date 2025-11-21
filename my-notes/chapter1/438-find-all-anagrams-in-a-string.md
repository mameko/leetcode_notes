# 438 Find All Anagrams in a String

Given a string**s**and a**non-empty**string**p**, find all the start indices of**p**'s anagrams in**s**.

Strings consists of lowercase English letters only and the length of both strings**s**and**p**will not be larger than 20,100.

The order of output does not matter.

**Example 1:**

```
Input:s: "cbaebabacd" p: "abc"
Output:[0, 6]

Explanation:
The substring with start index = 0 is "cba", which is an anagram of "abc".
The substring with start index = 6 is "bac", which is an anagram of "abc".
```

**Example 2:**

```
Input:s: "abab" p: "ab"
Output:[0, 1, 2]

Explanation:
The substring with start index = 0 is "ab", which is an anagram of "ab".
The substring with start index = 1 is "ba", which is an anagram of "ab".
The substring with start index = 2 is "ab", which is an anagram of "ab".
```

另一种解法是高频题班的。基本思想跟[242 Valid Anagram](../others/242-valid-anagram.md) 一样。用一个hashmap来数我们现在有多少个不同的字母。这里还做了一个优化，本来每次左边减一个，右边加一个，我们得把cnt数组加起来，看得到anagram没有。这样的T:O(256n)。这里用了absSum来优化这256.因为每次只添加和删除一个，我们首先把要删除的和添加的两个数的绝对值减掉，移动完以后再加上。然后判断是否得到0.如果是的话，证明我们找到了anagram，加进结果。

```java
public List<Integer> findAnagrams(String s, String p) {
    // write your code here
    List<Integer> res = new ArrayList<>();
    if (s == null || p == null) {
        return res;
    } else if (s.length() < p.length()) {
        return res;
    }

    int[] cnt = new int[256];
    char[] pc = p.toCharArray();
    char[] sc = s.toCharArray();
    int absSum = 0;
    for (int i = 0; i < pc.length; i++) {
        cnt[pc[i]]--; // 注意这里和下面要对应
        cnt[sc[i]]++; // p减 & s加，表示每次用s的cnt减去p的cnt
    }                 // 1. 如果这里反过来的话，（p++，s--）

    for(int item : cnt) {
        absSum += Math.abs(item);
    }

    if (absSum == 0) {
        res.add(0);
    }

    for (int i = p.length(); i < s.length(); i++) {
        int l = i - p.length();
        int r = i;

        absSum = absSum - Math.abs(cnt[sc[l]]) - Math.abs(cnt[sc[r]]);
        cnt[sc[l]]--; // 所以这里也得每次用s的cnt减去p的cnt
        cnt[sc[r]]++; // l--， r++， 2.这里也得反过来 （l++， r--）
        absSum = absSum + Math.abs(cnt[sc[l]]) + Math.abs(cnt[sc[r]]);

        if (absSum == 0) {
            res.add(l + 1);
        }
    }

    return res;
}
```

这题是Sliding Window的模板题，得背下来. 30多ms

```java
public List<Integer> findAnagrams(String s, String p) {
        List<Integer> result = new ArrayList<>();
        if (s == null || p == null || s.length() == 0 || p.length() == 0) {
            return result;
        }

        // 把每个char的数目存到targetMap里
        HashMap<Character, Integer> targetMap = new HashMap<>();
        for (int i = 0; i < p.length(); i++) {
            char cur = p.charAt(i);
            if (targetMap.containsKey(cur)) {
                targetMap.put(cur, targetMap.get(cur) + 1);
            } else {
                targetMap.put(cur, 1);
            }
        }

        // 这里开始sliding window
        int left = 0;
        int counter = p.length();// 初始化为目标字符串长度
        for (int right = 0; right < s.length(); right++) {
            char rightChar = s.charAt(right);
            if (targetMap.containsKey(rightChar)) {//先把窗口往右扩张
                if (targetMap.get(rightChar) >= 1) {//每找到一个，就看看窗口里是否已经包含足够个数
                    counter--;//是的话把counter--
                }
                // 然后把这个char的数目减一，如果减到负数表示在现在的window里这个char已经存在足够个数了，而且比需要的多
                targetMap.put(rightChar, targetMap.get(rightChar) - 1);
            }

            // 当counter减为0的时候，表示我们找到了一个anagram，把左坐标加进答案里
            if (counter == 0) {
                result.add(left);
            }

            // 然后当当前窗口size够大了以后，开始把左边界往后移，缩小窗口size
            if (right - left + 1 == p.length()) {
                char leftChar = s.charAt(left);
                if (targetMap.containsKey(leftChar)) {// 每找到一个在target里的字符
                    if (targetMap.get(leftChar) >= 0) {// 看一下是不是已经大于0，大于0表示这个char已经不在当前窗口内
                        counter++;//所以把counter+1，下次还得继续找
                    }
                    targetMap.put(leftChar, targetMap.get(leftChar) + 1);// 每个字符都加一个
                }
                left++;//左边界往后移，缩小窗口size
            }
        }
        return result;
```

换个int数组，快好多，9ms

```java
public List<Integer> findAnagrams(String s, String p) {
    List<Integer> res = new ArrayList<>();
    if (s == null || s.length() == 0 || p == null || p.length() == 0) {
        return res;
    }

    int[] target = new int[256];
    for (int i = 0; i < p.length(); i++) {
        target[p.charAt(i)]++;            
    }

    int cnt = p.length();                
    int left = 0;
    for (int right = 0; right < s.length(); right++) {
        char rc = s.charAt(right);            
        if (target[rc] > 0) {                    
            cnt--;
        }        
        target[rc]--;


        if (cnt == 0) {
            res.add(left);
        }

        while (right - left + 1 >= p.length()) { // 这一步while也行if也行，>=或==都行
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
