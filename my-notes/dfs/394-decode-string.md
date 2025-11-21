# 394 Decode String

Given an encoded string, return it's decoded string.

The encoding rule is:`k[encoded_string]`, where theencoded\_stringinside the square brackets is being repeated exactlyktimes. Note thatkis guaranteed to be a positive integer.

You may assume that the input string is always valid; No extra white spaces, square brackets are well-formed, etc.

Furthermore, you may assume that the original data does not contain any digits and that digits are only for those repeat numbers,k. For example, there won't be input like`3a`or`2[4]`.

**Examples:**

```
s = "3[a]2[bc]", return "aaabcbc".
s = "3[a2[c]]", return "accaccacc".
s = "2[abc]3[cd]ef", return "abcabccdcdcdef".
```

```java
public String expressionExpand(String s) {
        if (s == null || s.length() == 0) {
            return s;
        }

    StringBuilder res = new StringBuilder();
    int n = s.length();
    int times = 0;
        for (int i = 0; i < n; i++) {
        char c = s.charAt(i);
        if (Character.isAlphabetic(c)) {
            res.append(c);
        } else {
            if (s.charAt(i + 1) != '[') {
                times = times * 10 + Character.getNumericValue(c);
            } else {
                int j = i + 2;
                Stack<Character> stack = new Stack<>();
                stack.push('[');
                while (!stack.isEmpty()) {
                    if (s.charAt(j) == ']') {
                        stack.pop();
                    } else if (s.charAt(j) == '[') {
                        stack.push('[');
                    }
                    j++;
                }
                times = times * 10 + Character.getNumericValue(c);
                // cut the content between [] and recur
                String sub = s.substring(i + 2, j - 1);
                for (int k = 0; k < times; k++) {
                    String tmp = expressionExpand(sub);
                    res.append(tmp);
                }
                i = j - 1;
                times = 0;
            }
        }
        }    

        return res.toString();
    }
```
