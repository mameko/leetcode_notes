# 95 Unique Binary Search Trees II

Given an intege&#x72;_&#x6E;_, generate all structurally unique **BST's** (binary search trees) that store values 1 ... _n_.

**Example:**

```
Input: 3
Output:
[
  [1,null,3,2],
  [3,2,null,1],
  [3,1,null,null,2],
  [2,1,3],
  [1,null,2,null,3]
]

Explanation:

The above output corresponds to the 5 unique BST's shown below:

   1         3     3      2      1
    \       /     /      / \      \
     3     2     1      1   3      2
    /     /       \                 \
   2     1         2                 3
```

这题因为看了96 Unique Binary Search Tree的解法，所以很容易想到了递归。而且感觉中间步骤会有很多重复所以用了memorization as well。但是呢，写得不是特别elegant，啊，还有，记得加新树的时候要造新的root，不然的话会shallow copy，答案不正确。复杂度的话，抄leetcode题解的，总结，没太懂。![](<../../.gitbook/assets/95 Unique Binary Search Trees II.PNG>)

```java
// 这个版本是根据solution改的，漂亮点。
public List<TreeNode> generateTrees(int n) {
    if (n < 1) {
        return new ArrayList<>();
    }

    Map<String, List<TreeNode>> mem = new HashMap<>();
    return helper(1, n, mem);
}

public List<TreeNode> helper(int start, int end, Map<String, List<TreeNode>> mem) {
    List<TreeNode> cur = new ArrayList<>();
    if (start > end) {
        // 才发现还有这种操作...=_=...
        cur.add(null);
        return cur;
    }

    String key = String.valueOf(start + "_" + end);
    if (mem.containsKey(key)) {
        return mem.get(key);
    }

    for (int i = start; i <= end; i++) {
        List<TreeNode> leftList = helper(start, i - 1, mem);
        List<TreeNode> rightList = helper(i + 1, end, mem);

         for (TreeNode left : leftList) {
            for (TreeNode right : rightList) {
                TreeNode root = new TreeNode(i);
                root.left = left;
                root.right = right;
                cur.add(root);
            }
        }
     }

    mem.put(key, cur);
    return cur;
}
```

```java
public List<TreeNode> generateTrees(int n) {
    if (n < 1) {
        return new ArrayList<>();
    }

    Map<String, List<TreeNode>> mem = new HashMap<>();
    return helper(1, n, mem);
}

public List<TreeNode> helper(int start, int end, Map<String, List<TreeNode>> mem) {
    // 不合法的输入，所以返回null
    if (start > end) {
        return null;
    }
    // 这是只有一个点的情况
    if (start == end) {
        TreeNode root = new TreeNode(start);
        List<TreeNode> tmp = new ArrayList<>();
        tmp.add(root);
        return tmp;
    }
    //这是memorization快速找到以该“范围”的所有树
    String key = String.valueOf(start + "_" + end);
    if (mem.containsKey(key)) {
        return mem.get(key);
    }

    // 如果没生成过，就逐个生成，每个点都当一次root
    List<TreeNode> cur = new ArrayList<>();
    for (int i = start; i <= end; i++) {
        // 递归得到左边和右边的所有树
        List<TreeNode> leftList = helper(start, i - 1, mem);
        List<TreeNode> rightList = helper(i + 1, end, mem);

        // 这段判空有点恶心...改改...
        if (leftList == null) {
            for (TreeNode right : rightList) {                    
                TreeNode root = new TreeNode(i);
                root.right = right;
                cur.add(root);
            }
        } else if (rightList == null) {
            for (TreeNode left : leftList) {
                TreeNode root = new TreeNode(i);
                root.left = left;                    
                cur.add(root);
            }
        } else {
            for (TreeNode left : leftList) {
                for (TreeNode right : rightList) {
                    TreeNode root = new TreeNode(i);
                    root.left = left;
                    root.right = right;
                    cur.add(root);
                }
            }
        }
    }
    // 最后放到公告板，memorize里
    mem.put(key, cur);
    return cur;
}
```
