# 243 Shortest Word Distance

Given a list of words and two word&#x73;_&#x77;ord1\_and\_word2_, return the shortest distance between these two words in the list.

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

觉得两年前面L家的时候虽然是把S:O(1),T:O(N)过一遍的解法背下来了但没吃透这一题，。先贴那个。

```java
public int shortestDistance(String[] words, String word1, String word2) {
    if (words == null || words.length == 0 || word1 == null || word2 == null) {
        return 0;
    }

    int p1 = -1;
    int p2 = -1;
    int min = Integer.MAX_VALUE;

    for (int i = 0; i < words.length; i++) {
        if (p1 < i && words[i].equals(word1)) {
            p1 = i;
        } else if (p2 < i && words[i].equals(word2)) {
            p2 = i;
        } 
        if (p1 > -1 && p2 > -1) {
            int diff = Math.abs(p1 - p2);
            min = Math.min(min, diff);
        }
    }

    return min;
}
```

如今二刷发现这题水很深。最intuitive的是把位置存起来。然后这题就变成了two pointer求两个sorted array里的min diff了。T:O(N) 因为过了两遍，S:O(N)因为用了hashmap存了位置。

```java
public int shortestDistance(String[] words, String word1, String word2) {
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
        minDiff = Math.min(minDiff, curDiff);
    }

    return minDiff;
}
```
