# 168 Excel Sheet Column Title

Given a positive integer, return its corresponding column title as appear in an Excel sheet.

For example:

```
    1 -> A
    2 -> B
    3 -> C
    ...
    26 -> Z
    27 -> AA
    28 -> AB
```

这题没看起来那么容易，可以用10进制转2进制的方法。但难点在于26 -> z， 但我们一%，就会变成0.看了答案才知道，解决方法是每次进入循环先把n减一，然后append的时候加上'A‘（这是1）。例如：26，我们减减再mod一下得到25然后加上'A‘（这是1），刚好是26（Z）。这个1想了我半天。

```java
    public String convertToTitle(int n) {
        if (n < 1) {
            return "";
        }

        StringBuilder sb = new StringBuilder();

        while (n > 0) {
            n--;
            sb.append((char)('A' + (n % 26)));
            n = n / 26;
        }

        sb = sb.reverse();
        return sb.toString();
    }
```
