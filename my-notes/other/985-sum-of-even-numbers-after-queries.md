# 985 Sum of Even Numbers After Queries

We have an array `A` of integers, and an array `queries` of queries.

For the `i`-th query `val = queries[i][0], index = queries[i][1]`, we add val to `A[index]`.  Then, the answer to the `i`-th query is the sum of the even values of `A`.

_(Here, the given `index = queries[i][1]` is a 0-based index, and each query permanently modifies the array `A`.)_

Return the answer to all queries.  Your `answer` array should have `answer[i]` as the answer to the `i`-th query.

**Example 1:**

```
Input: A = [1,2,3,4], queries = [[1,0],[-3,1],[-4,0],[2,3]]
Output: [8,6,2,4]
Explanation: 
At the beginning, the array is [1,2,3,4].
After adding 1 to A[0], the array is [2,2,3,4], and the sum of even values is 2 + 2 + 4 = 8.
After adding -3 to A[1], the array is [2,-1,3,4], and the sum of even values is 2 + 4 = 6.
After adding -4 to A[0], the array is [-2,-1,3,4], and the sum of even values is -2 + 4 = 2.
After adding 2 to A[3], the array is [-2,-1,3,6], and the sum of even values is -2 + 6 = 4.
```

**Note:**

1. `1 <= A.length <= 10000`
2. `-10000 <= A[i] <= 10000`
3. `1 <= queries.length <= 10000`
4. `-10000 <= queries[i][0] <= 10000`
5. `0 <= queries[i][1] < A.length`

这题题目看上去有点迷，但看完栗子就清楚了要干嘛了。如果用brute force解，这个会是O(nk)的复杂度，因为每次操作都遍历一次，k次操作，遍历n个数。那么怎么优化呢？我们先算好一个偶数的sum，O(n)。然后每次操作，就根据这个数原先的奇偶来判断是否需要多evenTotal做修改。最后记得修改原数组，因为同一个位置可能改几遍。优化以后T:(n + k)

```java
public int[] sumEvenAfterQueries(int[] A, int[][] queries) {
    if (A == null || A.length == 0 || queries == null || queries.length == 0) {
        return null;
    }

    int evenTotal = 0;
    for (int num : A) {
        if (num % 2 == 0)  {
            evenTotal += num;
        }
    }

    List<Integer> tmp = new ArrayList<>();
    for (int[] query : queries) {
        int val = query[0];
        int loc = query[1];

        int original = A[loc];
        int modified = original + val;
        if (original % 2 == 0) {
            if (modified % 2 == 0) {
                evenTotal = evenTotal - original + modified;
            } else {
                evenTotal = evenTotal - original;
            }
        } else {
            if (modified % 2 == 0) {
                evenTotal += modified;
            } else {
                // no need to change anything   
            }
        }
        A[loc] = modified;
        tmp.add(evenTotal);
    }

    int[] result = new int[tmp.size()];
    for (int i = 0; i < tmp.size(); i++) {
        result[i] = tmp.get(i);
    }

    return result;
}
```

