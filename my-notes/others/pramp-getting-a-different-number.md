# Pramp - Getting a Different Number

Given an array`arr`of unique nonnegative integers, implement a function`getDifferentNumber`that finds the smallest nonnegative integer that is NOT in the array.

Even if your programming language of choice doesn’t have that restriction (like Python), assume that the maximum value an integer can have is`MAX_INT = 2^31-1`. So, for instance, the operation`MAX_INT + 1`would be undefined in our case.

Your algorithm should be efficient, both from a time and a space complexity perspectives.

Solve first for the case when you’re NOT allowed to modify the input`arr`. If successful and still have time, see if you can come up with an algorithm with an improved space complexity when modifying`arr`is allowed. Do so without trading off the time complexity.

Analyze the time and space complexities of your algorithm.

**Example:**

```
input:  arr = [0, 1, 2, 3]
output: 4
```

**Constraints:**

* **\[time limit] 5000ms**
* **\[input] array.integer**`arr`
  * 1 ≤ arr.length ≤ MAX\_INT
  * 0 ≤ arr\[i] ≤ MAX\_INT for every i, 0 ≤ i < MAX\_INT
* **\[output] integer**

这题在peer的引导下从超暴力的方式开始写，终于最后写到最优解。首先上S：O(n)的解。基本上就是把数组里的全丢set里。然后呢从0开始一直++，直到找到set里没有的为止。这样就找到最小的那个missing非负数了。

```java
static int getDifferentNumber(int[] arr) {
    if (arr == null || arr.length == 0) {
      return -1;
    }

    Set<Integer> set = new HashSet<>();
    for (int num : arr) {
      set.add(num);
    }

    int i = 0; // 定义在外面是如果遇到题目例子的情况，我们不会中途return
    for (i = 0; i < arr.length(); i++) { // 最暴力的是loop到INT_MAX
      if (!set.contains(i)) {
        return i;
      }
    }
    // 最后i++了，所以返回i是正确的
    return i;
  }
```

然后S：O(1)的解法。这里是要在原数组上swap。具体的做法是，如果我看到的数比数组的size大，我们知道缺的一定不是这个数。所以可以不动它。然后，如果是小于的话，我们就要把它移到该去的位置。例如，第一小的，放0位，第二小的放1位。这样做的原因是：我们数组size是N，那么缺的数一定是这\[0，N]里的。具体判断方法是如果i跟arr\[i]不等于的话，我们尝试把它移到正确位置。最后我们再来一圈，就是找\[0,N]里第一个缺的。那个就是最小的非负数了。

```java
  static int getDifferentNumber(int[] arr) {
    if (arr == null || arr.length == 0) {
      return -1;
    }    

    for (int i = 0; i < arr.length; i++) {
      while (arr[i] < arr.length && i != arr[i]) {
        int tmp = arr[i]; // i = 1, tmp = 0
        arr[i] = arr[tmp];//  arr[1] = 3
        arr[tmp] = tmp;   //  arr[0] = 0
      }
    }

    int i = 0;
    for (i = 0; i < arr.length; i++) {
      if (arr[i] != i) {
        return i;
      }
    }

    return i;
  }
```
