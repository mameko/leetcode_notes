# 307 Range Sum Query - Mutable

Given an integer array nums, find the sum of the elements between indices i and j (i ≤ j), inclusive.

The update(i, val) function modifies nums by updating the element at index i to val.

**Example:**

```
Given nums = [1, 3, 5]

sumRange(0, 2) -> 9
update(1, 2)
sumRange(0, 2) -> 8
```

**Constraints:**

* The array is only modifiable by the update function.
* You may assume the number of calls to update and sumRange function is distributed evenly.
* `0 <= i <= j <= nums.length - 1`

嘛，这题因为很多update所以不能再用纯数组类型的prefix sum了。这里可以用BIT或者Segment Tree。

先学了BIT。其实这题是一个很直白的implementation。虽然说是树，但其实存的时候是数组，root是tree\[0]，val也是0，其他的node分别对应一截sum，有长有短。然后parent的关系是这个index - （index & -index），好像就是去掉最低位的1。每次更新的时候，我们也要更新后面所有位置的prefix sum，然后需要更新的位置index + （index & -index）。每次求0到现在index的前序和，就把这个节点一直往上加到root

看了答案，发现其实不需要用numsCopy来存旧的数是多少，因为我们可以通过求sumRange(updateIndex, updateIndex)来求出这个数原来值是多少

* Time Complexity
  * Update (1D) - O(logN)
  * Query (1D) - O(logN)
  * Build (1D) - O(N⋅logN)
  * Here N denotes the number of elements present in the array. Maximum number of steps that we'll have to iterate for update and query method is upper bounded by the number of bits in the binary representation of the size of the array (N).
* Space Complexity
  * O(N)

```java
private int[] binaryIndexTree;
private int[] numsCopy;
public NumArray(int[] nums) { 
    // 这就是我们的树，比输入array多一位，第一格是dummy，空的       
    binaryIndexTree = new int[nums.length + 1];
    // 因为输入是要modify原array，所以存一个copy
    numsCopy = new int[nums.length];
    for (int i = 0; i < nums.length; i++) {
        int cur = nums[i];
        update(i, cur);
        numsCopy[i] = nums[i];
    }
}

public void update(int i, int val) {
    int index = i + 1;
    int diff = val - numsCopy[i];
    // 每次update到数组外就不用update了 
    while (index < binaryIndexTree.length) {
        binaryIndexTree[index] += diff;
        index = getNext(index);
    }

    numsCopy[i] = val;
}

public int sumRange(int i, int j) { // 跟基础解法一样
    return getPrefixSum(j + 1) - getPrefixSum(i);
}

// 这个是算[0, index]的prefix sum
private int getPrefixSum(int index) {
    int sum = 0;
    // 0就是root了，所以找到root为止行了
    while (index > 0) {
        sum += binaryIndexTree[index];
        index = getParent(index);
    }
    return sum;
}

// 找下一个需要update的index
private int getNext(int index) {
    return index + (index & -index);      
}    

// 找parent算prefix sum的时候要用
private int getParent(int index) {
    return index - (index & -index);
}
```
