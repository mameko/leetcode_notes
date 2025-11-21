# 254 Factor Combinations

Numbers can be regarded as product of its factors. For example,

```
8 = 2 x 2 x 2;
  = 2 x 4.
```

Write a function that takes an integer n and return all possible combinations of its factors.

**Note:**

1. You may assume that n is always positive.
2. Factors should be greater than 1 and less than n .

**Examples:**\
input:`1`\
output:

```
[]
```

input:`37`

output:

```
[]
```

input:`12`

output:

```
[
  [2, 6],
  [2, 2, 3],
  [3, 4]
]
```

input:`32`

output:

```
[
  [2, 16],
  [2, 2, 8],
  [2, 2, 2, 4],
  [2, 2, 2, 2, 2],
  [2, 4, 4],
  [4, 8]
]
```

这题在递归时要注意控制什么时候继续。把开始的数字和现在数字是多少传传进递归函数。T:O(2^n), S:(n),递归深度其实是这个数要除多少次被除尽。

eg：12

```
(12, 2, [], [])
  ---(6, 2, [2], [])
    ---(3, 2, [2, 2], [])
      ---(3 can't be divided by 2, so out of recur function)
    ---(3, 3, [2, 2], [])
      ---(1, [2, 2, 3],[]) (n == 1, add [2, 2, 3] to result
        .
        .
        .
```

```java
public List<List<Integer>> getFactors(int n) {
    List<List<Integer>> result = new ArrayList<>();
    if (n < 1) {
        return result;
    }

    ArrayList<Integer> tmp = new ArrayList<>();
    dfsHelper(n, 2, tmp, result);
    // remove [n] from result,例如n=32，32会被加到结果里，要去掉
    result.remove(result.size() - 1);
    return result;
}

private void dfsHelper(int n, int start, ArrayList<Integer> tmp, 
                       List<List<Integer>> result) {
    if (n == 1) {// 除尽的时候可以把tmp结果加到result里了
        result.add(new ArrayList<>(tmp));
        return;
    }

    // begin from start to remove dup，
    // = because we need to divide the number till 1
    for (int i = start; i <= n; i++) {
        if (n % i != 0) { // can't completly divide
            continue;
        }

        tmp.add(i);
        dfsHelper(n / i, i, tmp, result);
        tmp.remove(tmp.size() - 1);
    }

}
```
