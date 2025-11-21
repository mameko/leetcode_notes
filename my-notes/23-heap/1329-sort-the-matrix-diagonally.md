# 1329 Sort the Matrix Diagonally

A **matrix diagonal** is a diagonal line of cells starting from some cell in either the topmost row or leftmost column and going in the bottom-right direction until reaching the matrix's end. For example, the **matrix diagonal** starting from `mat[2][0]`, where `mat` is a `6 x 3` matrix, includes cells `mat[2][0]`, `mat[3][1]`, and `mat[4][2]`.

Given an `m x n` matrix `mat` of integers, sort each **matrix diagonal** in ascending order and return _the resulting matrix_.

**Example 1:**![](https://assets.leetcode.com/uploads/2020/01/21/1482_example_1_2.png)

```
Input: mat = [[3,3,1,1],[2,2,1,2],[1,1,1,2]]
Output: [[1,1,1,1],[1,2,2,2],[1,2,3,3]]
```

**Constraints:**

* `m == mat.length`
* `n == mat[i].length`
* `1 <= m, n <= 100`
* `1 <= mat[i][j] <= 100`

这题本来以为有什么规律，想了3秒没想出来，然后看提示了。看完以后，就知道是用heap了。但具体怎么用还得看到第三个hint才知道。主要是把diagonal的数字归纳到一个heap里。因为遍历时候我们从上到下，从左到右。所以放数字的时候，不用特别处理。T：O(nmlog(min(m,n))，两层循环mn，然后heap sort 对角线长度\*log对角线长度，然后对角线长度，不会超过n或者m。所以min。S：O(mn)，heaps最后把整个matrix装进去了

```java
public int[][] diagonalSort(int[][] mat) {
    if (mat == null || mat.length == 0 || mat[0].length == 0) {
        return mat;
    }

    int n = mat.length;
    int m = mat[0].length;

    Map<Integer, PriorityQueue<Integer>> map = new HashMap<>();
    for (int i = 0; i < n; i++) {
        for (int j = 0; j < m; j++) {
            int diff = i - j;
            if (!map.containsKey(diff)) {
              map.put(diff, new PriorityQueue<>());
            } 
            map.get(diff).offer(mat[i][j]);
        }
    }

    // 直接拿出来用就ok了，遍历从左到右，上到下，所以小到大排序刚刚好
    for (int i = 0; i < n; i++) {
        for (int j = 0; j < m; j++) {
            int diff = i - j;
            int val = map.get(diff).poll();
            mat[i][j] = val;
        }
    }

    return mat;
}
```

这题也可以用int\[]数组数频率，不一定要用map，因为只有26个字母。

```java
public boolean closeStrings(String word1, String word2) {
    if (word1.length() != word2.length()) {
        return false;
    }
    int word1Map[] = new int[26];
    int word2Map[] = new int[26];
    for (char c : word1.toCharArray()) {
        word1Map[c - 'a']++;
    }
    for (char c : word2.toCharArray()) {
        word2Map[c - 'a']++;
    }
    for (int i = 0; i < 26; i++) {
        if ((word1Map[i] == 0 && word2Map[i] > 0) ||
            (word2Map[i] == 0 && word1Map[i] > 0)) {
            return false;
        }
    }
    Arrays.sort(word1Map);
    Arrays.sort(word2Map);
    return Arrays.equals(word1Map, word2Map);
}
```

我们还可以利用一个integer来存keySet()的字母表。.

![](<../../.gitbook/assets/image (21).png>)

其实复杂度并没有减少，但这个思想可以借鉴

```java
public boolean closeStrings(String word1, String word2) {
    if (word1.length() != word2.length()) {
        return false;
    }
    int word1Map[] = new int[26];
    int word2Map[] = new int[26];
    int word1Bit = 0;
    int word2Bit = 0;
    for (char c : word1.toCharArray()) {
        word1Map[c - 'a']++;
        word1Bit = word1Bit | (1 << (c - 'a'));
    }
    for (char c : word2.toCharArray()) {
        word2Map[c - 'a']++;
        word2Bit = word2Bit | (1 << (c - 'a'));
    }
    if (word1Bit != word2Bit) {
        return false;
    }
    Arrays.sort(word1Map);
    Arrays.sort(word2Map);
    for (int i = 0; i < 26; i++) {
        if (word1Map[i] != word2Map[i]) {
            return false;
        }
    }
    return true;
}
```
