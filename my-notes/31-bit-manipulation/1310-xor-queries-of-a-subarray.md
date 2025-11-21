# 1310 XOR Queries of a Subarray

&#x20;Given the array `arr` of positive integers and the array `queries` where `queries[i] = [Li, Ri]`, for each query `i` compute the **XOR** of elements from `Li` to `Ri` (that is, `arr[Li]`` `**`xor`**` ``arr[Li+1]`` `**`xor`**` ``...`` `**`xor`**` ``arr[Ri]` ). Return an array containing the result for the given `queries`.

**Example 1:**

```
Input: arr = [1,3,4,8], queries = [[0,1],[1,2],[0,3],[3,3]]
Output: [2,7,14,8] 
Explanation: 
The binary representation of the elements in the array are:
1 = 0001 
3 = 0011 
4 = 0100 
8 = 1000 
The XOR values for queries are:
[0,1] = 1 xor 3 = 2 
[1,2] = 3 xor 4 = 7 
[0,3] = 1 xor 3 xor 4 xor 8 = 14 
[3,3] = 8
```

**Example 2:**

```
Input: arr = [4,8,2,10], queries = [[2,3],[1,3],[0,0],[0,3]]
Output: [8,0,4,4]
```

**Constraints:**

* `1 <= arr.length <= 3 * 10^4`
* `1 <= arr[i] <= 10^9`
* `1 <= queries.length <= 3 * 10^4`
* `queries[i].length == 2`
* `0 <= queries[i][0] <= queries[i][1] < arr.length`

这题一看，感觉套路怎么这么像prefix Sum，但是这里换了xor。自己算了一下发现，其实xor也有加法性质。（note：另外log也有，提醒自己一下）一开始用0就ok了，因为0异或任何数等于任何数。T:O(n), S:O(n)

```java
public int[] xorQueries(int[] arr, int[][] queries) {
    if (arr == null || arr.length == 0 || queries == null || queries.length == 0) {
        return null;
    }

    int n = arr.length;
    int[] prefixXor = new int[n + 1];
    for (int i = 1; i <= n; i++) {
        prefixXor[i] = prefixXor[i - 1] ^ arr[i - 1];
    }

    int m = queries.length;
    int[] result = new int[m];
    for (int i = 0; i < m; i++) {
        int from = queries[i][0];
        int to = queries[i][1];

        result[i] = prefixXor[to + 1] ^ prefixXor[from];
    }

    return result;
}
```
