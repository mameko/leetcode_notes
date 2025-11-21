# 288 Unique Word Abbreviation

## 288 Unique Word Abbreviation (L648)

An abbreviation of a word follows the form. Below are some examples of word abbreviations:

```
a) it                      --> it    (no abbreviation)     1
b) d|o|g                   --> d1g

              1    1  1
     1---5----0----5--8
c) i|nternationalizatio|n  --> i18n

              1
     1---5----0
d) l|ocalizatio|n          --> l10n
```

Assume you have a dictionary and given a word, find whether its abbreviation is unique in the dictionary. A word's abbreviation is unique if no other word from the dictionary has the same abbreviation.

## Example

Given dictionary =`[ "deer", "door", "cake", "card" ]`\
isUnique("dear") // return false\
isUnique("cart") // return true\
isUnique("cane") // return false\
isUnique("make") // return true

这题要注意的是如果字典中有多个cake出现，那么当判断isUnique的时候是否返回true。这个题需要跟面试官讨论。这里我们假设是， 就是如果字典有\["cake", "cake"] isUnique("cake")返回true。我们可以发现，如果字典中某单词出现的个数跟abbr之后出现的个数相同的话，证明这个词的abbr是unique的。

```java
HashMap<String, Integer> org;
HashMap<String, Integer> abbr;

public ValidWordAbbr(String[] dictionary) {
    // do intialization if necessary
    org = new HashMap<>();
    abbr = new HashMap<>();
    for (String str : dictionary) {
        org.put(str, org.getOrDefault(str, 0) + 1);
        String abb = getAbbr(str);
        abbr.put(abb, abbr.getOrDefault(abb, 0) + 1);
    }
}

/*
 * @param word: a string
 * @return: true if its abbreviation is unique or false
 */
public boolean isUnique(String word) {
    String target = getAbbr(word);
    return org.get(word) == abbr.get(target);
}

private String getAbbr(String str) {
    if (str.length() < 3) {
        return str;
    }
    StringBuilder sb = new StringBuilder();
    sb.append(str.charAt(0));
    sb.append(str.length() - 2);
    sb.append(str.charAt(str.length() - 1));
    return sb.toString();    
}

/× 这个还能写简单点
private String getAbbr(String str) {
    if (str.length() < 3) {
        return str;
    }
    return str.charAt(0) + String.valueOf(str.length() - 2) + str.charAt(str.length() - 1);
}
×/
```
