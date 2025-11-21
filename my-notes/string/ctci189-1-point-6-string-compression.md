# 443 String Compression & ctci189 1 point 6 String Compression

## 443 String Compression

Given an array of characters, compress it [**in-place**](https://en.wikipedia.org/wiki/In-place_algorithm).

The length after compression must always be smaller than or equal to the original array.

Every element of the array should be a**character**(not int) of length 1.

After you are done **modifying the input array** [**in-place**](https://en.wikipedia.org/wiki/In-place_algorithm), return the new length of the array.

**Follow up:**&#x43;ould you solve it using only O(1) extra space?

**Example 1:**

```
Input:["a","a","b","b","c","c","c"]
Output:Return 6, and the first 6 characters of the input array should be: ["a","2","b","2","c","3"]

Explanation:"aa" is replaced by "a2". "bb" is replaced by "b2". "ccc" is replaced by "c3".
```

**Example 2:**

```
Input:["a"]
Output:Return 1, and the first 1 characters of the input array should be: ["a"]

Explanation:Nothing is replaced.
```

**Example 3:**

```
Input:["a","b","b","b","b","b","b","b","b","b","b","b","b"]
Output:Return 4, and the first 4 characters of the input array should be: ["a","b","1","2"].

Explanation:
Since the character "a" does not repeat, it is not compressed. "bbbbbbbbbbbb" is replaced by "b12".
Notice each digit has it's own entry in the array.
```

**Note:**

1. All characters have an ASCII value in`[35, 126]`.
2. `1 <= len(chars) <= 1000`.

边界高难度，这里主要还得考虑双位数的输出方法。12=》1，2

```java
public int compress(char[] chars) {
    if (chars == null || chars.length == 0) {
        return 0;
    }

    int left = 0;
    int right = 0;

    while (right < chars.length && left < chars.length) {            
        char last = chars[right];
        int cnt = 0;
        while (right < chars.length && chars[right] == last) {
            cnt++;                
            right++;
        }

        chars[left++] = last;         
        if (left >= chars.length) break; // 要注意越界
        if (cnt > 1) {// 把双位数输出               
            String cntS = cnt + "";
            int i = 0;
            while (i < cntS.length()) {
                chars[left++] = cntS.charAt(i++);     
            }
        }                         
    }

    return left;
}
```

## ctci189 1 point 6 String Compression

String Compression: Implement a method to perform basic string compression using the counts of repeated characters. For example, the string aabcccccaaa would become a2blc5a3. If the "compressed" string would not become smaller than the original string, your method should return the original string. You can assume the string has only uppercase and lowercase letters (a - z).

题目不是很难，就是得注意边界条件。

cc的标准答案：

```java
String compress(String str) {
    StringBuilder compressed= new StringBuilder();
    int countConsecutive = 0;
    for (int i= 0; i < str.length(); i++) {
        countConsecutive++;

        /* If next character is different than current, append this char to result.*/
        if (i + 1 >= str.length() I I str.charAt(i) != str.charAt(i + 1)) {
            compressed.append(str.charAt(i));
            compressed.append(countConsecutive);
            countConsecutive = 0;
        }
    }
    return compressed.length() < str.length() ? compressed.toString() : str;
}
```

我的代码：

```java
public String stringCompress(String s) {
    if (s ==null || s.length()==0) {
        return "emptySring";
    }

    char last = s.charAt(0);
    int count = 1;
    StringBuilder sb = new StringBuilder();
    sb.append(last);

    for (int i = 1; i < s.length(); i++) {
        char c = s.charAt(i);
        if (c == last) {
            count++;
        } else {
            last = c;
            sb.append(count);
            sb.append(last);
            count = 1;
        }
    }

    sb.append(count);
    if (s.length() <= sb.length()) {
        return s;
    } else {
        return sb.toString();
    }

}
```
