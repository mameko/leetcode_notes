# 605 Can Place Flowers

Suppose you have a long flowerbed in which some of the plots are planted and some are not. However, flowers cannot be planted in adjacent plots - they would compete for water and both would die.

Given a flowerbed (represented as an array containing 0 and 1, where 0 means empty and 1 means not empty), and a number**n**, return if**n**new flowers can be planted in it without violating the no-adjacent-flowers rule.

**Example 1:**

```
Input: flowerbed = [1,0,0,0,1], n = 1
Output: True
```

**Example 2:**

```
Input: flowerbed = [1,0,0,0,1], n = 2
Output: False
```

**Note:**

1. The input array won't violate no-adjacent-flowers rule.
2. The input array size is in the range of \[1, 20000].
3. **n** is a non-negative integer which won't exceed the input array size.

这题一看，只想到了brute force，然后一边check一遍place flower。然后发现边界好tricky。在想是不是有什么gready的最优解法。然而是没有的。答案也是这样，就是写得好看点而已。都是T:O(N)，S：O(1)

```java
public boolean canPlaceFlowers(int[] flowerbed, int n) {
    if (flowerbed == null || flowerbed.length == 0 || n < 0) {
        return false;
    }

    for (int i = 0; i < flowerbed.length; i++) {
        if (flowerbed[i] == 0) {
            if (i == 0 && i == flowerbed.length - 1) {
                n--;         
                flowerbed[i] = 1;
            } else if (i == 0 && flowerbed[i + 1] == 0) {
                n--;         
                flowerbed[i] = 1;
            } else if (i == flowerbed.length - 1 && flowerbed[i - 1] == 0) {
                n--;         
                flowerbed[i] = 1;
            } else if (i != 0 && flowerbed[i - 1] == 0 && i != flowerbed.length - 1 && flowerbed[i + 1] == 0) {
                n--;         
                flowerbed[i] = 1;
            }
        }
    }

    return n <= 0;
}

//答案的写法：
public boolean canPlaceFlowers(int[] flowerbed, int n) {
    if (flowerbed == null || flowerbed.length == 0 || n < 0) {
        return false;
    }

    for (int i = 0; i < flowerbed.length; i++) {
        if (flowerbed[i] == 0 
            && (i == 0 || flowerbed[i - 1] == 0) 
            && (i == flowerbed.length - 1 || flowerbed[i + 1] == 0)) {
            n--;         
            flowerbed[i] = 1;            
        }
    }

    return n <= 0;
}
```
