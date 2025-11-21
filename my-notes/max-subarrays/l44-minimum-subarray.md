# L44 Minimum Subarray

Given an array of integers, find the subarray with smallest sum.

Return the sum of the subarray.

## Notice

The subarray should contain one integer at least.

**Example**

For`[1, -1, -2, 1]`, return`-3`.

every element multiply -1

then find max subarray sum

then sum \* -1

```java
public int minSubArray(ArrayList<Integer> nums) {
    if (nums == null || nums.size() == 0) {
        return 0;
    }

    for (int i = 0; i < nums.size(); i++) {
        nums.set(i, nums.get(i) * -1);
    }

    int sum = 0;
    int max = Integer.MIN_VALUE;
    for (int i = 0; i < nums.size(); i++) {
        sum = sum + nums.get(i);
        if (sum > max) {
            max = sum;
        }

        if (sum < 0) {
            sum = 0;
        }
    }

    return max * -1;
}
```
