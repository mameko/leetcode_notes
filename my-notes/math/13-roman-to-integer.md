# 13 Roman to Integer

Given a roman numeral, convert it to an integer.

Input is guaranteed to be within the range from 1 to 3999.

如果前面一个字母比后面小，减去，相反的话加上。然后第一个数字的处理比较巧妙，这里直接把小于的情况用0减去，所以是IV的话，res在i=0之后会变成-1，然后加上5就等于正确结果4.如果VI的话，res在i=0后会变成1，然后加上5，同样得到正确结果6.至于怎么想到的真的不知道。T:O(n), S:O(1)

```java
public int romanToInt(String s) {
    if (s == null || s.isEmpty()) {
        return 0;
    }   

    Map<Character, Integer> hm = new HashMap<>();
    hm.put('I', 1);
    hm.put('V', 5);
    hm.put('X', 10);
    hm.put('L', 50);
    hm.put('C', 100);
    hm.put('D', 500);
    hm.put('M', 1000);

    int res = 0;
    for (int i = 0; i < s.length(); i++) {
        int cur = hm.get(s.charAt(i));
        if (i == s.length() - 1 || hm.get(s.charAt(i + 1)) <= cur ) {
            res += cur;
        } else {
            res -= cur;
        }
    }
    return res;
}
```
