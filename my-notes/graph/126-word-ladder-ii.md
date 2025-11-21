# 126 Word Ladder II

Given two words (startandend), and a dictionary, find all shortest transformation sequence(s) fromstarttoend, such that:

1. Only one letter can be changed at a time
2. Each intermediate word must exist in the dictionary

## Notice

* All words have the same length.
* All words contain only lowercase alphabetic characters.

**Example**

Given:\
start=`"hit"`\
end=`"cog"`\
dict=`["hot","dot","dog","lot","log"]`

Return

```
  [
    ["hit","hot","dot","dog","cog"],
    ["hit","hot","lot","log","cog"]
  ]
```

好难，也没什么好说，就是先bfs找距离，然后再dfs找所有这个长度的路径。

```java
public List<List<String>> findLadders(String start, String end, Set<String> dict) {
    List<List<String>> res = new ArrayList<>();
    if (start == null || end == null || start.length() == 0 || end.length() == 0 || dict == null) {
        return res;
    }

    if (start.equals(end)) {
        ArrayList<String> tmp = new ArrayList<>();
        tmp.add(start);
        res.add(tmp);
        return res;
    }

    dict.add(start);
    dict.add(end);

    HashMap<String, Integer> visitedAndDis = new HashMap<>();
    HashMap<String, ArrayList<String>> neighborListRev = new HashMap<>();

    bfs(start, end, dict, neighborListRev, visitedAndDis);

    ArrayList<String> path = new ArrayList<>();
    dfs(end, start, visitedAndDis, neighborListRev, res, path);

    return res;
}

private void dfs(String cur, String first, HashMap<String, Integer> visitedAndDis, HashMap<String, ArrayList<String>> neighborListRev, List<List<String>> res, ArrayList<String> path) {

    path.add(cur);
    if (cur.equals(first)) {
        Collections.reverse(path);
        res.add(new ArrayList<>(path));
        Collections.reverse(path);
    } else {
        for (String nei : neighborListRev.get(cur)) {
            if (visitedAndDis.containsKey(nei) && (visitedAndDis.get(nei) == (visitedAndDis.get(cur) - 1))) {
                dfs(nei, first, visitedAndDis, neighborListRev, res, path);
            }
        }
    }
    path.remove(path.size() - 1);
}

private void bfs(String start, String end, Set<String> dict, HashMap<String, ArrayList<String>> neighborListRev,
            HashMap<String, Integer> visitedAndDis) {
        Queue<String> queue = new LinkedList<>();
        queue.offer(start);
        visitedAndDis.put(start, 1);

        for (String s : dict) {
            neighborListRev.put(s, new ArrayList<String>());
        }

        while (!queue.isEmpty()) {
            int size = queue.size();
            for (int i = 0; i < size; i++) {
                String cur = queue.poll();
                ArrayList<String> nei = findNei(cur, dict);
                for (String n : nei) {
                    neighborListRev.get(n).add(cur);
                    if (!visitedAndDis.containsKey(n)) {
                        int distance = visitedAndDis.get(cur) + 1;
                        visitedAndDis.put(n, distance);
                        queue.offer(n);
                    }
                }
            }
        }
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

在leet上的题目要求有点不一样，如果end不在字典里的话，直接返回空：

```java
public class Solution {
    public List<List<String>> findLadders(String beginWord, String endWord, List<String> wordList) {
        List<List<String>> res = new ArrayList<>();
        if (wordList == null || wordList.size() == 0) {
            return res;
        }

        if (!wordList.contains(endWord)) {
            return res;
        }

        HashSet<String> dict = new HashSet<>();
        for (String s : wordList) {
            dict.add(s);
        }

        HashMap<String, Integer> visitedAndDis = new HashMap<>();
        HashMap<String, List<String>> neiRevList = new HashMap<>();

        // bfs from start to end find distance
        bfs(visitedAndDis, neiRevList, dict, beginWord, endWord);

        // dfs end to start find all the results
        List<String> path = new ArrayList<>();
        dfs(res, visitedAndDis, neiRevList, dict, beginWord, endWord, path);

        return res;
    }

    private void dfs(List<List<String>> res, HashMap<String, Integer> visitedAndDis, HashMap<String, List<String>> neiRevList, HashSet<String> wordList, String goal, String cur, List<String> path) {

        path.add(cur);
        if (cur.equals(goal)) {
            Collections.reverse(path);
            res.add(new ArrayList<>(path));   
            Collections.reverse(path);
        } else {
            for (String nei : neiRevList.get(cur)) {
                if (visitedAndDis.containsKey(nei) && (visitedAndDis.get(nei) == visitedAndDis.get(cur) - 1)) {
                    dfs(res, visitedAndDis, neiRevList, wordList, goal, nei, path);       
                }
            }
        }
        path.remove(path.size() - 1);
    }

    private void bfs(HashMap<String, Integer> visitedAndDis, HashMap<String, List<String>> neiRevList, HashSet<String> wordList, String start, String end) {
        Queue<String> queue = new LinkedList<>();
        queue.offer(start);
        visitedAndDis.put(start, 0);

        // wordList.add(start);
        wordList.add(end);

        for (String w : wordList) {
            neiRevList.put(w, new ArrayList<>());
        }

        while (!queue.isEmpty()) {
            String curWord = queue.poll();

            List<String> neiList = getNei(wordList, curWord);
            for (String nei : neiList) {
                neiRevList.get(nei).add(curWord);
                if (!visitedAndDis.containsKey(nei)) {
                    int dis = visitedAndDis.get(curWord) + 1;
                    visitedAndDis.put(nei, dis);
                    queue.offer(nei);
                }
            }

        }

        return;
    }

    private List<String> getNei(HashSet<String> wordList, String cur) {
        List<String> res = new ArrayList<>();

        char[] cs = cur.toCharArray();
        for (int i = 0; i < cs.length; i++) {
            for (char c = 'a'; c <= 'z'; c++) {
                char tmp = cs[i];
                if (cs[i] != c) {
                    cs[i] = c;
                } else {
                    continue;
                }

                String newWord = new String(cs);
                if (wordList.contains(newWord)) {
                    res.add(newWord);
                }

                cs[i] = tmp;
            }
        }

        return res;
    }
}
```

还有一种听说很快的bi -bfs：

```java
  public List<List<String>> findLadders(String start, String end, Set<String> dict) {
    // hash set for both ends
    Set<String> set1 = new HashSet<String>();
    Set<String> set2 = new HashSet<String>();

    // initial words in both ends
    set1.add(start);
    set2.add(end);

    // we use a map to help construct the final result
    Map<String, List<String>> map = new HashMap<String, List<String>>();

    // build the map
    helper(dict, set1, set2, map, false);

    List<List<String>> res = new ArrayList<List<String>>();
    List<String> sol = new ArrayList<String>(Arrays.asList(start));

    // recursively build the final result
    generateList(start, end, map, sol, res);

    return res;
  }

  boolean helper(Set<String> dict, Set<String> set1, Set<String> set2, Map<String, List<String>> map, boolean flip) {
    if (set1.isEmpty()) {
      return false;
    }

    if (set1.size() > set2.size()) {
      return helper(dict, set2, set1, map, !flip);
    }

    // remove words on current both ends from the dict
    dict.removeAll(set1);
    dict.removeAll(set2);

    // as we only need the shortest paths
    // we use a boolean value help early termination
    boolean done = false;

    // set for the next level
    Set<String> set = new HashSet<String>();

    // for each string in end 1
    for (String str : set1) {
      for (int i = 0; i < str.length(); i++) {
        char[] chars = str.toCharArray();

        // change one character for every position
        for (char ch = 'a'; ch <= 'z'; ch++) {
          chars[i] = ch;

          String word = new String(chars);

          // make sure we construct the tree in the correct direction
          String key = flip ? word : str;
          String val = flip ? str : word;

          List<String> list = map.containsKey(key) ? map.get(key) : new ArrayList<String>();

          if (set2.contains(word)) {
            done = true;

            list.add(val);
            map.put(key, list);
          } 

          if (!done && dict.contains(word)) {
            set.add(word);

            list.add(val);
            map.put(key, list);
          }
        }
      }
    }

    // early terminate if done is true
    return done || helper(dict, set2, set, map, !flip);
  }

  void generateList(String start, String end, Map<String, List<String>> map, List<String> sol, List<List<String>> res) {
    if (start.equals(end)) {
      res.add(new ArrayList<String>(sol));
      return;
    }

    // need this check in case the diff between start and end happens to be one
    // e.g "a", "c", {"a", "b", "c"}
    if (!map.containsKey(start)) {
      return;
    }

    for (String word : map.get(start)) {
      sol.add(word);
      generateList(word, end, map, sol, res);
      sol.remove(sol.size() - 1);
    }
  }
```
