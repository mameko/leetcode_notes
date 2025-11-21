# 244 Shortest Word Distance II

Design a class which receives a list of words in the constructor, and implements a method that takes two words\_word1\_and\_word2\_and return the shortest distance between these two words in the list. Your method will be called\_repeatedly\_many times with different parameters.

**Example:**\
Assume that words =`["practice", "makes", "perfect", "coding", "makes"]`.

```
Input:word1 = “coding”, word2 = “practice”
Output: 3
```

```
Input:word1 = "makes", word2 = "coding"
Output: 1
```

**Note:**\
You may assume tha&#x74;_&#x77;ord1_**does not equal to**_word2_, and\_word1\_and\_word2\_are both in the list.

其实就是把243 Shortest Word Distance的解法拆开2步。两年前做出了解法，但看不出套路。

```java
// <word, list_of_loc>
Map<String, List<Integer>> wordMap;
public WordDistance(String[] words) {
    if (words == null || words.length == 0) {
        // throw exception
    }
    wordMap = new HashMap<>();
    for (int i = 0; i < words.length; i++) {
        String curWord = words[i];
        wordMap.putIfAbsent(curWord, new ArrayList<>());
        wordMap.get(curWord).add(i);
    }
}

public int shortest(String word1, String word2) {
    if (word1 == null || word1.length() == 0 || word2 == null || word2.length() == 0) {
        return -1;
    }

    // min diff in two sorted array
    List<Integer> word1Loc = wordMap.get(word1);
    List<Integer> word2Loc = wordMap.get(word2);
    int p1 = 0;
    int p2 = 0;
    int minDis = Integer.MAX_VALUE;
    while (p1 < word1Loc.size() && p2 < word2Loc.size()) {
        int curDis = Math.abs(word1Loc.get(p1) - word2Loc.get(p2));
        if (word1Loc.get(p1) < word2Loc.get(p2)) {
            p1++;
        } else {
            p2++;
        }
        minDis = Math.min(minDis, curDis);
    }
    return minDis;
}
```
