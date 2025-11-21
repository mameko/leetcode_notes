# 26 Tree

**二叉树**

**LinkedList to Tree / List / Array to Tree：**

[L106 Convert Sorted List to Balanced BST](../18-linkedlist/l106-convert-sorted-list-to-balanced-bst.md) (109)

[L378 Convert Binary Search Tree to Doubly Linked List](../18-linkedlist/l378-convert-binary-search-tree-to-doubly-linked-list.md)

[L453 Flatten Binary Tree to Linked List](../18-linkedlist/l453-flatten-binary-tree-to-linked-list.md) (114) - 修改树

[L242 Convert Binary Tree to Linked Lists by Depth](l242-convert-binary-tree-to-linked-lists-by-depth.md) -- bfs & dfs

[108 Convert Sorted Array to Binary Search Tree](108-convert-sorted-array-to-binary-search-tree.md)

**Path Sum类：**

[129 Sum Root to Leaf Numbers](129-sum-root-to-leaf-numbers.md)

[L475 Binary Tree Maximum Path Sum II](l475-binary-tree-maximum-path-sum-ii.md) -- max from root to leaf

[L94 Binary Tree Maximum Path Sum](l94-binary-tree-maximum-path-sum.md) (124) -- max from any to any

\---------感觉这几条path sum的基调都是target - node.val再递归然后判断 = node.val比加起来方便------

[112 Path Sum](112-path-sum.md)

[113 Path Sum II](113-path-sum-ii.md) (L376 Binary Tree Path Sum)

[437 Path Sum III](437-path-sum-iii.md) -- 跟L246一样，只是这题求的是方案个数，L246求具体方案

[L246 Binary Tree Path Sum II](l246-binary-tree-path-sum-ii.md)

[L472 Binary Tree Path Sum III](l472-binary-tree-path-sum-iii.md) -- 有parent pointer，其实是图的遍历找target

\------------------下面这几题是连续递增/递减序列------------------

[298 Binary Tree Longest Consecutive Sequence](298-binary-tree-longest-consecutive-sequence.md)（L595）-- along parent to child path Inc from up to down

[L614 Binary Tree Longest Consecutive Sequence II](l614-binary-tree-longest-consecutive-sequence-ii.md) (549) -- any to any

[543 Diameter of Binary Tree](543-diameter-of-binary-tree.md)

**Tree Node Sum/Value类：**

[L597 Subtree with Maximum Average](l597-subtree-with-maximum-average.md)

[L596 minimum Subtree](l596-minimum-subtree.md)

[L628 Maximum Subtree](l628-maximum-subtree.md)

[501 Find Mode in Binary Search Tree](501-find-mode-in-binary-search-tree.md)

[333 Largest BST Subtree](333-largest-bst-subtree.md)

[508 Most Frequent Subtree Sum](508-most-frequent-subtree-sum.md)

[404 Sum of Left Leaves](404-sum-of-left-leaves.md)

[563 Binary Tree Tilt](563-binary-tree-tilt.md)

[1022 Sum of Root To Leaf Binary Numbers](1022-sum-of-root-to-leaf-binary-numbers.md)

[617 Merge Two Binary Trees](617-merge-two-binary-trees.md)

**Lowest Common Ancestor类：**

[235 Lowest Common Ancestor of a BST](235-lowest-common-ancestor-of-a-bst.md)

[236 Lowest Common Ancestor of a Binary Tree](236-lowest-common-ancestor-of-a-binary-tree.md) (L88)

[L474 Lowest Common Ancestor II](l474-lowest-common-ancestor-ii.md) -- 有parent pointer，[链表intersection](../18-linkedlist/160-intersection-of-two-linked-lists.md)很像。

[L578 Lowest Common Ancestor III](l578-lowest-common-ancestor-iii.md) - 要找的点可能不在树里

[1257 Smallest Common Region](1257-smallest-common-region.md) -- 像有parent的那题，结构有点不一样

**判断树：**

[L467 Complete Binary Tree](l467-complete-binary-tree.md) - 要补code

[100 Same Tree](100-same-tree.md) (L469)

[L470 Tweaked identical Binary Tree](l470-tweaked-identical-binary-tree.md)

[101 Symmetric Tree](101-symmetric-tree.md) (L468)

[98 Valid Binary Search Tree](98-valid-binary-search-tree.md) (L95)

[110 Balanced Binary Tree](110-balanced-binary-tree.md) (L93)

[255 Verify Preorder Sequence in BST](255-verify-preorder-sequence-in-bst.md) -- 单调栈

[L245 Subtree](l245-subtree.md) (572) -- preorder + "#" 可以uniquely identify a tree

[652 Find Duplicate Subtrees](652-find-duplicate-subtrees.md)

**修改树：**

[226 Invert Binary Tree](226-invert-binary-tree.md) (L175)

[116 Populating Next Right Pointers in Each Node](116-populating-next-right-pointers-in-each-node.md)

[117 Populating Next Right Pointers in Each Node II](117-populating-next-right-pointers-in-each-node-ii.md)

[156 Binary Tree Upside Down](156-binary-tree-upside-down.md) 感觉是在修改特殊的链表

[L375 Clone Binary Tree](l375-clone-binary-tree.md)

[538 Convert BST to Greater Tree](538-convert-bst-to-greater-tree.md) (L661)

[814 Binary Tree Pruning](814-binary-tree-pruning.md)

[669 Trim a Binary Search Tree](669-trim-a-binary-search-tree.md)

**基本变形考：**

[模板](../../templates/shu-de-mo-ban.md)

[173 Binary Search Tree Iterator](173-binary-search-tree-iterator.md) (L86) - inorder

[222 Count Complete Tree Nodes](222-count-complete-tree-nodes.md) - 满二叉树变种考法

[230 Kth Smallest Element in a BST ](230-kth-smallest-element-in-a-bst.md)- 中序遍历or分治（分治做法比较像median那题）

[671 Second Minimum Node In a Binary Tree](671-second-minimum-node-in-a-binary-tree.md)

[938 Range Sum of BST](938-range-sum-of-bst.md)

**Traversal Related：**

[99 Recover Binary Search Tree](99-recover-binary-search-tree.md)

[270 Closest Binary Search Tree Value](270-closest-binary-search-tree-value.md) - pramp 3 - TreeSet flooring / celing / lower / higher

[272 Closest Binary Search Tree Value II](272-closest-binary-search-tree-value-ii.md) -- 可以转化为 [L460 K closest in sorted Arrays](../17-binary-search/l460-k-closest-numbers-in-sorted-array.md)

[257 Binary Tree Paths](257-binary-tree-paths.md) (L480)

[366 Find Leaves of Binary Tree](366-find-leaves-of-binary-tree.md) (L650)

[L11 Search Range in Binary Search Tree](l11-search-range-in-binary-search-tree.md)

[285 Inorder Successor in BST](285-inorder-successor-in-bst.md)

[510 Inorder Successor in BST II](510-inorder-successor-in-bst-ii.md)

[545 Boundary of Binary Tree](545-boundary-of-binary-tree.md)

[1379 Find a Corresponding Node of a Binary Tree in a Clone of That Tree](1379-find-a-corresponding-node-of-a-binary-tree-in-a-clone-of-that-tree.md)

**BFS:**

[102 Binary Tree Level Order Traversal](102-binary-tree-level-order-traversal.md) (L69) -- [对角线打印矩阵](./)

[107 Binary Tree Level Order Traversal II](107-binary-tree-level-order-traversal-ii.md) (L70)

[103 Binary Tree Zigzag Level Order Traversal](103-binary-tree-zigzag-level-order-traversal.md) (L71) -- [L185 Matrix Zigzag Traversal](./)

[513 Find Bottom Left Tree Value](513-find-bottom-left-tree-value.md) -- bfs

[515 Find Largest Value in Each Tree Row](515-find-largest-value-in-each-tree-row.md) -- bfs

[199 Binary Tree Right Side View](119-binary-tree-right-side-view.md)

[582 Kill Process](582-kill-process.md)

[637 Average of Levels in Binary Tree](../graph/637-average-of-levels-in-binary-tree.md)

[690 Employee Importance](690-employee-importance.md)

[1245 Tree Diameter](1245-tree-diameter.md)

[623 Add One Row to Tree](623-add-one-row-to-tree.md)

**建树：**

[105 Construct Binary Tree from Preorder and Inorder Traversal](105-construct-binary-tree-from-preorder-and-inorder-traversal.md) (L73)

[106 Construct Binary Tree form Inorder and Postorder Traversal](106-construct-binary-tree-form-inorder-and-postorder-traversal.md) (L72)

[536 Construct Binary Tree from String](536-construct-binary-tree-from-string.md)

**其他：**

[104 Maximum Depth of Binary Tree](104-maximum-depth-of-binary-tree.md) (L97)

[L155 Minimum Depth of Binary Tree](l155-minimum-depth-of-binary-tree.md) (111)

[250 Count Univalue Subtrees](250-count-univalue-subtrees.md)

[337 House Robber III](337-house-robber-iii.md) (L534)

[96 Unique Binary Search Tree](96-unique-binary-search-tree.md) -- catalan number DP

[95 Unique Binary Search Trees II](95-unique-binary-search-trees-ii.md) -- 搜索

**Serilize or Deserialize Tree:**

[297 Serialize and Deserialize Binary Tree](297-serialize-and-deserialize-binary-tree.md) (L7)

[449 Serialize and Deserialize BST](449-serialize-and-deserialize-bst.md) - deserialize的非递归版本要用单调栈来把复杂度降低到O(n)

[Serilization and Deserialize n-ary Tree](serilization-and-deserialize-n-ary-tree.md)

[L527 Trie Serialization](../25-trie/l527-trie-serialization.md)

**n叉树：**

[Trie](../25-trie/)

[L619 Binary Tree Longest Consecutive Sequence III](l619-binary-tree-longest-consecutive-sequence-iii.md) -- [L614](l614-binary-tree-longest-consecutive-sequence-ii.md)的n叉树follow up

**other Related:**

[314 Binary Tree Vertical Order Traversal](../others/314-binary-tree-vertical-order-traversal.md) -- HashTable

[987 Vertical Order Traversal of a Binary Tree](../others/987-vertical-order-traversal-of-a-binary-tree.md) -- same as 314 排序方法不一样而已

[L392 House Robber I](../22-dp/198-house-robber.md) - DP

[L535 House Robber II](../22-dp/213-house-robber-ii.md) - DP

[L126 Max Tree](l126-max-tree.md) - cartesian tree 单调栈

[线段树（Segment Tree）](../27-segment-tree/)
