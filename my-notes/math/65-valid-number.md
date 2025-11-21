# 65 Valid Number

Validate if a given string can be interpreted as a decimal number.

Some examples:\
`"0"`=>`true`\
`" 0.1 "`=>`true`\
`"abc"`=>`false`\
`"1 a"`=>`false`\
`"2e10"`=>`true`\
`" -90e3 "`=>`true`\
`" 1e"`=>`false`\
`"e3"`=>`false`\
`" 6e-1"`=>`true`\
`" 99e2.5 "`=>`false`\
`"53.5e93"`=>`true`\
`" --6 "`=>`false`\
`"-+3"`=>`false`\
`"95a54e53"`=>`false`

**Note:**&#x49;t is intended for the problem statement to be ambiguous. You should gather all requirements up front before implementing one. However, here is a list of characters that can be in a valid decimal number:

* Numbers 0-9
* Exponent - "e"
* Positive/negative sign - "+"/"-"
* Decimal point - "."

Of course, the context of these characters also matters in the input.

这题不是老师讲感觉这辈子都不会做。主要做法是，先判断+/-号，然后数字+‘.'，最后’e'的部分。返回时注意判断是否整条string都parse掉。然后在判断‘e'的部分时，记得e后面要有数字。如果没加数字就已经到末尾的话，直接返回false。

这题需要注意的地方是：

1. trim完以后得判断一下是否已经为空，为空return false。例如：“　　　”
2. 每个while的地方都得判断i是否越界
3. 小数点大于1 & 数字小于1。例如:"."

```java
public boolean isNumber(String s) {
    if (s == null) {
        return false;
    }
    // 把首尾空格砍掉
    s = s.trim();
    int len = s.length();
    // 砍完发现为空串，返回false
    if (len == 0) {
        return false;
    }

    char[] sc = s.toCharArray();
    int i = 0;

    if (sc[i] == '+' || sc[i] == '-') {
        i++;
    }

    int digCnt = 0;
    int pntCnt = 0;
    while(i < len && (Character.isDigit(sc[i]) || sc[i] == '.')) {
        if (Character.isDigit(sc[i])) {
            digCnt++;
        } else {
            pntCnt++;
        }
        i++;
    }
    // 如果有2个小数点，或者没有数字的话，返回false
    if (pntCnt > 1 || digCnt < 1) {
        return false;
    }

    // 判断e部分是否valid
    if (i < len && sc[i] == 'e') {
        i++;  

        if (i < len && (sc[i] == '+' || sc[i] == '-')) {
            i++;
        }
        // ’e'后面要有数字，如果这就到结尾了，返回false
        if (i == len) {
            return false;
        }
        // 看e后面的数字是否valid
        while (i < len && Character.isDigit(sc[i])) {
            i++;
        }
    }
    return i == len;
}
```
