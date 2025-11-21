# L132 Word Search II

Given a matrix of lower alphabets and a dictionary. Find all words in the dictionary that can be found in the matrix. A word can start from any position in the matrix and go left/right/up/down to the adjacent position.

**Example**

Given matrix:

```
doaf
agai
dcan
```

and dictionary:

```
{"dog", "dad", "dgdg", "can", "again"}
```

return {"dog", "dad", "can", "again"}

dog:

```
doaf
agai
dcan
```

dad:

```
doaf
agai
dcan
```

can:

```
doaf
agai
dcan
```

again:

```
doaf
agai
dcan
```

[**Challenge**](http://www.lintcode.com/en/problem/word-search-ii/#challenge)

Using trie to implement your algorithm.

把字典建成trie，然后搜索时用来剪枝。还是递归。这里因为trie是看下一个节点，所以要注意off by one error。

写法二：刷第二次的时候，改了一下，简洁一点。

```java
public class Solution {
    int[] x = {0, 0, 1, -1};
    int[] y = {1, -1, 0, 0};
    public List<String> findWords(char[][] board, String[] words) {
        List<String> res = new ArrayList<>();
        if (board == null || board.length == 0 || board.length == 0 || words == null || words.length == 0) {
            return res;
        }

        TrieNode root = new TrieNode();
        for (String word : words) {
            root.add(word);
        }

        int n = board.length;
        int m = board[0].length;
        HashSet<String> resSet = new HashSet<>();
        for (int i = 0; i < n; i++) {
            for (int j = 0; j < m; j++) {
                boolean[][] visited = new boolean[n][m];
                StringBuilder tmp = new StringBuilder();
                dfsHelper(visited, resSet, root, board, i, j, tmp);
            }
        }

        res.addAll(resSet);
        return res;
    }

    private void dfsHelper(boolean[][] visited, Set<String> res, TrieNode root, char[][] board, int row, int col, StringBuilder tmp) {
        TrieNode currentNode = root.children.get(board[row][col]);
        if (visited[row][col] || currentNode == null) {
            return;
        }

        tmp.append(board[row][col]);
        visited[row][col] = true;

        if (currentNode.end == true) {// 这里要先加上这个字符，然后判断是否加进结果里。
            res.add(tmp.toString());// 还有，不能return，因为这个trie的branch下面可能还有合法的串要加上
        }

        for (int i = 0; i < 4; i++) {
            int ni = row + x[i];
            int nj = col + y[i];
            if (inBound(board, ni, nj)) {
                dfsHelper(visited, res, currentNode, board, ni, nj, tmp);
            }
        }
        visited[row][col] = false;
        tmp.deleteCharAt(tmp.length() - 1);
    }

    private boolean inBound(char[][] board, int row, int col) {
        if (row < 0 || col < 0 || row >= board.length || col >= board[0].length) {
            return false;
        }

        return true;
    }
}

class TrieNode {
    HashMap<Character, TrieNode> children = new HashMap<>();
    boolean end;

    public void add(String word) {
        if (word == null || word.isEmpty()) {
            return;
        }

        TrieNode last = this;
        for (int i = 0; i < word.length(); i++) {
            TrieNode cur = last.children.get(word.charAt(i));
            if (cur == null) {
                cur = new TrieNode();
                last.children.put(word.charAt(i), cur);
            }
            last = cur;
        }
        last.end = true;
    }
}
```

写法一：

```java
public class Solution {
    /**
     * @param board: A list of lists of character
     * @param words: A list of string
     * @return: A list of string
     */
    int[] x = {0, 0, 1, -1}; 
    int[] y = {1, -1, 0, 0};
    public ArrayList<String> wordSearchII(char[][] board, ArrayList<String> words) {
        ArrayList<String> res = new ArrayList<>();
        if (board == null || board.length == 0 || board[0].length == 0 || words == null || words.size() == 0) {
            return res;
        }

        //add all words to trie dictionary
        TrieNode root = new TrieNode();
        for (String s : words) {
            root.addWord(s);
        }

        HashSet<String> resSet = new HashSet<>();
        for (int i = 0; i < board.length; i++) {
            for (int j = 0; j < board[0].length; j++) {
                if (root.children.containsKey(board[i][j])) {
                    boolean[][] visited = new boolean[board.length][board[0].length];
                    StringBuilder sb = new StringBuilder();
                    sb.append(board[i][j]);
                    visited[i][j] = true;
                    searchdsf(root, i, j, sb, resSet, visited, board);
                }
            }
        }

        res.addAll(resSet);
        return res;
    }

    private void searchdsf(TrieNode node, int row, int col, StringBuilder sb, HashSet<String> res,
            boolean[][] visited, char[][] board) {
        TrieNode next = node.children.get(board[row][col]);
        if (next == null) {
            return;
        }

        if (next.isEnd) {
            res.add(sb.toString());
        }

        for (int i = 0; i < 4; i++) {
            int nexti = row + x[i];
            int nextj = col + y[i];

            if (inBound(nexti, nextj, board) && !visited[nexti][nextj]) {
                sb.append(board[nexti][nextj]);
                visited[nexti][nextj] = true;
                searchdsf(next, nexti, nextj, sb, res, visited, board);
                visited[nexti][nextj] = false;
                sb.deleteCharAt(sb.length() - 1);
            }

        }
    }


    private boolean inBound(int row, int col, char[][] board) {
        if (row >= board.length || row < 0 || col < 0 || col >= board[0].length) {
            return false;
        }

        return true;
    }
}

class TrieNode {
    HashMap<Character, TrieNode> children;
    boolean isEnd;

    public TrieNode() {
        children = new HashMap<>();
        isEnd = false;
    }

    public void addWord(String word) {
        if (word == null || word.isEmpty()) {
            return;
        }

        Character first = word.charAt(0);
        TrieNode child = children.get(first);
        if (child == null) {
            child = new TrieNode();
            children.put(first, child);
        }

        if (word.length() > 1) {
            child.addWord(word.substring(1));
        } else {
            child.isEnd = true;
        }
    }

}
```
