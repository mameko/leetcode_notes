# 315 Count of Smaller Numbers After Self

You are given an integer array nums and you have to return a new counts array. The counts array has the property where `counts[i]` is the number of smaller elements to the right of `nums[i]`.

**Example 1:**

```
Input: nums = [5,2,6,1]
Output: [2,1,1,0]
Explanation:
To the right of 5 there are 2 smaller elements (2 and 1).
To the right of 2 there is only 1 smaller element (1).
To the right of 6 there is 1 smaller element (1).
To the right of 1 there is 0 smaller element.
```

**Constraints:**

* `0 <= nums.length <= 10^5`
* `-10^4 <= nums[i] <= 10^4`

嘛，看了大神的解释还是有点懵逼，跟[493 Reverse Pairs](493-reverse-pairs.md)一样。BIT和mergesort都ok。

解法一：BIT。解法来自discussion里这位大神的[这篇](https://leetcode.com/problems/count-of-smaller-numbers-after-self/discuss/384514/Detailed-Analysis-of-Merge-Sort-Solution-and-Binary-Index-Tree-Solution-for-similar-problems)解析。其实用这种解法有两个注意的地方。

首先，我们怎么判断是从屁股开始scan好，还是从头开始好呢？这里，因为我们扫描nums\[i]的时候，要找的是在这个num的右边有多少个XX（这里是比num小的数）。这种情况下，从屁股开始scan比较好，因为你scan的时候都知道右边有啥了。但记得最后要reverse一下答案。或者result.add的时候要add到头那里。

第二点需要注意的是，二分的写法。原来这种模板叫做\[left, right) close left open right，是c++ lower\_bound的实现方法。（[知乎二分文章](https://www.zhihu.com/question/36132386)）这里找的是>= val的第一个数，所以在15行的地方，我们要减一。就是左边所有数都比这个小。

树里面还是存这个数字的频率，树的node还是排序数组里的下标。因为我们从右边开始扫描，所以树里存了的是已经看到过的num。update的时候，从\[i, end]都需要在update里++。譬如，你要插入排序数组里第三位的num。那么排在后面的第四位node，第五位node也得++。比第三位数小的多了一个, 比第四位，第五位小的，也多了一个。查找的时候，我们减一再找，因为二分返回的是>=这个数的位置。减一就是小于这个数的位置。然后一直找到root，就能找到所有比这个数小的数字数目了。（BIT的节点存了频率）

啊，这里因为数字的reange只有\[-10000, 10000]所以，可以用[1649 Create Sorted Array through Instructions](1649-create-sorted-array-through-instructions.md)的方法，还是用BIT存频率，但树的每个下标都对应这个range里的一个值。

过了几年之后又去上九章，九章那里提出了一个比较有趣的做法，比这些高级数据结构更容易。然后复杂度是sqrt(n)的。就是，譬如，我们数字range是10000个，我们开个100的数组来数，每一段有多少个数字，\[0,99],\[100,199]...譬如，我们要插入1，189，那么数组前连个格就会变成\[1, 1,...]。每次算前面有多少个小于的时候，我们先把前面的block的数字加起来。然后再数自己这个block有多少数字。当然，每个数字属于的那一段，我们要再开一个数据结构来存，因为我们快速算完前面的block，自己这个block还是得loop的。但是，范围缩小了，每次查询只用2 \* sqrt(n)

```java
public List<Integer> countSmaller(int[] nums) {
    List<Integer> result = new ArrayList<>();
    if (nums == null || nums.length == 0) {
        return result;
    }

    int n = nums.length;
    int[] BIT = new int[n + 1];
    int[] copy = Arrays.copyOf(nums, n);
    Arrays.sort(copy);

    for (int i = n - 1; i >= 0; i--) {
        int cur = nums[i];
        int loc = findLoc(copy, cur);
        result.add(query(BIT, loc - 1));
        update(BIT, loc);
    }
    Collections.reverse(result);
    return result;
}

private void update(int[] BIT, int index) {
    while (index < BIT.length) {
        BIT[index]++;
        index += index & -index;
    }
}

private int query(int[] BIT, int index) {
    int sum = 0;
    while (index > 0) {
        sum += BIT[index];
        index -= index & -index;
    }
    return sum;
}

private int findLoc(int[] arr, int val) {
    int left = 0;
    int right = arr.length;

    while (left < right) {
        int mid = left + (right - left) / 2;
        if (arr[mid] == val) {
            right = mid;
        } else if (arr[mid] > val) {
            right = mid;
        } else {
            left = mid + 1;
        }
    }

    return left + 1;
}
```
