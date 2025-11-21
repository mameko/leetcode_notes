# 1649 Create Sorted Array through Instructions

Given an integer array `instructions`, you are asked to create a sorted array from the elements in `instructions`. You start with an empty container `nums`. For each element from **left to right** in `instructions`, insert it into `nums`. The **cost** of each insertion is the **minimum** of the following:

* The number of elements currently in `nums` that are **strictly less than** `instructions[i]`.
* The number of elements currently in `nums` that are **strictly greater than** `instructions[i]`.

For example, if inserting element `3` into `nums = [1,2,3,5]`, the **cost** of insertion is `min(2, 1)` (elements `1` and `2` are less than `3`, element `5` is greater than `3`) and `nums` will become `[1,2,3,3,5]`.

Return _the **total cost** to insert all elements from_ `instructions` _into_ `nums`. Since the answer may be large, return it **modulo** `109 + 7`

**Example 1:**

```
Input: instructions = [1,5,6,2]
Output: 1
Explanation: Begin with nums = [].
Insert 1 with cost min(0, 0) = 0, now nums = [1].
Insert 5 with cost min(1, 0) = 0, now nums = [1,5].
Insert 6 with cost min(2, 0) = 0, now nums = [1,5,6].
Insert 2 with cost min(1, 2) = 1, now nums = [1,2,5,6].
The total cost is 0 + 0 + 0 + 1 = 1.
```

**Example 2:**

```
Input: instructions = [1,2,3,6,5,4]
Output: 3
Explanation: Begin with nums = [].
Insert 1 with cost min(0, 0) = 0, now nums = [1].
Insert 2 with cost min(1, 0) = 0, now nums = [1,2].
Insert 3 with cost min(2, 0) = 0, now nums = [1,2,3].
Insert 6 with cost min(3, 0) = 0, now nums = [1,2,3,6].
Insert 5 with cost min(3, 1) = 1, now nums = [1,2,3,5,6].
Insert 4 with cost min(3, 2) = 2, now nums = [1,2,3,4,5,6].
The total cost is 0 + 0 + 0 + 0 + 1 + 2 = 3.
```

**Example 3:**

```
Input: instructions = [1,3,3,3,2,4,2,1,2]
Output: 4
Explanation: Begin with nums = [].
Insert 1 with cost min(0, 0) = 0, now nums = [1].
Insert 3 with cost min(1, 0) = 0, now nums = [1,3].
Insert 3 with cost min(1, 0) = 0, now nums = [1,3,3].
Insert 3 with cost min(1, 0) = 0, now nums = [1,3,3,3].
Insert 2 with cost min(1, 3) = 1, now nums = [1,2,3,3,3].
Insert 4 with cost min(5, 0) = 0, now nums = [1,2,3,3,3,4].
​​​​​​​Insert 2 with cost min(1, 4) = 1, now nums = [1,2,2,3,3,3,4].
​​​​​​​Insert 1 with cost min(0, 6) = 0, now nums = [1,1,2,2,3,3,3,4].
​​​​​​​Insert 2 with cost min(2, 4) = 2, now nums = [1,1,2,2,2,3,3,3,4].
The total cost is 0 + 0 + 0 + 0 + 1 + 0 + 1 + 0 + 2 = 4.
```

**Constraints:**

* `1 <= instructions.length <= 105`
* `1 <= instructions[i] <= 105`

一开始看题，就只想到brute force，每次通过二分找插入位置来算cost。但是九章模板老是off by one。所以brute force做不出来。然后看了答案，说是用BIT或者segmet Tree做。研究了半天还是有点迷。

解法一：BIT，有点像bucket sort。把instruction的值作为BIT下标，出现次数作为BIT的val存起来。所以建立BIT的时候要把instruction的最大range + 1放进去，加了2可能是因为怕还不够吧。然后prefixSum这个下标-1，就是这个数字前面有多啊少个小于TA的，然后把i - prefixSum就知道后面有多少个大于TA的数字（这里i是现在插了多少个数，prefixSum（val）是包括val在内，前面有多少个数。所以相减得到后面有多少个数）。T：O(NlogM)，N是instruction的长度，M是instruction里值的range。S：O(M)，存了BIT

```java
private int[] BIT;
private int n;

public int createSortedArray(int[] instructions) {
    if (instructions == null || instructions.length == 0) {
        return 0;
    }

    long MOD = 1000000007;
    long cost = 0;
    n = 100002; // max of instruction size + 1
    BIT = new int[n];

    for (int i = 0; i < instructions.length; i++) {
        int curVal = instructions[i];
        int leftCost = query(curVal - 1);
        int rightCost = i - query(curVal);
        cost += Math.min(leftCost, rightCost);
        update(curVal, 1);// insert this number's count in the BIT
    }

    return (int)(cost % MOD);        
}

private int query(int index) {
    index = index + 1;
    int sum = 0;
    while (index > 0) {
        sum += BIT[index];
        index -= lsb(index);
    }
    return sum;
}

private void update(int index, int val) {
    index = index + 1;
    while (index < n + 1) {
        BIT[index] += val;
        index += lsb(index);
    }
}

private int lsb(int index) {
    return index & -index;
}
```
