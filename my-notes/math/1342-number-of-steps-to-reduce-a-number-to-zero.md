# 1342 Number of Steps to Reduce a Number to Zero

Given a non-negative integer `num`, return the number of steps to reduce it to zero. If the current number is even, you have to divide it by 2, otherwise, you have to subtract 1 from it.

**Example 1:**

```
Input: num = 14
Output: 6
Explanation: 
Step 1) 14 is even; divide by 2 and obtain 7. 
Step 2) 7 is odd; subtract 1 and obtain 6.
Step 3) 6 is even; divide by 2 and obtain 3. 
Step 4) 3 is odd; subtract 1 and obtain 2. 
Step 5) 2 is even; divide by 2 and obtain 1. 
Step 6) 1 is odd; subtract 1 and obtain 0.
```

**Example 2:**

```
Input: num = 8
Output: 4
Explanation: 
Step 1) 8 is even; divide by 2 and obtain 4. 
Step 2) 4 is even; divide by 2 and obtain 2. 
Step 3) 2 is even; divide by 2 and obtain 1. 
Step 4) 1 is odd; subtract 1 and obtain 0.
```

**Example 3:**

```
Input: num = 123
Output: 12
```

**Constraints:**

* `0 <= num <= 10^6`

这题，本来以为有什么数学方法做的，然后看了提示，说硬刚。那么我就上了。不过幸好不难。T:O(logn)，每次数字几乎都是除以二。S:O(1)。这里其实还可以用bit manipulation。时间复杂度不变，就贴上来参考。最后以为是0，我们就用一步操作，right shift，如果是1，我们要两步操作，减一再right shift。但要注意，最开始的那一位，我们只能算一次，因为不用right shift了。

```java
public int numberOfSteps (int num) {
    if (num < 1) {
        return 0;
    }
    
    int count = 0;
    while (num > 0) {
        if (num % 2 == 0) {
            num = num / 2;
        } else {
            num = num - 1;
        }
        count++;
    }
    
    return count;
}
    
// 参考答案
public int numberOfSteps(int num) {
    
    // Get the binary for num, as a String.
    String binaryString = Integer.toBinaryString(num);
    
    int steps = 0;
    // Iterate over all the bits in the binary string.
    for (char bit : binaryString.toCharArray()) {
        if (bit == '1') { // If the bit is a 1 
            steps = steps + 2; // Then it'll take 2 to remove.
        } else { // bit == '0'
            steps = steps + 1; // Then it'll take 1 to remove.
        }
    }

    // We need to subtract 1, because the last bit was over-counted.
    return steps - 1;
}   

// 还能这样写，每一位检查是否是1
public int numberOfSteps(int num) {

    // We need to handle this as a special case, otherwise it'll return -1.
    if (num == 0) return 0;

    int steps = 0;

    for (int powerOfTwo = 1; powerOfTwo <= num; powerOfTwo = powerOfTwo * 2) {
        // Apply the bit mask to check if the bit at "powerOfTwo" is a 1.
        if ((powerOfTwo & num) != 0) {
            steps = steps + 2;
        } else {
            steps = steps + 1;
        }
    }

    // We need to subtract 1, because the last bit was over-counted.
    return steps - 1;
}
```
