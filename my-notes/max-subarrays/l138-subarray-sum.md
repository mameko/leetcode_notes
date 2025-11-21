# L138 Subarray Sum

Given an integer array, find a subarray where the sum of numbers is **zero**. Your code should return the index of the first number and the index of the last number.

## Notice

There is at least one subarray that it's sum equals to zero.

**Example**

Given`[-3, 1, 2, -3, 4]`, return`[0, 2]`or`[1, 3]`.

这题求的是sum为0，一开始做的时候在蛋疼地slide window。后来发现原来是要用hashmap做。

1. 把数组内元素逐个相加
2. 如果有等于0的subarray的话，在那些数之前的sum跟过了subarray之后的sum会相等。所以用一个Hashmap来存储sum和位置。
3. 最后，如果sum了整个array都没有找到的话，就没有了。

例子：

```
                 [-5,    10,     5,    -3,     1,     1,     1]
  HashMap:
  sum             -5      5     10      7      8      9     10 
  i                0      1      2      3      4      5      6

return [3, 6] -- （i + 1， j）
```

```java
public ArrayList<Integer> subarraySum(int[] nums) {
    ArrayList<Integer> result = new ArrayList<Integer>();

    if(nums == null || nums.length == 0) {
        return result;
    }

    HashMap<Integer, Integer> map = new HashMap<>();
    int sum = 0;
    int len = nums.length;
    for (int i = 0 ;i < len; i++){
        sum = sum + nums[i];
        if (map.containsKey(sum)){
            result.add(map.get(sum) + 1);
            result.add(i);
            break;
        } else if(sum == 0){ // [1,-1]，add this because you don't have next value to return in first branch
            result.add(0);
            result.add(i);
            break;
        }
        map.put(sum,i);
    }
    return result;
}
```
