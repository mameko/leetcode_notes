# 1657 Determine if Two Strings Are Close

Two strings are considered **close** if you can attain one from the other using the following operations:

* Operation 1: Swap any two **existing** characters.
  * For example, `abcde -> aecdb`
* Operation 2: Transform **every** occurrence of one **existing** character into another **existing** character, and do the same with the other character.
  * For example, `aacabb -> bbcbaa` (all `a`'s turn into `b`'s, and all `b`'s turn into `a`'s)

You can use the operations on either string as many times as necessary.

Given two strings, `word1` and `word2`, return `true` _if_ `word1` _and_ `word2` _are **close**, and_ `false` _otherwise._

**Example 1:**

```
Input: word1 = "abc", word2 = "bca"
Output: true
Explanation: You can attain word2 from word1 in 2 operations.
Apply Operation 1: "abc" -> "acb"
Apply Operation 1: "acb" -> "bca"
```

**Example 2:**

```
Input: word1 = "a", word2 = "aa"
Output: false
Explanation: It is impossible to attain word2 from word1, or vice versa, in any number of operations.
```

**Example 3:**

```
Input: word1 = "cabbba", word2 = "abbccc"
Output: true
Explanation: You can attain word2 from word1 in 3 operations.
Apply Operation 1: "cabbba" -> "caabbb"
Apply Operation 2: "caabbb" -> "baaccc"
Apply Operation 2: "baaccc" -> "abbccc"
```

**Example 4:**

```
Input: word1 = "cabbba", word2 = "aabbss"
Output: false
Explanation: It is impossible to attain word2 from word1, or vice versa, in any amount of operations.
```

**Constraints:**

* `1 <= word1.length, word2.length <= 105`
* `word1` and `word2` contain only lowercase English letters.

这题，看了题目以后，想了半天。在怀疑是不是用dp但又想不出递推式。后来看了hint发现，这个op1和op2都是障眼法。op1可以让任意字母互换，op2能让任意字母对应的频率互换。然后就想出了hashmap了。这里实现的时候需要注意一点，hashmap的values()方法返回的是collection，直接equals是不等的（即使同样位置同样数字），要用containsAll()来判断两个collection是不是互相包含。



```java
public boolean closeStrings(String word1, String word2) {
    if (word1 == null && word2 ==  null) {
        return true;
    } else if (word2 == null || word1 == null) {
        return false;
    } if (word1.length() != word2.length()) {
        return false;
    }

    Map<Character, Integer> freq1 = new HashMap<>();
    Map<Character, Integer> freq2 = new HashMap<>();

    for (char c : word1.toCharArray()) {
        freq1.put(c, freq1.getOrDefault(c, 0) + 1);
    }

    for (char c : word2.toCharArray()) {
        freq2.put(c, freq2.getOrDefault(c, 0) + 1);
    }

    if (freq1.keySet().equals(freq2.keySet()) 
     && freq1.values().containsAll(freq2.values()) 
     && freq2.values().containsAll(freq1.values())) {
        return true;
    } else {
        return false;
    }
```
