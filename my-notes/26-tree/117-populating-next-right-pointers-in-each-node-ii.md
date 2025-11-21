# 117 Populating Next Right Pointers in Each Node II

Follow up for problem "Populating Next Right Pointers in Each Node".

What if the given tree could be any binary tree? Would your previous solution still work?

**Note:**

* You may only use constant extra space.

For example,\
Given the following binary tree,

```
         1
       /  \
      2    3
     / \    \
    4   5    7
```

After calling your function, the tree should look like:

```
         1 -> NULL
       /  \
      2 -> 3 -> NULL
     / \    \
    4-> 5 -> 7 -> NULL
```

```java
public void connect(TreeLinkNode root) {
    if (root == null) {
        return;
    }

    TreeLinkNode first = null;// first in next level
    TreeLinkNode pointer = null;// pointer to go right
    TreeLinkNode cur = root;// current level

    while (cur != null) {
        while (cur != null) {
            // if have left
            if (cur.left != null) {
                // and we are not at the 1st 
                if (pointer != null) {
                    pointer.next = cur.left; 
                } else {
                    first = cur.left;
                }

                pointer = cur.left;
            }

            if (cur.right != null) {
                if (pointer != null) {
                    pointer.next = cur.right;
                } else {
                    first = cur.right;
                }

                pointer = cur.right;
            }

            cur = cur.next;
        }

        cur = first;
        first = null;
        pointer = null;
    }
}
```
