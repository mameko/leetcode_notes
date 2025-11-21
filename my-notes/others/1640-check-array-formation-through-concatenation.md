# 1640 Check Array Formation Through Concatenation



You are given an array of **distinct** integers `arr` and an array of integer arrays `pieces`, where the integers in `pieces` are **distinct**. Your goal is to form `arr` by concatenating the arrays in `pieces` **in any order**. However, you are **not** allowed to reorder the integers in each array `pieces[i]`.

Return `true` _if it is possible to form the array_ `arr` _from_ `pieces`. Otherwise, return `false`.

**Example 1:**

```
Input: arr = [85], pieces = [[85]]
Output: true
```

**Example 2:**

```
Input: arr = [15,88], pieces = [[88],[15]]
Output: true
Explanation: Concatenate [15] then [88]
```

**Example 3:**

```
Input: arr = [49,18,16], pieces = [[16,18,49]]
Output: false
Explanation: Even though the numbers match, we cannot reorder pieces[0].
```

**Example 4:**

```
Input: arr = [91,4,64,78], pieces = [[78],[4,64],[91]]
Output: true
Explanation: Concatenate [91] then [4,64] then [78]
```

**Example 5:**

```
Input: arr = [1,3,5,7], pieces = [[2,4,6,8]]
Output: false
```

**Constraints:**

* `1 <= pieces.length <= arr.length <= 100`
* `sum(pieces[i].length) == arr.length`
* `1 <= pieces[i].length <= arr.length`
* `1 <= arr[i], pieces[i][j] <= 100`
* The integers in `arr` are **distinct**.
* The integers in `pieces` are **distinct** (i.e., If we flatten pieces in a 1D array, all the integers in this array are distinct).

嗯，这题呢，一开始看就觉得好像只有brute force了，后来看了答案，好像真的没有什么更好的拼法。brute force就是一边按顺序拿arr里的数跟可能开始的piece对，如果找到了，就往下比对subarr。一边增加arrIndex一边比对。最后return是否找到arrIndex长度等长的数。这是因为我们有可能有更多的piece。譬如第三个栗子，如果改成【\[49], \[18], \[16, 18, 49]】我们最后判断就要用length了。

这里因为是unique的pieces然后要快速找到，所以就想到了set。因为我们要记录位置，所以用了map。S：O(n), T:O(m)，n是arr长度，m是所有pieces加起来的总数

```java
public boolean canFormArray(int[] arr, int[][] pieces) {
    if (arr == null || arr.length == 0 || pieces == null || pieces.length == 0) {
        return false;
    }

    HashMap<Integer, Integer> valueToLocation = new HashMap<>();
    for (int i = 0; i < pieces.length; i++) {
        valueToLocation.put(pieces[i][0], i);
    }

    int arrIndex = 0;

    while (arrIndex < arr.length && valueToLocation.containsKey(arr[arrIndex])) {
        int[] subArr = pieces[valueToLocation.get(arr[arrIndex])];
        for (int j = 0; j < subArr.length; j++) {
            if (arrIndex < arr.length && subArr[j] == arr[arrIndex]) {
                arrIndex++;
            } else {
                return false;
            }
        }
    } 
    return arrIndex == arr.length;
}
```
