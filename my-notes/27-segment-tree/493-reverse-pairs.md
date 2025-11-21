# 493 Reverse Pairs

Given an array `nums`, we call `(i, j)` an **important reverse pair** if `i < j` and `nums[i] > 2*nums[j]`.

You need to return the number of important reverse pairs in the given array.

**Example1:**

```
Input: [1,3,2,3,1]
Output: 2
```

**Example2:**

```
Input: [2,4,3,5,1]
Output: 3
```

**Note:**

1. The length of the given array will not exceed `50,000`.
2. All the numbers in the input array are in the range of 32-bit integer.

嘛，其实这题，N方的算法不难，但OJ要nlogn才让过，所以是一道hard的题。

解法一：brute force，T:O(n2), S:O(n)

```java
public int reversePairs(int[] nums) {
    if (nums == null || nums.length == 0) {
        return 0;
    }

    int count = 0;
    for (int i = 0; i < nums.length; i++) {
        for (int j = 0; j < i; j++) {
            // 变long，不然大数据会越界
            if (nums[j] > nums[i] * 2l) {
                count++;
            }
        }
    }

    return count;
}
```

解法二：BST，最坏情况还是n2。做法是，每个BST表示数组里一个数字。然后再加一个field表示大于等于自己的数有多少个。把数组loop一遍，每次，我们去BST里找找大于等于2\*nums\[i] + 1的有多少个，然后把数字加到total里。找完，我们把nums\[i]插入，插入以后还得把前面从root到这个新加的Node的每个节点+1。譬如下面，你插了5进去，那么3的count\_ge也得加1。这个lib自带的BST没这个功能，所以要手动实现。

![](<../../.gitbook/assets/image (14).png>)

```java
public int reversePairs(int[] nums) {
    if (nums == null || nums.length == 0) {
        return 0;
    }

    Node root = null;
    int n = nums.length;
    int count = 0;
    for (int i = 0; i < n; i++) {
        count += search(root, nums[i] * 2l + 1);
        root = insert(root, nums[i]);
    }

    return count;
}

private int search(Node root, long target) {
    if (root == null) {
        return 0;
    }

    if (target == root.val) {
        return root.countGE;
    }

    if (target < root.val) {
        return root.countGE + search(root.left, target);
    } else {
        return search(root.right, target);
    }       
}

private Node insert(Node root, int val) {
    if (root == null) {
        return new Node(val);
    }

    if (val == root.val) {
        root.countGE++;
    } else if (val < root.val) {
        root.left = insert(root.left, val);
    } else {
        root.countGE++;
        root.right = insert(root.right, val);            
    }        

    return root;
}

/*
class Node {
    Node left;
    Node right;
    int val;
    int countGE;
    
    public Node(int val) {
        this.val = val;
        left = null;
        right = null;
        countGE = 1;            
    }
}
*/
```

解法三：为了解决手动实现BST这么高难度的问题，所以用了BIT。BIT的node下标是这个数字在nums排序数组里的位置。存的值是这个数字出现的次数。所以每次update我们val+1。说实在，自己是想不出来这样用BIT。Solution说，普通的BIT，我们query\[0, index]， update从\[index, end]。但这里我们找的是i后面的数字，每次我们把num\[i]插入以后，我们要更新前面的数目（就是更新root）然后每次查询，我们找的是num\[i]后面的数，所以把BIT两个函数reverse一下实现了这个功能。

这里贴一个大牛的分析[https://leetcode.com/problems/reverse-pairs/discuss/97268/general-principles-behind-problems-similar-to-reverse-pairs](https://leetcode.com/problems/reverse-pairs/discuss/97268/general-principles-behind-problems-similar-to-reverse-pairs)

找的时候，我们每次找2\*num\[i] + 1在BIT里的位置。一直找到root，就知道大于的数有多少个。这里二分找的是这个数字在排序数组种第一次出现的位置（ the index of the first element in the `copy` array that is no less than itself）。譬如，\[1, 1, 2, 3, 3]，每次update，我们看到1，更新的是（1， 1）。因为“1”第一次出现在排序数组小标0处。所以在BIT，下标+1，所以是1，值为1，因为出现了一次。再譬如，3，下标是3，BIT里下表是4，值是1。

这里的BIT实现其实跟[307 Range Sum Query - Mutable](307-range-sum-query-mutable.md)的不一样。每次更新，更新的是比现在数字cur小的位置（parent）。

其实如果，这一题从屁股开始loop，我们就可以用正常方向的BIT了。但二分找位置的时候，我们要找的是num\[i] / 2 - 了。[https://codeleading.com/article/61231602920/](https://codeleading.com/article/61231602920/)

这里还用不了九章二分，用了的话，update和search两个只能活一个。譬如，\[-5, -5]，我们便查找边插入，就会发现2\*(-5) + 1 = -9，不在数组里。感觉九章的二分只能用在数组range内。每次超出数组范围的，都很难做。

```javascript
int[] BIT;
int n;
public int reversePairs(int[] nums) {
    if (nums == null || nums.length == 0) {
        return 0;
    }

    n = nums.length;
    BIT = new int[n + 1];

    int[] copy = Arrays.copyOf(nums, n);
    Arrays.sort(copy);

    int count = 0;
    for (int i = 0; i < n; i++) {
        int cur = nums[i];
        count += search(index(copy, 2l * cur + 1));
        update(index(copy, cur));
    }

    return count;
}

private void update(int i) {
    while (i > 0) {
        BIT[i] += 1;
        i = i - (i & -i);
    }
}

private int search(int i) {
    int sum = 0;
    while (i < n + 1) {
        sum += BIT[i];
        i = i + (i & -i);
    }
    return sum;
}

// 大神的二分，注意，这里return时候＋1是因为返回的是BIT的下标
private int index(int[] arr, long val) {
    int l = 0, r = arr.length - 1, m = 0;

    while (l <= r) {
        m = l + ((r - l) >> 1);

        if (arr[m] >= val) {
            r = m - 1;
        } else {
            l = m + 1;
        }
    }

    return l + 1;
}
```

解法四：merge sort其实也行
