# 245 Shortest Word Distance III

Given a list of words and two word&#x73;_&#x77;ord1\_and\_word2_, return the shortest distance between these two words in the list.

\_word1\_and\_word2\_may be the same and they represent two individual words in the list.

**Example:**\
Assume that words =`["practice", "makes", "perfect", "coding", "makes"]`.

```
Input:word1 = “makes”, word2 = “coding”
Output: 1
```

```
Input:word1 = "makes", word2 = "makes"
Output: 3
```

**Note:**\
You may assume\_word1\_and\_word2\_are both in the list.

其实这题还是能继续沿用244 Shortest Word Distance II的解法的。就是在判断的时候加一个条件就能跳过相同的数字，不把0更新进去。

```java
public int shortestWordDistance(String[] words, String word1, String word2) {
    if (words == null || words.length == 0 || word1 == null || word1.length() == 0 || word2 == null || word2.length() == 0) {
        return -1;
    }

    // <word, List_of_location>
    Map<String, List<Integer>> wordMap = new HashMap<>();
    for (int i = 0; i < words.length; i++) {
        String curWord = words[i];
        wordMap.putIfAbsent(curWord, new ArrayList<>());
        wordMap.get(curWord).add(i);
    }

    // min diff of 2 sorted array
    List<Integer> word1Loc = wordMap.get(word1);
    List<Integer> word2Loc = wordMap.get(word2);
    int p1 = 0;
    int p2 = 0;
    int minDiff = Integer.MAX_VALUE;
    while (p1 < word1Loc.size() && p2 < word2Loc.size()) {
        int curDiff = Math.abs(word1Loc.get(p1) - word2Loc.get(p2));
        if (word1Loc.get(p1) < word2Loc.get(p2)) {
            p1++;
        } else {
            p2++;
        }
        if (curDiff > 0) { // 判断一下，如果相同就跳过
            minDiff = Math.min(minDiff, curDiff);
        }
    }

    return minDiff;
}
```

当然这题也有像243 Shortest Word Distance那样的S：O(1)的解法。当然还是没懂

```java
public int shortestWordDistance(String[] words, String word1, String word2) {
    if (words == null || words.length == 0 || word1 == null || word2 == null) {
        return 0;
    }

    int p1 = -1;
    int p2 = -1;
    int tmp = p1; // if 2 words are the same, use tmp to track the last position
    int min = Integer.MAX_VALUE;

    for (int i = 0; i < words.length; i++) {
        tmp = p1;// record previous position
        if (words[i].equals(word1)) {
            p1 = i;
        } 

        if (words[i].equals(word2)) {
            p2 = i;
        }

        if (p1 > -1 && p2 > -1) {
            if (word1.equals(word2) && tmp != p1 && tmp != -1) {
                int diff = Math.abs(p1 - tmp);
                min = Math.min(min, diff);
            } else if (p1 != p2) {// do update when 2 words are not the same
                int diff = Math.abs(p1 - p2);
                min = Math.min(min, diff);
            }
        }
    }

    return min;
}
```
