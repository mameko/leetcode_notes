# hankerrank Data Updates

![](<../../.gitbook/assets/image (23).png>)

嘛，一看能brute force，但反复改来改去的，一定有什么好点的方法。想了几分钟，发现，可以统计奇偶值来决定最后到底要不要flip那一位。index从1开始这个有点奇怪，调了一阵子。

```java
public static List<Integer> getFinalData(List<Integer> data, 
                                         List<List<Integer>> updates) {
    if (data == null || data.isEmpty()) {
        return data;
    }

    List<Integer> result = new ArrayList<>();
    int sizeOfData = data.size();
    int[] counter = new int[sizeOfData];
    for (List<Integer> update : updates) {
        int start = update.get(0);
        int end = update.get(1);
        for (int i = start - 1; i < end; i++) {
            counter[i]++;
        }
    }
    
    for (int i = 0; i < sizeOfData; i++) {
        if (counter[i] % 2 != 0) { // 奇数的要flip
            result.add(data.get(i) * -1);
        } else {
            result.add(data.get(i)); // 偶数的负负得正，不用flip
        }
    }
    
    return result;
}
```
