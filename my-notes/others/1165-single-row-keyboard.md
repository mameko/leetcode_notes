# 1165 Single-Row Keyboard

There is a special keyboard with **all keys in a single row**.

Given a string `keyboard` of length 26 indicating the layout of the keyboard (indexed from 0 to 25), initially your finger is at index 0. To type a character, you have to move your finger to the index of the desired character. The time taken to move your finger from index `i` to index `j` is `|i - j|`.

You want to type a string `word`. Write a function to calculate how much time it takes to type it with one finger.

**Example 1:**

```
Input: keyboard = "abcdefghijklmnopqrstuvwxyz", word = "cba"
Output: 4
Explanation: The index moves from 0 to 2 to write 'c' then to 1 to write 'b' then to 0 again to write 'a'.
Total time = 2 + 1 + 1 = 4. 
```

**Example 2:**

```
Input: keyboard = "pqrstuvwxyzabcdefghijklmno", word = "leetcode"
Output: 73
```

**Constraints:**

* `keyboard.length == 26`
* `keyboard` contains each English lowercase letter exactly once in some order.
* `1 <= word.length <= 10^4`
* `word[i]` is an English lowercase letter.

这题，一看，还以为有什么快速的方法。然后看了提示，发现还真是老老实实地一步一步做。T:O(n), S:O(n)

```java
public int calculateTime(String keyboard, String word) {
    if (keyboard == null || keyboard.length() < 26 || word == null || word.isEmpty()) {
        return 0;
    }

    // <char, loc>
    Map<Character, Integer> map = new HashMap<>();
    for (int i = 0; i < keyboard.length(); i++) {
        map.put(keyboard.charAt(i), i);
    }

    int sum = 0;
    int prev = 0;
    for (int next = 0; next < word.length(); next++) {
        int nextLoc = map.get(word.charAt(next));
        int dis = Math.abs(nextLoc - prev);
        sum += dis;
        prev = nextLoc;
    }

    return sum;
}
```
