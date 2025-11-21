# 370 Range Addition

Assume you have an array of length**n**initialized with all**0**'s and are given**k**update operations.

Each operation is represented as a triplet:**\[startIndex, endIndex, inc]**&#x77;hich increments each element of subarray**A\[startIndex ... endIndex]**(startIndex and endIndex inclusive) with**inc**.

Return the modified array after all**k**operations were executed.

**Example:**

```
Given:

    length = 5,
    updates = [
        [1,  3,  2],
        [2,  4,  3],
        [0,  2, -2]
    ]

Output:

    [-2, 0, 3, 5, 3]
```

**Explanation:**

```
Initial state:
[ 0, 0, 0, 0, 0 ]

After applying operation [1, 3, 2]:
[ 0, 2, 2, 2, 0 ]

After applying operation [2, 4, 3]:
[ 0, 2, 5, 5, 3 ]

After applying operation [0, 2, -2]:
[-2, 0, 3, 5, 3 ]
```

**Hint:**

1. Thinking of using advanced data structures? You are thinking it too complicated.
2. For each update operation, do you really need to update all elements between i and j?
3. Update only the first and end element is sufficient.
4. The optimal time complexity is O(**k** + **n**) and uses O(1) extra space.

这题基本做法是，把需要改变的range开始的那个数加上要改变的数字，然后把range结束后一个数减去要改变的数字。最后用一个loop来算prefix sum。这样就能得到最终答案。我多开了一个数组tmp所以空间复杂度不是O(1) 。

```java
public int[] getModifiedArray(int length, int[][] updates) {
    if (updates == null || updates.length == 0 || updates[0].length == 0 || length < 1) {
        return new int[1];
    }

    // update [start] and [end + 1] each time.
    int[] tmp = new int[length + 1];
    // get each interval
    for (int i = 0; i < updates.length; i++) {
        // incrase to [start]
        tmp[updates[i][0]] += updates[i][2];
        // decrease [end + 1]
        tmp[updates[i][1] + 1] -= updates[i][2];
    }

    // use a loop to collect answers
    int[] res = new int[length];
    res[0] = tmp[0];
    for (int i = 1; i < length; i++) {
        res[i] = res[i - 1] + tmp[i];
    }

    return res;
}
```
