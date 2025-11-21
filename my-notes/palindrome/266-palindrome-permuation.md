# 266 Palindrome Permuation

Given a string, determine if a permutation of the string could form a palindrome.

For example,\
`"code"`-> False,`"aab"`-> True,`"carerac"`-> True.

**Hint:**

1. Consider the palindromes of odd vs even length. What difference do you notice?
2. Count the frequency of each character.
3. If each character occurs even number of times, then it must be a palindrome. How about character which occurs odd number of times?

这题我的解法并不是最优的。用了hashmap去数每个字母出现的次数。如果odd出现大于1次，表示这个string不能组成palindrome。好一点的解法，可以用hashset。如果现在看到的char已经存在在set中，我们把它从set中移除；如果不在，就加进去。最后判断一下set是否为空，后者size ==1。因为重复元素互相抵消了，所以最后剩下1个奇数的char或者没有奇数的char表示能组成palindrome。

解法一：T:O(n)，S:O(n)

```java
public boolean canPermutePalindrome(String s) {
    if (s == null || s.length() == 0) {
        return false;
    }

    HashMap<Character, Integer> hm = new HashMap<>();
    boolean singleExist = false;
    for (int i = 0; i < s.length(); i++) {
        char c = s.charAt(i);
        if (hm.containsKey(c)) {
            hm.put(c, hm.get(c) + 1);
        } else {
            hm.put(c, 1);
        }
    }

    for (Map.Entry<Character, Integer> e : hm.entrySet()) {
        if (e.getValue() % 2 != 0) {
            if (singleExist) {
                return false;
            }
            singleExist = true;
        }
    }

    return true;
}
```

解法二：T:O(n)，S:O(n)

```java
public boolean canPermutePalindrome(String s) {
    if (s == null || s.length() == 0) {
        return false;
    }

    HashSet<Character> hs = new HashSet<>();
    for (Character c : s.toCharArray()) {
        if (hs.contains(c)) {
            hs.remove(c);
        } else {
            hs.add(c);
        }
    }

    return hs.size() <= 1;
}
```
