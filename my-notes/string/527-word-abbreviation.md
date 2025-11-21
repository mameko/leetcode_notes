# 527 Word Abbreviation

Given an array of n distinct non-empty strings, you need to generate**minimal**possible abbreviations for every word following rules below.

1. Begin with the first character and then the number of characters abbreviated, which followed by the last character.
2. If there are any conflict, that is more than one words share the same abbreviation, a longer prefix is used instead of only the first character until making the map from word to abbreviation become unique. In other words, a final abbreviation cannot map to more than one original words.
3. If the abbreviation doesn't make the word shorter, then keep it as original.

**Example:**

```
Input: ["like", "god", "internal", "me", "internet", "interval", "intension", "face", "intrusion"]
Output: ["l2e","god","internal","me","i6t","interval","inte4n","f2e","intr4n"]
```

**Note:**

1. Both n and the length of each word will not exceed 400.
2. The length of each word is greater than 1.
3. The words consist of lowercase English letters only.
4. The return answers should be **in the same order** as the original array.

这个是从discuss里找的答案，跟我原始版本的感觉挺像，用了一个makeAbbr函数，传进去开始缩小的位置。这里楼主每次查重，然后通过不断循环来把重复的变长，然后在check有没有重复，知道没有重复的abbreviation为止。然后返回结果。我想应该有一次就能搞定的方法，不过没通过OJ...

```java
public List<String> wordsAbbreviation(List<String> dict) {
    int len=dict.size();
    String[] ans=new String[len];
    int[] prefix=new int[len];
    for (int i=0;i<len;i++) {
        prefix[i]=1;
        ans[i]=makeAbbr(dict.get(i), 1); // make abbreviation for each string
    }
    for (int i=0;i<len;i++) {
        while (true) {
            HashSet<Integer> set=new HashSet<>();
            for (int j=i+1;j<len;j++) {//注意这里存的是下标
                if (ans[j].equals(ans[i])) set.add(j); // check all strings with the same abbreviation
            }
            if (set.isEmpty()) break;
            set.add(i);
            for (int k: set) 
                ans[k]=makeAbbr(dict.get(k), ++prefix[k]); // increase the prefix
        }
    }
    return Arrays.asList(ans);
}

private String makeAbbr(String s, int k) {
    if (k>=s.length()-2) return s;
    StringBuilder builder=new StringBuilder();
    builder.append(s.substring(0, k));
    builder.append(s.length()-1-k);
    builder.append(s.charAt(s.length()-1));
    return builder.toString();
}
```

另一种解法是九章老师教的，用的是hashmap来数字符串的个数，如果字符串出现数目大于1，表示有重复，我们就继续增长前续缩进数目。hashmap里存的是缩进了的字符和那个字符出现的数目。感觉会多用点空间。

```java
public String[] wordsAbbreviation(String[] dict) {
    // write your code here
    if (dict == null || dict.length == 0) {
        return null;
    }

    int len = dict.length;
    String[] res = new String[len];
    int[] pre = new int[len];
    HashMap<String, Integer> map = new HashMap<>();
    for (int i = 0; i < len; i++) {
        pre[i] = 1;
        res[i] = getAbbr(dict[i], pre[i]);
        map.put(res[i], map.getOrDefault(res[i], 0) + 1);
    }

    while(true) {
        boolean unique = true;// 每次假定我们得到结果
        for (int i = 0; i < len; i++) { // loop一遍看看
            if (map.getOrDefault(res[i], 0) > 1) { // 发现还有重复
                res[i] = getAbbr(dict[i], ++pre[i]); // 少缩一点
                map.put(res[i], map.getOrDefault(res[i], 0) + 1);// 放进去map里等下一轮
                unique = false; // 设一下条件表示我们还得继续
            }
        }
        if (unique) { // 最后判断一下是否得到结果，跳出while循环
            break;
        }
    }

    return res;
}

private String getAbbr(String org, int pre) {
    StringBuilder sb = new StringBuilder();
    if (org.length() <= 2 + pre) {
        return org;
    } else {
        sb.append(org.substring(0, pre));
        sb.append(org.length() - pre - 1);
        sb.append(org.charAt(org.length() - 1));
        return sb.toString();
    }
}
```
