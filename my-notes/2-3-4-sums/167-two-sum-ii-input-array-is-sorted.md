# 167 Two Sum II - input array is sorted

Given an array of integers that is already**sorted in ascending order**, find two numbers such that they add up to a specific target number.

The function twoSum should return indices of the two numbers such that they add up to the target, where index1 must be less than index2. Please note that your returned answers (both index1 and index2) are not zero-based.

You may assume that each input would have exactly one solution.

**Input:**&#x6E;umbers={2, 7, 11, 15}, target=9\
**Output:**&#x69;ndex1=1, index2=2

注意，这是1开始，LC真是蹭得累

```java
public int[] twoSum(int[] numbers, int target) {
    int[] res = new int[2];
    if (numbers == null || numbers.length == 0) {
        return res;
    }

    int n = numbers.length;
    int i = 0; 
    int j = n - 1;
    while (i < j) {
        int sum = numbers[i] + numbers[j];
        if (sum == target) { 
            //题目要求从1开始，所以统统加1
            res[0] = i + 1;
            res[1] = j + 1;
            return res;
        } else if (sum > target) {
            j--;
        } else {
            i++;
        }
    }
    return res;
}
```
