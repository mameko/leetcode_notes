# 7 Subarrays (prefix sum)

这类题用到prefix sum。感觉sliding window求size，subarrays求sum/具体下标。

prefix的array/matrix留空前面一个格子/一行一列来算比较方便，用的时候把j +1减去 i 就好了。

6年后发现，还有一类是用hashmap来算prefix/suffix的

**基础题：**

[303 Range Sum Query - immutable](303-range-sum-query.md) -- prefixSum

[304 Range Sum Query 2D - immutable](304-range-sum-query-2d-immutable.md) -- prefix Sum

[L558 Sliding Window Matrix Maximum](l558-sliding-window-matrix-maximum.md)

[1310 XOR Queries of a Subarray](../31-bit-manipulation/1310-xor-queries-of-a-subarray.md) -- 套着xor皮的prefix sum

[528 Random Pick with Weight](528-random-pick-with-weight.md)&#x20;

**Max/Min :**

[L41 Maximum Subarray](l41-maximum-subarray.md)(53) -- L402求下标

[L42 Maximum Subarray II](l42-maximum-subarray-ii.md)

[L44 Minimum Subarray](l44-minimum-subarray.md)

[L45 Maximum Subarray Difference](l45-maximum-subarray-difference.md)

L620 Maximum Subarray IV

L621 Maximum Subarray V

L617 Maximum Average Subarray (2分，留意2分讲座笔记）

523 Continuous Subarray Sum -- 感觉有点2分的意味，有空看看

[Stock1](../stocks/l149-best-time-to-buy-and-sell-stock.md) [Stock 3](../stocks/l151-best-time-to-buy-and-sell-stock-iii.md) 都有点prefix Sum

**首尾链接：**

[L403 Continuous Subarray Sum II](l403-continuous-subarray-sum-ii.md) -- 有环

[L402 Continuous Subarray Sum](l402-continuous-subarray-sum.md) -- L41求max值，这里求具体下标。

**DP：**

[L43 Maximum Subarray III](../22-dp/l43-maximum-subarray-iii.md) (DP -- 划分类)

[L191 Maximum product Subarrays](../22-dp/152-maximum-product-subarray.md) (DP) （152）

[Stock 3](../stocks/l151-best-time-to-buy-and-sell-stock-iii.md)（DP -- 划分类）

**hash or 2分：-- 有点像在用hashmap做dp的感觉**

[L138 Subarray Sum](l138-subarray-sum.md) -- hash

[L405 Submatrix Sum](l405-submatrix-sum.md) --L138进化版

[L404 Subarray Sum II](l404-subarray-sum-ii.md) -- 用2分优化

[325 Maximum Size Subarray Sum Equals k](325-maximum-size-subarray-sum-equals-k.md) -- 感觉又是一个L138的变种

[L139 Subarray Sum Closest](l139-subarray-sum-closest.md)

[523 Continuous Subarray Sum](523-continuous-subarray-sum.md) -- L138又变，这次来个mod

[525 Contiguous Array](../others/525-contiguous-array.md) -- 又一个L138

[1477 Find Two Non-overlapping Sub-arrays Each With Target Sum](1477-find-two-non-overlapping-sub-arrays-each-with-target-sum.md)

[560 Subarray Sum Equals K](560-subarray-sum-equals-k.md)

变体or相关：

[437 Path Sum III](../26-tree/437-path-sum-iii.md)

**other related：**

[713 Subarray Product Less Than K](../chapter1/713-subarray-product-less-than-k.md)

[724 Find Pivot Index](724-find-pivot-inde.md) -- 某次contest的题

[308 Range Sum Query 2D - Mutable](../27-segment-tree/308-range-sum-query-2d-mutable.md) -- segment tree or BIT

[307 Range Sum Query - Mutable](../27-segment-tree/307-range-sum-query-mutable.md) -- segment tree or BIT

[L643 Longest Absolute File Path (388)](../21-data-structure/l643-longest-absolute-file-path-388.md) -- 实现上可以用prefix sum数组或者stack
