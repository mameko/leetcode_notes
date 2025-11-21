# 127 Word Ladder

Given two words (beginWordandendWord), and a dictionary's word list, find the length of shortest transformation sequence frombeginWordtoendWord, such that:

1. Only one letter can be changed at a time.
2. Each transformed word must exist in the word list. Note that beginWord is not a transformed word.

For example,

Given:\
beginWord=`"hit"`\
endWord=`"cog"`\
wordList=`["hot","dot","dog","lot","log","cog"]`

As one shortest transformation is`"hit" -> "hot" -> "dot" -> "dog" -> "cog"`,\
return its length`5`.

**Note:**

* Return 0 if there is no such transformation sequence.
* All words have the same length.
* All words contain only lowercase alphabetic characters.
* You may assume no duplicates in the word list.
* You may assume beginWord and endWord are non-empty and are not the same.

这题，有两个难点，1想到bfs，2怎么把word变成图的点连起来。这里提供两个算距离的方法，以供自己参考。还有两种concatinate那些单词的方法，第一种快点，因为第二种在leetcode超时了。

用level：

```java
public int ladderLength(String beginWord, String endWord, Set<String> wordList) {
    if (wordList == null || wordList.size() == 0) {
        return 0;
    }

    if (beginWord.equals(endWord)) {
        return 1;
    }

    wordList.add(beginWord);
    wordList.add(endWord);

    return bfs(beginWord, endWord, wordList);
}

private int bfs(String beginWord, String endWord, Set<String> wordList) {
    HashSet<String> visited = new HashSet<>();
    Queue<String> queue = new LinkedList<>();
    queue.offer(beginWord);
    visited.add(beginWord);
        int level = 1;

    while (!queue.isEmpty()) {
        int size = queue.size();
        level++;
        for (int i = 0; i < size; i++) {
            String curNode = queue.poll();
            List<String> neighbor = getNeighbor(curNode, wordList);
            for (String nei : neighbor) {
                if (visited.contains(nei)) {
                    continue;
                }
                if (nei.equals(endWord)) {
                    return level;
                }
                visited.add(nei);
                queue.offer(nei);

            }
        }
    }

    return 0;
}

private List<String> getNeighbor(String curNode, Set<String> wordList) {
    char[] cs = curNode.toCharArray();
    List<String> result = new ArrayList<>();
    for (int i = 0; i < cs.length; i++) {
        for (char c = 'a'; c <= 'z'; c++) {
            char tmp = cs[i];
            if (cs[i] == c) {
                continue;
            } else {
                cs[i] = c;
            }
            String nei = new String(cs);
            if (wordList.contains(nei)) {
                result.add(nei);
            }
            cs[i] = tmp;
        }
    }

    return result;
}
```

用distance hashmap：

```java
public int ladderLength(String start, String end, Set<String> dict) {
    if (start == null || end == null || start.length() == 0 || end.length() == 0 || dict == null) {
        return 0;
    }

    if (start.equals(end)) {
        return 1;
    }

    dict.add(start);
    dict.add(end);

    HashMap<String, Integer> visitedAndDis = new HashMap<>();
    Queue<String> queue = new LinkedList<>();
    queue.offer(start);
    visitedAndDis.put(start, 1);
    while (!queue.isEmpty()) {
        int size = queue.size();
        for (int i = 0; i < size; i++) {
            String cur = queue.poll();
            ArrayList<String> nei = findNei(cur, dict);
            for (String n : nei) {
                if (!visitedAndDis.containsKey(n)) {
                    int distance = visitedAndDis.get(cur) + 1;
                    visitedAndDis.put(n, distance);
                    if (n.equals(end)) {
                        return distance;
                    }
                    queue.offer(n);
                }
            }
        }
    }

    return Integer.MAX_VALUE;
}

private ArrayList<String> findNei(String cur, Set<String> dict) {
    ArrayList<String> nei = new ArrayList<String>();

    for (int i = 0; i < cur.length(); i++) {
        for (char c = 'a'; c <= 'z'; c++) {
            if (cur.charAt(i) == c) {
                continue;
            }
            String replaced = cur.substring(0, i) + c + cur.substring(i + 1);
            if (dict.contains(replaced)) {
                nei.add(replaced);
            }
        }
    }

    return nei;
}
```
