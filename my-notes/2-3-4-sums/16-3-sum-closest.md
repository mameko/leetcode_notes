# 16 3 Sum Closest

Given an arraySofnintegers, find three integers inSsuch that the sum is closest to a given number, target. Return the sum of the three integers. You may assume that each input would have exactly one solution.

```
For example, given array S = {-1 2 1 -4}, and target = 1.

    The sum that is closest to the target is 2. (-1 + 2 + 1 = 2).
```

这题跟2sum closest很像，2 pointer。这里因为保证有1个solution，所以不用判断数组小于3的情况。下面代码可以简单点。

```java
public int threeSumClosest(int[] numbers, int target) {
    int res = Integer.MIN_VALUE;
    if(numbers == null || numbers.length == 0){
        return res;
    }

    int sum = 0;
    int len = numbers.length;
    int mindiff = Integer.MAX_VALUE;
    Arrays.sort(numbers);

    if(len < 4){
        for(int i = 0;i< len; i++){
            sum = sum + numbers[i];
        }

        return sum;
    } else{
        for(int i = 0;i<3;i++){
            sum = sum + numbers[i];
        }
    }

    res = sum;
    mindiff = Math.abs(sum - target);
    for(int i = 0;i<len -1; i++){
        int left = i+1;
        int right = len -1;
        while(left < right){
            sum = numbers[left] + numbers[right] + numbers[i];
            int diff = Math.abs(sum - target);
            if(diff < mindiff){
                mindiff = diff;
                res = sum;
            }
            if(sum < target){
                left++;
            }else{
                right--;
            }
        }
    }
    return res;        
}
```
