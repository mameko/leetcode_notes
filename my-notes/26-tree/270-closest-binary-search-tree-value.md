# 270 Closest Binary Search Tree Value

Given a non-empty binary search tree and a target value, find the value in the BST that is closest to the target.

**Note:**

* Given target value is a floating point.
* You are guaranteed to have only one unique value in the BST that is closest to the target.

两年后再写一遍：

```java
public int closestValue(TreeNode root, double target) {
    if (root == null) {
        return Integer.MIN_VALUE;
    }        

    double minDiff = Double.MAX_VALUE;
    int res = Integer.MIN_VALUE;
    while (root != null) {
        int cur = root.val;
        double diff = Math.abs(cur - target);            
        if (diff < minDiff) {
            minDiff = Math.min(minDiff, diff);
            res = cur;
        }
        if (cur > target) {
            root = root.left;
        } else {
            root = root.right;
        }
    }

    return res;
}
```

```java
public int closestValue(TreeNode root, double target) {
    if (root == null) {
        return Integer.MAX_VALUE;
    }

    double minVal = Double.MAX_VALUE;
    int minNodeVal = Integer.MAX_VALUE;

    while (root != null) {
        double diff = root.val - target;
        if (diff > 0) {
            if (Math.abs(diff) < minVal) {
                minVal = Math.abs(diff);
                minNodeVal = root.val;
            }
            root = root.left;
        } else {
            if (Math.abs(diff) < minVal) {
                minVal = Math.abs(diff);
                minNodeVal = root.val;
            }
            root = root.right;
        }
    }

    return minNodeVal;
}
```
