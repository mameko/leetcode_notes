# L1870 Number of Substrings with All Zeroes

Given a string `str` containing only `0` or `1`, please return the number of substrings that consist of `0` .

**Example 1:**

```
Input:"00010011"
Output:9
Explanation:
There are 5 substrings of "0",
There are 3 substrings of "00",
There is 1 substring of "000".
So return 9
```

**Example 2:**

```
Input:"010010"
Output:5
```

这题感觉有点像数学，一开始我把1个0，2个0，3个0，4个0，5个0的substring总数列出来，1，3，6，10，15，发现了规律，就是每次都是上一个加现在的数字. 譬如，5个0的substring就是，1 + 2 + 3 + 4 + 5 = (1 + 5) \* 5 / 2.然后通向双指针来找0的section。

后来看了九章老师的答案，发现，其实3个0的情况可以直接算，000，譬如，i指向第一个0，j指向最后一个0的下一位。每个循环可以把j的加到ans里，所以是1 + 2 + 3。

```java
public int stringCount(String str) {
    if (str == null || str.isEmpty()) {
        return 0;
    }

    int i = 0; 
    int j = 0;
    int res = 0;
    int len = str.length();
    while (i < len) {
        if (str.charAt(i) == '1') {
            i++;
        }
        j = i;
        while (j < len && str.charAt(j) == '0') {
            j++;
        }
        int zeroLen = j - i;
        res = res + zeroLen * (1 + zeroLen) / 2;
        i = j;
    }

    return res;
}

// 九章答案
public int stringCount(String str) {
    if (str == null) {
        return 0;
    }
    
    int j = 1, answer = 0;
    for (int i = 0; i < str.length(); i++) {
        if (str.charAt(i) != '0') {
            continue;
        }
        
        j = Math.max(j, i + 1);
        while (j < str.length() && str.charAt(j) == '0') {
            j++;
        }
        answer += j - i;
    }
    
    return answer;
}
```
