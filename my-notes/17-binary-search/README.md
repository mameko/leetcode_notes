# 17 Binary Search

**2分模板：**

[L458 Last Position of Target](l458-last-position-of-target.md)

[L14 First Position of Target](l14-first-position-of-target.md)

[L462 Total Occurrence of Target](l462-total-occurrence-of-target.md)

**普通2分：**

[74 Search in a 2D Matrix](74-search-in-a-2d-matrix.md) (L28)

[240 Search in a 2D matrix II](240-search-in-a-2d-matrix-ii.md) (L38)

[162 Find Peak Element](162-find-peak-element.md) (L75)

[L390 Find Peak Element II](l390-find-peak-element-ii.md)

[L585 maximum Number in Mountain Sequence](l585-maximum-number-in-mountain-sequence.md)

[278 First Bad Version (L74)](l74-first-bad-version.md)

[35 Search Insert Position](35-search-insert-position.md) (L60)

[L447 Search in a Big Sorted Array](l447-search-in-a-big-sorted-array.md) (702)

[L459 Closest Number in Sorted Array](l459-closest-number-in-sorted-array.md)

[L460 K closest Numbers in Sorted Array](l460-k-closest-numbers-in-sorted-array.md)

[744 Find Smallest Letter Greater Than Target](744-find-smallest-letter-greater-than-target.md)

[1064 Fixed Point](1064-fixed-point.md)

[1228 Missing Number In Arithmetic Progression](../math/1228-missing-number-in-arithmetic-progression.md)

[528 Random Pick with Weight](../max-subarrays/528-random-pick-with-weight.md)&#x20;

[L194 Find Words](l194-find-words.md)

**复杂一点的2分搜索：**

[Rotate Arrays](../rotated-arrays/)

[540 Single Element in a Sorted Array](540-single-element-in-a-sorted-array.md)

[34 Search for a Range](24-search-for-a-range.md) (L61)

[302 Smallest Rectangle Enclosing Black Pixels](302-smallest-rectangle-enclosing-black-pixels.md) - 还可以用bfs，不过2分比较好 (L600)

[275 H-Index II](275-h-index-ii.md) -- 九章模板套不了，二分终极版

[L824 Single Number IV](../31-bit-manipulation/l824-single-number-iv.md)

[1539 Kth Missing Positive Number](1539-kth-missing-positive-number.md) -- 九章模板套不了，好像那些答案不在中间的都挺难套九章二分的

[1712 Ways to Split Array Into Three Subarrays](1712-ways-to-split-array-into-three-subarrays.md)

**二分答案：**&#x8FD9;种题的 T：O(nlog(answer range))，只能用于答案range是连续而且有一个分界点。写法：一个用来筛选答案的2分函数，在主函数里调用这个函数2分地找。

[287 Find the Duplicate number](287-find-the-duplicate-number.md) -- 竟然还能用[142 Linked List Cycle II](../18-linkedlist/142-linked-list-cycle-ii.md)的解法

[367 Valid Perfect Square](367-valid-perfect-square.md)

[374 Guess Number Higher or Lower](374-guess-number-higher-or-lower.md)

[L437 copy books I](l437-copy-books-i.md) - can also use DP

[L438 copy books II](l438-copy-books-ii.md) - can also use DP

[410 Split Array Largest Sum](410-split-array-largest-sum.md) - can also use DP

pramp mock test 2，grant cut

[L141 Sqrt(x)](l141-sqrt-x.md) (69)

[RootOfNumber](rootofnumber.md) -- pramp，进化版的[L586 Sqrt(x) II](../math/l586-sqrt-x-ii.md)

[L183 Wood Cut](l183-wood-cut.md)

[L254 Drop Egg](l254-drop-egg.md) - ctci 6.8

L617 maximum Average Subarray

[881 Boats to Save People](881-boats-to-save-people.md)

[1631 Path With Minimum Effort](../graph/1631-path-with-minimum-effort.md)

[441 Arranging Coins](441-arranging-coins.md)

[875 Koko Eating Bananas](875-koko-eating-bananas.md)

[878 Nth Magical Number](878-nth-magical-number.md)

**与树相关的2分：**

[222 Count Complete Tree Nodes](../26-tree/222-count-complete-tree-nodes.md)

[230 Kth Smallest Element in a BST](../26-tree/230-kth-smallest-element-in-a-bst.md)

[270 Closest Binary Search Tree Value](../26-tree/270-closest-binary-search-tree-value.md) - pramp 3 - TreeSet flooring / celing / lower / higher

**Other related:**

375 Guess Number Higher or Lower II - DP or MiniMax

[L586 Sqrt(x) II](../math/l586-sqrt-x-ii.md) --- 牛顿法

L584 Drop Eggs II -- DP

**感觉比较难的二分？：**

[4 Median of Two Sorted Arrays](../other/4-median-of-two-sorted-arrays.md) - 与[230](../26-tree/230-kth-smallest-element-in-a-bst.md)有点像
