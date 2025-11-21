# 917 Reverse Only Letters

Given a string`S`, return the "reversed" string where all characters that are not a letter stay in the same place, and all letters reverse their positions.

**Example 1:**

```
Input: "ab-cd"
Output:"dc-ba"
```

**Example 2:**

```
Input: "a-bC-dEf-ghIj"
Output: "j-Ih-gfE-dCba"
```

**Example 3:**

```
Input: "Test1ng-Leet=code-Q!"
Output: "Qedo1ct-eeLg=ntse-T!"
```

**Note:**

1. `S.length <= 100`
2. `33 <= S[i].ASCIIcode <= 122`
3. `S`doesn't contain`\`or`"`

就是改改partition模板

```java
public String reverseOnlyLetters(String S) {
    if (S == null || S.length() == 0) {
        return S;
    }

    char[] cs = S.toCharArray();
    int left = 0;
    int right = cs.length - 1;

    while (left < right) {
        while (left < right && !Character.isLetter(cs[left])) {
            left++;
        }

        while (left < right && !Character.isLetter(cs[right])) {
            right--;
        }

        if (left < right) {
            char tmp = cs[left];
            cs[left] = cs[right];
            cs[right] = tmp;
            left++;
            right--;
        }        
    }

    return new String(cs);
}
```
