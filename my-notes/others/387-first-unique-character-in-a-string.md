# 387 First Unique Character in a String

Given a string, find the first non-repeating character in it and return it's index. If it doesn't exist, return -1.

**Examples:**

```
s = "leetcode"
return 0.

s = "loveleetcode",
return 2.
```

**Note:**&#x59;ou may assume the string contain only lowercase letters.

这题，不知道为毛想得那么复杂，用了hashmap来记录每个char的位置，然后用record数组来记录是否重复了。而且是从后面开始往前看。然后再从开始往后看，找到第一个没有重复的字母的下标，然后返回。

```java
public int firstUniqChar(String s) {
    if (s == null || s.isEmpty()) {
        return -1;
    }

    int n = s.length();
    // <char, loc>
    HashMap<Character, Integer> exist = new HashMap<>();
    boolean[] record = new boolean[n];

    // process dup from back
    for (int i = n - 1; i >= 0; i--) {
        char c = s.charAt(i);
        if (!exist.containsKey(c)) {
            record[i] = true;
            exist.put(c, i);
        } else {
            record[exist.get(c)] = false;
        }
    }

    // find first from front
    for (int i = 0; i < n; i++) {
        if (record[i]) {
            return i;
        }
    }

    return -1;
}
```

下面贴一个大神简易版：就用一个hashmap数了数目，然后从头开始找，找到第一个在hashmap里数目是1的返回下标。

```java
public int firstUniqChar(String s) {
    int freq [] = new int[26];
    for(int i = 0; i < s.length(); i ++)
        freq [s.charAt(i) - 'a'] ++;
    for(int i = 0; i < s.length(); i ++)
        if(freq [s.charAt(i) - 'a'] == 1)
            return i;
    return -1;
}

//二刷，java8版
public int firstUniqChar(String s) {
    if (s == null || s.length() == 0) {
        return -1;
    }

    Map<Character, Integer> map = new HashMap<>();
    for (int i = 0; i < s.length(); i++) {
        char cur = s.charAt(i);
        map.put(cur, map.getOrDefault(cur, 0) + 1);            
    }

    for (int i = 0; i < s.length(); i++) {
        if (map.get(s.charAt(i)) == 1) {
            return i;
        }
    }

    return -1;
}
```

这题L家有个follow up，如果输入是stream怎么办？初步想了一下，估计得用LRU那种结构。首先弄一个队列doublylinkedlist，这样就能知道顺序了，每次见到新的char放队尾，最后返回队首就ok了。然后我们需要一种方式快速找到dup，所以用一个set来存已经dup的char，如果下一个扫进来的已经重复了我们就skip不加list里。然后，我们还需要快速删除某一节点的功能，所以要用一个hashmap来存每个点的位置\<char, nodeLoc>。
