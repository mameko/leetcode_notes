# 524 Longest Word in Dictionary through Deleting

Given a string and a string dictionary, find the longest string in the dictionary that can be formed by deleting some characters of the given string. If there are more than one possible results, return the longest word with the smallest lexicographical order. If there is no possible result, return the empty string.

**Example 1:**

```
Input:
s = "abpcplea", d = ["ale","apple","monkey","plea"]

Output: 
"apple"
```

**Example 2:**

```
Input:
s = "abpcplea", d = ["a","b","c"]

Output: 
"a"
```

**Note:**

1. All the strings in the input will only contain lower-case letters.
2. The size of the dictionary won't exceed 1,000.
3. The length of all the strings in the input won't exceed 1,000.

这题我只能想到排完序brute force找是不是subseqence。T:O(nlogn + nx)，这里subsequnce要O(x)的时间，所以最坏情况下要n次。（还是脑残了）

其实呢，还可以不排序，每次isSubsequnce以后比较已经找到的产犊和次序。T:O(n\*x), S:O(x)

```java
public String findLongestWord(String s, List<String> d) {
    if (s == null || s.isEmpty() || d == null || d.isEmpty()) {
        return "";
    }

    Collections.sort(d, (s1, s2) -> {
       if (s1.length() == s2.length()) {
           return s1.compareTo(s2);
       } else {
           return s2.length() - s1.length();
       }
    });

    for (String str : d) {
        if (match(str, s)) {
            return str;
        }
    }

    return "";
}

private boolean match(String str, String target) {
    if (str.length() > target.length()) {
        return false;
    }

    int i = 0;
    int j = 0;
    while (i < str.length() && j < target.length()) {
        int cur = str.charAt(i);

        if (cur != target.charAt(j)) {
            j++;
        } else {
            i++;
            j++;
        }

    }

    return i == str.length();
}

// 参考答案
public boolean isSubsequence(String x, String y) {
    int j = 0;
    for (int i = 0; i < y.length() && j < x.length(); i++)
        if (x.charAt(j) == y.charAt(i))
            j++;
    return j == x.length();
}
public String findLongestWord(String s, List < String > d) {
    Collections.sort(d, new Comparator < String > () {
        public int compare(String s1, String s2) {
            return s2.length() != s1.length() ? s2.length() - s1.length() : s1.compareTo(s2);
        }
    });
    for (String str: d) {
        if (isSubsequence(str, s))
            return str;
    }
    return "";
}

// 参考答案二
public boolean isSubsequence(String x, String y) {
    int j = 0;
    for (int i = 0; i < y.length() && j < x.length(); i++)
        if (x.charAt(j) == y.charAt(i))
            j++;
    return j == x.length();
}
public String findLongestWord(String s, List < String > d) {
    String max_str = "";
    for (String str: d) {
        if (isSubsequence(str, s)) {
            if (str.length() > max_str.length() || (str.length() == max_str.length() && str.compareTo(max_str) < 0))
                max_str = str;
        }
    }
    return max_str;
}
```
