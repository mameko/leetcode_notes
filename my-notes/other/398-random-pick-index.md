# 398 Random Pick Index

Given an array of integers with possible duplicates, randomly output the index of a given target number. You can assume that the given target number must exist in the array.

**Note:**\
The array size can be very large. Solution that uses too much extra space will not pass the judge.

**Example:**

```
int[] nums = new int[] {1,2,3,3,3};
Solution solution = new Solution(nums);

// pick(3) should return either index 2, 3, or 4 randomly. Each index should have equal probability of returning.
solution.pick(3);

// pick(1) should return 0. Since in the array only nums[0] is equal to 1.
solution.pick(1);
```

一看题我还以为要用hashmap来存起每个值的位置，然后pick的时候再从位置们里面random挑。看了答案原来要用水塘抽样。其实不是特别懂原理，感觉水塘抽样跟shuffle很像，都是框起前面一截，然后从哪一截里任意取一个。

```java
    Random r;
    int[] n;
    public Solution(int[] nums) {
        r = new Random();
        n = nums;
    }

    public int pick(int target) {
        int result = 0;
        int count = 0;
        for (int i = 0; i < n.length; i++) {
            if (n[i] != target) {
                continue;
            }

            if (r.nextInt(++count) == 0) {
                result = i;
            }
        }

        return result;
    }
```
