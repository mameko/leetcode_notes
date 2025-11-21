# 12 Integer to Roman

Given an integer, convert it to a roman numeral.

Input is guaranteed to be within the range from 1 to 3999.

没啥技巧可言，知道了罗马数字转换的做法就很简单。不过记得break。

```java
    public String intToRoman(int num) {
        if (num < 1 || num > 3999) {
            return "";
        }

        int[] val    = {1000, 900, 500, 400, 100, 90, 50, 40, 10, 9, 5, 4, 1};
        String[] str = {"M", "CM", "D", "CD", "C", "XC", "L", "XL", "X", "IX", "V", "IV", "I"};
        StringBuilder sb = new StringBuilder();

        while (num > 0) {
            for (int i = 0; i < val.length; i++) {
                if (num >= val[i]) {
                    sb.append(str[i]);
                    num = num - val[i];
                    break;
                }
            }
        }

        return sb.toString();
    }
```
