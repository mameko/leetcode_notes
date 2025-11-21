# 414 Third Maximum Number

Given a **non-empty** array of integers, return the **third** maximum number in this array. If it does not exist, return the maximum number. The time complexity must be in O(n).

**Example 1:**

```
Input: [3, 2, 1]
Output: 1

Explanation: The third maximum is 1.
```

**Example 2:**

```
Input: [1, 2]
Output: 2

Explanation: The third maximum does not exist, so the maximum (2) is returned instead.
```

**Example 3:**

```
Input: [2, 2, 3, 1]
Output: 1

Explanation: Note that the third maximum here means the third maximum distinct number.
Both numbers with value 2 are both considered as second maximum.
```

这题一看要O(n)而且还是无序的，我就想到是不是得qucik select。因为排序得nlogn，用数据结构priority queue呀，treeset什么的还是得nlogn。不过这是easy的题，最后想了半天没想出来然后看答案了。原来是用3个变量来自己维护一个类似heap的东西。最后要注意更新heap的时候，重复元素得跳过，不然的话，就会把max2或max3的值给冲掉。

```java
    public int thirdMax(int[] nums) {
        if (nums == null || nums.length == 0)    {
            return 0;
        }

        Integer max1 = null;
        Integer max2 = null;
        Integer max3 = null;

        for (Integer n : nums) {
            // skip dupplicate
            if (n.equals(max1) || n.equals(max2) || n.equals(max3)) {
                continue;
            }

            if (max1 == null || n > max1) {
                max3 = max2;
                max2 = max1;
                max1 = n;
            } else if (max2 == null || n > max2) {
                max3 = max2;
                max2 = n;
            } else if (max3 == null || n > max3) {
                max3 = n;
            }
        }

        return max3 == null ? max1 : max3; //题目要求
    }
```
