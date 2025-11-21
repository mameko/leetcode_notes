# 771 Jewels and Stones

You're given strings`J`representing the types of stones that are jewels, and`S`representing the stones you have. Each character in`S`is a type of stone you have. You want to know how many of the stones you have are also jewels.

The letters in`J`are guaranteed distinct, and all characters in`J`and`S`are letters. Letters are case sensitive, so`"a"`is considered a different type of stone from`"A"`.

**Example 1:**

```
Input: J = "aA", S = "aAAbbbb"
Output: 3
```

**Example 2:**

```
Input: J = "z", S = "ZZ"
Output: 0
```

**Note:**

* `S`and`J`will consist of letters and have length at most 50.
* The characters in`J`are distinct.

这题感觉没什么悬念。就是HashSet直接上。

```java
public int numJewelsInStones(String J, String S) {
    if (J == null || J.length() == 0 || S == null || S.length() == 0) {
        return 0;
    }

    Set<Character> js = new HashSet<>();
    for (int i = 0; i < J.length(); i++) {
        js.add(J.charAt(i));
    }

    int cnt = 0;
    for (int i = 0; i < S.length(); i++) {
        if (js.contains(S.charAt(i))) {
            cnt++;
        }
    }

    return cnt;
}
```
