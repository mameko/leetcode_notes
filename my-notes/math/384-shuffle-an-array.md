# 384 Shuffle an Array

Shuffle a set of numbers without duplicates.

**Example:**

```
// Init an array with set 1, 2, and 3.
int[] nums = {1,2,3};
Solution solution = new Solution(nums);

// Shuffle the array [1,2,3] and return its result. Any permutation of [1,2,3] must equally likely to be returned.
solution.shuffle();

// Resets the array back to its original configuration [1,2,3].
solution.reset();

// Returns the random shuffling of array [1,2,3].
solution.shuffle();
```

这题纯考shuffle算法，这里用的是CC189上写的那个算法。

```java
class Solution {
    int[] original;
    public Solution(int[] nums) {
        original = nums;
    }

    /** Resets the array to its original configuration and return it. */
    public int[] reset() {
        return original;
    }

    /** Returns a random shuffling of the array. */
    public int[] shuffle() {
        int len = original.length;
        int[] res = new int[len];
        for (int i = 0; i < len; i++) {
            res[i] = original[i];
        }
        Random r = new Random();
        for (int i = 0; i < len; i++) {
            int k = r.nextInt(i + 1);
            int tmp = res[k];
            res[k] = res[i];
            res[i] = tmp;
        }

        return res;
    }
}

/**
 * Your Solution object will be instantiated and called as such:
 * Solution obj = new Solution(nums);
 * int[] param_1 = obj.reset();
 * int[] param_2 = obj.shuffle();
 */
```
