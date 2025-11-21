# 21 DP

所有dp都能用记忆化搜索，只是用那个的话难做空间优化。在初始状态难找或状态转移麻烦的时候，我们可以从大到小用memorization。

如果是求可行与否，最大/最小值，方案总数之类的很有可能是DP。

* 动归的时间计算：状态个数 × 每个状态要用的时间
* 感觉这几个dp的格局有点，jump game，LIS，work break，palindrome min cut。两个循环，外层枚举i，内层j从0找到i，边找边更新。
* dp空间优化可以用%来做，如果只需要2行的话，我们可以用i%2来优化空间。

**所谓“划分类” DP，是指给定原数组之后，将其划分为k个子数组，使其sum / product最大/最小的DP类问题。**

**而这类问题的optimal substructure是，对于给定区间的globalMax，其最优解一定由所属区间内的localMax区间组成，可能不连续。以“必须以当前结尾”来保证local子问题之间的连续性；用global来记录最优解之间可能的不连续性。**

**划分类DP与区间类DP主要区别在于，我们只取其中k段，而中间部分可以留空；而区间类DP子问题之间是连贯而不留空的。区间类DP是记忆化的divide & conquer,划分类DP则是不连续subarray的组合，不同于区间类DP每一个区间都要考虑与计算，划分类DP很多元素和子区间是可以忽略的，有贪心法的思想在里面，因此也依赖于正确的local / global结构来保证算法的正确性。**

摘自：[https://mnmunknown.gitbooks.io/algorithm-notes/content/626,\_dong\_tai\_gui\_hua\_ff0c\_subarray\_lei.html](https://mnmunknown.gitbooks.io/algorithm-notes/content/626,_dong_tai_gui_hua_ff0c_subarray_lei.html)

**简单的DP：**

[120 Triangle](120-triangle.md)

[L397 Longest Increasing Continuous Subsequence](l397-longest-increasing-continuous-subsequence.md)

**坐标型：**&#x72B6;态表示dp\[i]/dp\[i]\[j]为**第i个**，从起点走到那个位置

[64 Minimum Path Sum](64-minimum-path-sum.md) (L110)

[70 Climbing Stairs](70-climbing-stairs.md) -- fibonacci (L111)

[L272 Climbing Stairs II](l272-climbing-stairs-ii.md) -- fibonacci

[62 Unique Paths](62-unique-paths.md) (L114)

[63 Unique Paths II](63-unique-paths-ii.md) (L115)

[174 Dungeon Game](174-dungeon-game.md)

[221 Maximal Square](221-maximal-square.md) (L436)

L631 Maximal Square II

[300 Longest Increasing Subsequence](397-longest-increasing-continuous-subsequence.md)(L76)

[354 Russian Doll Envelopers](354-russian-doll-envelopers.md)-- L300变形 (L602)

[368 Largest Divisible Subset](368-largest-divisible-subset.md) -- L300变形 (L603)

[meeting room 3](meeting-room-3.md) -- weighted interval scheduling problem L300变形

[1235 Maximum Profit in Job Scheduling](1235-maximum-profit-in-job-scheduling.md) -- 这题好像354，但其实更像meeting room

[334 Increasing Triplet Subsequence](334-increasing-triplet-subsequence.md) -- 继续变

[L116 Jump Game](l116-jump-game.md)

[L117 Jump Game II](l117-jump-game-ii.md)

[L398 Longest Increasing Continuous Subsequence II](l398-longest-increasing-subsequence-ii.md) -- memorization (329 Longest Increasing Path in a Matrix) <--听说还能topo sort来做

[403 Frog Jump](403-frog-jump.md) (L622) -- hm做dp

[494 Target Sum](494-target-sum.md) -- hm做dp

**单序列型：**&#x72B6;态表示dp\[i]为**前i个**位置/数字/字符，通常要加一，dp\[0]前0个

[139 Word Break](139-word-break.md) (L107)

[L108 Palindrome Partitionaing II](l108-palindrome-partitionaing-ii.md) -- min cut (132)

[91 Decode Ways](91-decode-ways.md) (L152)

[198 House Robber](198-house-robber.md) (L392) -- cc17.16

[213 House Robber II](213-house-robber-ii.md) -- (L534) 破环：1. repeat算区间， 2.断开算两边

[413 arithmetic Slices](413-arithmetic-slices.md)

**双序列型：**&#x64;p\[i]\[j]表示第一个的**前i个**与第二个的**前j个**

[L77 Longest Common Subsequence](l77-longest-common-subsequence.md)

[L79 Longest Common SubString](l79-longest-common-substring.md)

[72 Edit Distance](72-edit-distance.md) -- (L119) [161 One Edit Distance](../other/161-one-edit-distance.md) 相关但解决方法不同。~~edit distance终极版本是snapchat面经edit distance to palindrome~~突然發現Leetcode有了

[1216 Valid Palindrome III](1216-valid-palindrome-iii-1.md) -- [680 Valid Palindrome II](../two-pointers-other/680-valid-palindrome-ii.md)是這題的edit one版本，不過比較像2ptr的解法，雖然我用了递归。

[115 Distinct Subsequences](115-distinct-subsequences.md) (L118)

[97 Interleaving String](97-interleaving-string.md) (L29)

[L581 Longest Repeating Subsequence](l581-longest-repeating-subsequence.md)

[44 Wildcard matching](44-wildcard-matching.md) (L192)

[10 Regular Expression Matching](10-regular-expression-matching.md) (L154)

**博弈类：**&#x9760;人品来画搜索树来解决

[L395 Coins in Line II](l395-coins-in-line-ii.md)

[L394 Coins in Line](l394-coins-in-line.md)

292 Nim Game

**MiniMax :**

375 Guess Number higher or Lower II

464 Can I Win

**区间类：大区间依赖于小区间，按区间来枚举；也可以记忆化搜索不用考虑顺序**

[312 Burst Ballons](312-burst-ballons.md) (168)

[L396 Coins in line III](l395-coins-in-line-iii.md)

[87 Scramble String](87-scramble-string.md) (L430)

[L476 Stone Game](l476-stone-game.md)

[L593 Stone Game II](l593-stone-game-ii.md) -- 破环， 用repeat的方法

\-------- 什么时候适合repeat的方法来做呢？规模为n的问题可以在链上做，变成环以后，求环的各种断开方式里最好的一种

L435 Post Office Problem

**划分类：**

[188 Best Time to Buy and Sell Stock IV (L393)](../stocks/l393-best-time-to-buy-and-sell-stock-iv.md)

[L43 Maximum Subarray III](l43-maximum-subarray-iii.md)

**背包类：**

[BackPacks](../backpacks/)

L89 k Sum

[416 Partition Equal Subset Sum](../backpacks/416-partition-equal-subset-sum.md) -- 加起来除以2，然后求背包容量就是那个avg值

L91 Minimum Adjustment Cost

**貌似做了未分类：**

[L584 Drop Eggs II](l584-drop-eggs-ii.md) -- DP

[152 Maximum Product Subarray](152-maximum-product-subarray.md)(L191)

[96 Unique Binary Search Tree](../26-tree/96-unique-binary-search-tree.md) -- DP, Catalan number (L163)

95 Unique Binary Search Tree II -- 搜索

343 Integer Break

[264 Ugly Number II](264-ugly-number-ii.md) (L4)

[32 Longest Valid Parentheses](../21-data-structure/32-longest-valid-parentheses.md) -- can use stack

[256 Paint House](256-paint-house.md)

[265 Paint House II](265-paint-house-ii.md)

[276 Paint Fence](276-paint-fence.md) (L514)

[361 Bomb Enemy](361-bomb-enemy.md)

[392 Is Subsequence](392-is-subsequence.md)

[516 Longest Palindromic Subsequence](../palindrome/516-longest-palindromic-subsequence.md)

[5 Longest Palindromic Substring](../palindrome/5-longest-palindromic-substring.md)

[494 Target Sum](494-target-sum.md)

[1641 Count Sorted Vowel Strings](1641count-sorted-vowel-strings.md)

[750 Number Of Corner Rectangles](750-number-of-corner-rectangles.md)

**其他类？**

[L437 copy books I](../17-binary-search/l437-copy-books-i.md) - 也可以二分答案，我也只会二分

[L438 copy books II](../17-binary-search/l438-copy-books-ii.md) - 也可以二分答案，我也只会二分

[410 Split Array Largest Sum](./) -- 也可以二分答案，我也只会二分

ip address 那题跟work break有点像，work break又跟min cut palindrome有点像。

L20 Dices Sum

363 Max Sum of Rectangle No Larger Than K

321 Create Maximum Number (L552)

376 Wiggle Subsequence

446 Arithmetic Slices II - Subsequence

418 Sentence Screen Fitting -- memorization (MIT course)

474 Ones and Zeros

466 Count The Repetitions

489 Predict the Winner

467 Unique Substrings in Wraparound String

471 Encode String with Shortest Length

472 Concatenated Words

[517 Super Washing Machines](517-super-washing-machines.md)

338 Counting Bits -- Bit manipulation

[L194 Find Words](../17-binary-search/l194-find-words.md) -- 什么鬼自动机，也可以用二分

**other related:**

[131 Palindrome Partitioning](../28-search/131-palindrome-partitioning.md) (L136) -- search

L90 k Sum II - DFS

[140 Word Break II](../28-search/140-word-break-ii.md) --(L582) search

L623 K Edit Distance

[85 Maximal Rectangle](85-maximal-rectangle.md) (L510) -- [histogram](../dan-diao-zhan/84-largest-rectangle-in-histogram.md)

[L517 Ugly Number](../math/l517-ugly-number.md) (263)

L518 Super Ugly Number (313)
