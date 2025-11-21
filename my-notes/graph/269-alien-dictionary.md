# 269 Alien Dictionary

There is a new alien language which uses the latin alphabet. However, the order among letters are unknown to you. You receive a list of **non-empty** words from the dictionary, where **words are sorted lexicographically by the rules of this new language**. Derive the order of letters in this language.

**Example 1:**\
Given the following words in dictionary,

```
[
  "wrt",
  "wrf",
  "er",
  "ett",
  "rftt"
]
```

The correct order is:`"wertf"`.

**Example 2:**\
Given the following words in dictionary,

```
[
  "z",
  "x"
]
```

The correct order is:`"zx"`.

**Example 3:**\
Given the following words in dictionary,

```
[
  "z",
  "x",
  "z"
]
```

The order is invalid, so return`""`.

**Note:**

1. You may assume all letters are in lowercase.
2. You may assume that if a is a prefix of b, then a must appear before b in the given dictionary.
3. If the order is invalid, return an empty string.
4. There may be multiple valid order of letters, return any one of them is fine.

又过了几年，又去上九章，又被归纳了。这是alphabetical topo sort的典型题。topo sort这一步的唯一不同是，我们用priorityqueue，每次按char的字母表顺序来topo sort。同样难点还是build graph。

```java
public String alienOrder(String[] words) {
    if (words == null || words.length == 0) {
        return "";
    }

    // build a graph for characters
    Map<Character, Set<Character>> graph = buildGraph(words);

    if (graph.isEmpty()) {
        return "";
    }

    // topo sort alphabetically
    StringBuilder result = topoSort(graph);
    return result.toString();
}

private Map<Character, Set<Character>> buildGraph(String[] words) {
    Map<Character, Set<Character>> graph = new HashMap<>();
    // add all chars to graph
    for (String word : words) {
        for (int i = 0; i < word.length(); i++) {
            Character curChar = word.charAt(i);
            if (graph.containsKey(curChar)) {
                continue;
            }
            graph.put(curChar, new HashSet<>());
        }
    }

    // add edges
    for (int i = 0; i < words.length - 1; i++) {
        String word1 = words[i];
        String word2 = words[i + 1];

        int index = 0;
        while(index < word1.length() && index < word2.length()) {
            char c1 = word1.charAt(index);
            char c2 = word2.charAt(index);
            if (c1 != c2) {
                graph.get(c1).add(c2);
                break;
            }
            index++;
        }

        // to prevent ["abc", "ab"]
        if (index == Math.min(word1.length(), word2.length())) {
            if (word1.length() > word2.length()) {
                return new HashMap<>();
            }
        }
    }
    return graph;
}

private Map<Character, Integer> getIndegree(Map<Character, Set<Character>> graph) {
    Map<Character, Integer> indegree = new HashMap<>();
    for (Character ch : graph.keySet()) {
        indegree.put(ch, 0);
    }

    for (Character ch : graph.keySet()) {
        Set<Character> neis = graph.get(ch);
        for (Character nei : neis) {
            indegree.put(nei, indegree.getOrDefault(nei, 0) + 1);
        }
    }

    return indegree;
}

private StringBuilder topoSort(Map<Character, Set<Character>> graph) {
    Map<Character, Integer> indegree = getIndegree(graph);
    Queue<Character> queue = new PriorityQueue<>();
    StringBuilder result = new StringBuilder();

    // put indegree 0 node to queue
    for (Character ch : indegree.keySet()) {
        if (indegree.get(ch) == 0) {
            queue.offer(ch);
        }
    }

    // no indegree 0 node, can't topo sort
    if (queue.size() == 0) {
        return null;
    }

    while (!queue.isEmpty()) {
        Character curChar = queue.poll();
        result.append(curChar);

        Set<Character> neis = graph.get(curChar);
        for (Character nei : neis) {
            indegree.put(nei, indegree.get(nei) - 1);
            if (indegree.get(nei) == 0) {
                queue.offer(nei);
            }
        }
    }

    // not able to finish topo sort
    if (graph.size() != result.length()) {
        return new StringBuilder();
    }

    return result;
}
```



这题的难点主要是怎么把所给的dictionary变成graph，这里变成graph以后就可以直接topo sort找顺序了。在把字典变成图的时候要注意什么时候该加indegree，什么时候该加edge，别加重复了。留意注释。

```java
public String alienOrder(String[] words) {
    if (words == null || words.length == 0) {
        return "";
    }

    if (words.length == 1) {
        return words[0];
    }

    Map<Character, Integer> indegree = new HashMap<>();
    Map<Character, Set<Character>> edges = new HashMap<>();
    Set<Character> exist = new HashSet<>();

    // make graph & fill indegrees
    for (int i = 0; i < words.length - 1; i++)  {
        String w1 = words[i];
        String w2 = words[i + 1];

        int len = Math.min(w1.length(), w2.length());
        int j = 0;
        boolean found = false;
        while (j < len) {
            char c1 = w1.charAt(j);
            char c2 = w2.charAt(j);
            exist.add(c1);
            exist.add(c2);
            if (c1 != c2 && !found) {
                found = true;// once we found diff, we set the flag

                // add edge
                Set<Character> tmp;
                if (edges.containsKey(c1)) {
                    tmp = edges.get(c1);
                    if (tmp.contains(c2)) {//skip adding dup edges & indegrees
                        j++;
                        continue;
                    }
                } else {
                    tmp = new HashSet<>();
                }
                tmp.add(c2);
                edges.put(c1, tmp);

                // add indegree
                if (indegree.containsKey(c2)) {
                    indegree.put(c2, indegree.get(c2) + 1);
                } else {
                    indegree.put(c2, 1);
                }
            }
            j++;
        }

        // add rest of char in word to exist
        if (w1.length() == len) {
            while (j < w2.length()) {
                exist.add(w2.charAt(j++));
            }
        } else {
            while (j < w1.length()) {
                exist.add(w1.charAt(j++));
            }
        }
    }

    // do topo sort
    Queue<Character> q = new LinkedList<>();
    StringBuilder sb = new StringBuilder();

    // add indegree = 0 to result & queue
    for (Character ch : exist) {
        if (!indegree.containsKey(ch)) {
            q.offer(ch);
            sb.append(ch);
        }
    }

    while (!q.isEmpty()) {
        char cur = q.poll();

        if (!edges.containsKey(cur)) {
            continue;
        }

        for (Character nei : edges.get(cur)) {
            indegree.put(nei, indegree.get(nei) - 1);
            if (indegree.get(nei) == 0) {
                indegree.remove(nei);
                q.offer(nei);
                sb.append(nei);
            }
        }
    }

    if (indegree.size() != 0) {
        return "";
    }

    return sb.toString();
}
```
