# 477 Total Hamming Distance

The[Hamming distance](https://en.wikipedia.org/wiki/Hamming_distance)between two integers is the number of positions at which the corresponding bits are different.

Now your job is to find the total Hamming distance between all pairs of the given numbers.

**Example:**

```
Input: 4, 14, 2
Output: 6


Explanation:
 In binary representation, the 4 is 0100, 14 is 1110, and 2 is 0010 (just
showing the four bits relevant in this case). So the answer will be:
HammingDistance(4, 14) + HammingDistance(4, 2) + HammingDistance(14, 2) = 2 + 2 + 2 = 6.
```

**Note:**

1. Elements of the given array are in the range of`0`to`10^9`
2. Length of the array will not exceed`10^4`.

这题只想到了brute force的方法，就是n方地两两求一个hamming distance。看了答案发现能够通过算每一位的diff，然后把它们加起来。

```java
    public int totalHammingDistance(int[] nums) {
        int total = 0;// 总共的difference
        int n = nums.length;

        for (int i = 0; i < 32; i++) {// 32 位整数
            int count = 0;
            for (int j = 0; j < n; j++) {
                count += (nums[j] >> i) & 1;// 看这一位是否为1，数所有数字的这一位有多少个1
            }

            total += count * (n - count);// 如果有count个1，我们就有n-count个0，然后我们一diff就都得加上
        }

        return total;
    }
```
