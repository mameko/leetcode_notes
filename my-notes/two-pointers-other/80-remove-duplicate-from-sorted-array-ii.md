# 80 Remove Duplicate from Sorted Array II

Follow up for "Remove Duplicates":\
What if duplicates are allowed at mosttwice?

For example,\
Given sorted arraynums=`[1,1,1,2,2,3]`,

Your function should return length =`5`, with the first five elements ofnumsbeing`1`,`1`,`2`,`2`and`3`. It doesn't matter what you leave beyond the new length.

这题很难，要用3个指针。cur是用来记录结果array的下标。然后用i和j来一段一段找该放到结果里的部分。首先，3个指针都在起始点，然后用一个循环把j往后移，如果数字不同，我们可以break掉，然后把i移到j的位置。这里，因为j一开始是从i开始，所以j必定会往后移一格，这样就保证不会死循环。如果数字相同，我们算一下位置差值，如果小于2（题目要求），我们就把那个数字放到结果里，然后把结果下标++。

```java
public int removeDuplicates(int[] nums) {
    if (nums == null || nums.length == 0) {
        return 0;
    }

    int i = 0;
    int j = 0;
    int cur = 0;
    while (i < nums.length) {
        int curNum = nums[i];
        for (j = i; j < nums.length; j++) {
            if (nums[j] != curNum) {
                break;
            }

            if (j - i < 2) {
                nums[cur++] = curNum;
            } 
        }

        i = j;
    }

    return cur;
}
```
