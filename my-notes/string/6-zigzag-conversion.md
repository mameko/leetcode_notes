# 6 ZigZag Conversion

The string`"PAYPALISHIRING"`is written in a zigzag pattern on a given number of rows like this: (you may want to display this pattern in a fixed font for better legibility)

```
P   A   H   N
A P L S I I G
Y   I   R
```

And then read line by line:`"PAHNAPLSIIGYIR"`

Write the code that will take a string and make this conversion given a number of rows:

```
string convert(string text, int nRows);
```

`convert("PAYPALISHIRING", 3)`should return`"PAHNAPLSIIGYIR"`

这题主要是找规律，发现首先得每一节的长度，这个等于2n - 2。然后，第一行和最后一行每一节只有一个字母，所以得分开处理。中间的行里面每节有两个。所以得多加一个。append第二个的时候得注意是否已经超出长度，如果是的话，我们开始append下一行的。这题把n=2，3，4，5写出来以后就能发现规律，难点在于边界的控制。

```java
public String convert(String s, int numRows) {
        if (s == null || s.isEmpty() || numRows < 2) {
            return s;
        }

        int len = s.length();
        int segmentLen = 2 * numRows - 2;
        StringBuilder sb = new StringBuilder();
        for (int i = 0; i < numRows; i++) {
            int seg = segmentLen - 2 * i; 
            for (int j = i; j < len; j += segmentLen) {
                sb.append(s.charAt(j));
                if (j + seg >= len) {
                    break;
                }

                if (i == 0 || i == numRows - 1) {
                    continue;
                }
                sb.append(s.charAt(j + seg));
            }           
        }
        return sb.toString();
    }
```

.
