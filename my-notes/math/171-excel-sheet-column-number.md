# 171 Excel Sheet Column Number

Related to question[Excel Sheet Column Title](https://leetcode.com/problems/excel-sheet-column-title/)

Given a column title as appear in an Excel sheet, return its corresponding column number.

For example:

```
    A -> 1
    B -> 2
    C -> 3
    ...
    Z -> 26
    AA -> 27
    AB -> 28
```

感觉就是进制转换。T:O(n),S:O(n)主要是用了hashmap存那26个字母对应的数字。可以通过减‘a'来直接算，那样的话会S:O(1)

```java
public int titleToNumber(String s) {
    if (s == null || s.isEmpty()) {
        return 0;
    }

    HashMap<Character, Integer> charToInt = new HashMap<>();
    for (int i = 1, c = 'A'; i <=26; i++, c++) {
        charToInt.put((char) c, i);
    }

    int res = 0;
    for (int i = 0; i < s.length(); i++) {
        res = res * 26 + charToInt.get(s.charAt(i));
    }        
    return res;
}
```

```java
public int titleToNumber(String s) {
    if (s == null || s.isEmpty()) {
        return -1;
    }

    int res = 0;
    for (int i = 0; i < s.length(); i++) {
        int cur = (int)(s.charAt(i) - 'A' + 1);
        res = res * 26 + cur;
    }

    return res;
}
```
