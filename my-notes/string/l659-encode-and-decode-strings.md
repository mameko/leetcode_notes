# L659 Encode and Decode Strings

Design an algorithm to encode**a list of strings**to**a string**. The encoded string is then sent over the network and is decoded back to the original list of strings.

Machine 1 (sender) has the function:

```
string encode(vector<string> strs) {
  // ... your code
  return encoded_string;
}
```

Machine 2 (receiver) has the function:

```
vector<string> decode(string s) {
  //... your code
  return strs;
}
```

So Machine 1 does:

```
string encoded_string = encode(strs);
```

and Machine 2 does:

```
vector<string> strs2 = decode(encoded_string);
```

`strs2`in Machine 2 should be the same as`strs`in Machine 1.

Implement the`encode`and`decode`methods.

**Note:**

* The string may contain any possible characters out of 256 valid ascii characters. Your algorithm should be generalized enough to work on any possible characters.
* Do not use class member/global/static variables to store states. Your encode and decode algorithms should be stateless.
* Do not rely on any library method such as`eval`or serialize methods. You should implement your own encode/decode algorithm.

一年前，蜜汁解题...才发现没写下来..真那么高频吗？而且完全不知道自己写毛...看了一阵子，好像是把string list里的每个string拿出来，算个长度然后加个斜杠然后把string贴上去。例如，\["abc", null, "de", "", "k"] =》 3/abc0/n2/de0/e1/k

```java
// Encodes a list of strings to a single string.
public String encode(List<String> strs) {
    if (strs == null || strs.size() == 0) {
        return null;
    }

    StringBuilder sb = new StringBuilder();
    for (String str : strs) {
        if (str == null || str.length() == 0) {
            sb.append("0");
            sb.append("/");
            if (str == null) {
                sb.append("n");
            } else {
                sb.append("e");
            }
        } else {
            sb.append(str.length());
            sb.append("/");
            sb.append(str);
        }
    }
    return sb.toString();
}

public List<String> decode(String str) {
    List<String> res = new ArrayList<>();
    if (str == null || str.length() == 0) {
        return res;
    }

    int n = str.length();
    int start = 0;
    int end = 0;
    while (start < n) {
        int loc = str.indexOf('/', start);
        if (loc < 0) {
            break;
        }

        int len = Integer.valueOf(str.substring(start, loc));
        if (len == 0) {
            char c = str.charAt(loc + 1);
            String part = c == 'n' ? null : "";
            res.add(part);
            start = loc + 2;
        } else {
            start = loc + 1;
            end = start + len;
            String part = str.substring(start, end);
            res.add(part);
            start = end;
        }
    }
    return res;
}
```

另一种编码方式是像正常的escape字符那样，用：：代表：，用：；代表两个string的分隔。因为如果用：，或者空格或者其他字符的话，本来的string list里都有可能出现那种字符，所以需要用escape字符来处理。

```java
/*
 * @param strs: a list of strings
 * @return: encodes a list of strings to a single string.
 */
public String encode(List<String> strs) {
    // write your code here
    if (strs == null || strs.size() == 0) {
        return "";
    }

    StringBuilder sb = new StringBuilder();
    for (String s : strs) {
        char[] sc = s.toCharArray();
        for (char c : sc) {
            if (c == ':') {
                sb.append(':');
            } 
            sb.append(c);
        }
        sb.append(":;");
    }

    return sb.toString();
}

/*
 * @param str: A string
 * @return: dcodes a single string to a list of strings
 */
 public List<String> decode(String s) {
    List<String> res = new ArrayList<>();
    if (s == null || s.length() == 0) {
        return res;
    }
    // 这里用split来分开":;"是不行的，有些[""]的test case不过
    int i = 0;
    StringBuilder sb = new StringBuilder();
    char[] sc = s.toCharArray();
    while (i < sc.length) {
        if (sc[i] == ':') {
            if (sc[i + 1] == ';') {
                res.add(sb.toString());
                sb = new StringBuilder();
                i += 2;
            } else {
                sb.append(':');
                i += 2;
            }
        } else {
            sb.append(sc[i]);
            i++;
        }
    }

    return res;
}

// 下面的做法不行：
public List<String> decode(String str) {
    // write your code here
    List<String> res = new ArrayList<>();
    if (str == null || str.length() == 0) {
        return res;
    }

    String[] parts = str.split(":;");
    for (String part : parts) {
        StringBuilder sb = new StringBuilder();
        char[] pc = part.toCharArray();
        for (int i = 0; i < pc.length; i++) {
            if (pc[i] != ':') {
                sb.append(pc[i]);
            } else {
                // ：：=》 ：，所以跳过一个
                i++;
            }
        }
        res.add(sb.toString());
    }

    return res;
}
```
