# 652 Find Duplicate Subtrees

Given a binary tree, return all duplicate subtrees. For each kind of duplicate subtrees, you only need to return the root node of any**one**of them.

Two trees are duplicate if they have the same structure with same node values.

**Example 1:**

```
        1
       / \
      2   3
     /   / \
    4   2   4
       /
      4
```

The following are two duplicate subtrees:

```
      2
     /
    4
```

and

```
    4
```

Therefore, you need to return above trees' root in the form of a list.

这题呢，一开始就只想到暴力解。利用[100 Same Tree](100-same-tree.md) (L469)来判断是否相同，然后遍历全树。其实呢，也是个n方的解法呀，不知为毛没过大数据。后来看了答案才发现应该用[L245 Subtree](l245-subtree.md) (572)的解法。因为preorder+null可以唯一确定一棵树。所以把这个string作为key，放到hashmap里。也是O(n^2)，因为每次建立这个key都得O(n)，我们要遍历全树。这里处理方法有点想不到，以前一直觉得hm里存的东西要返回。这里结果是另外存的，hashmap只是用来数数的。

```java
// <preOrder, cnt>
Map<String, Integer> hm;
List<TreeNode> res;
public List<TreeNode> findDuplicateSubtrees(TreeNode root) {
    res = new ArrayList<>();
    if (root == null) {
        return res;
    }

    hm = new HashMap<>();
    preOrder(root);
    return res;
}

private String preOrder(TreeNode root) {
    if (root == null) {
        return "#";
    }
    // 制造preorder
    String key = root.val + "," + preOrder(root.left) + "," + preOrder(root.right);
    hm.put(key, hm.getOrDefault(key, 0) + 1); // 数数
    // 重复了，加结果
    if (hm.get(key) == 2) {
        res.add(root);
    }

    return key;
}
```

然而，答案还有一种O(n)的解法。每个key都只compute一次。我们用“val，leftType，rightType” 来表示一种树，如果这个相同的话，就是相同的数了。但是这里我们要多开一个map来存每种树的数目。原因是，我们可以通过第一个treeType的hash来快速找到子树的type，就不用像前面那样算了。只是查表，不用O(n)地找。

```java
int type;
// <treeType, cnt>
Map<Integer, Integer> cnt;
// <"root,leftType,rightType", treeType>
Map<String, Integer> treeTypes;
List<TreeNode> res;
public List<TreeNode> findDuplicateSubtrees(TreeNode root) {
    res = new ArrayList<>();
    if (root == null) {
        return res;
    }
    // type-0 means null
    type = 1;
    cnt = new HashMap<>();
    treeTypes = new HashMap<>();
    getType(root);
    return res;
}

private int getType(TreeNode root) {
    if (root == null) {
        return 0;// Type-0
    }

    String typeKey = root.val + "," + getType(root.left) + "," + getType(root.right);
    int curType = type;
    treeTypes.putIfAbsent(typeKey, type++);
    cnt.put(treeTypes.get(typeKey), cnt.getOrDefault(treeTypes.get(typeKey), 0) + 1);

    if (cnt.get(treeTypes.get(typeKey)) == 2) {
        res.add(root);
    }        

    return treeTypes.get(typeKey);
}

// Java 8里有一种更逆天的写法，写下来作为参考
private int getType(TreeNode root) {
    if (root == null) {
        return 0;// Type-0
    }

    String typeKey = root.val + "," + getType(root.left) + "," + getType(root.right);               
    int uid = treeTypes.computeIfAbsent(typeKey, x-> type++); // 好好了解一下
    cnt.put(uid, cnt.getOrDefault(uid, 0) + 1);

    if (cnt.get(uid) == 2) {
        res.add(root);
    }        

    return uid;
}
```
