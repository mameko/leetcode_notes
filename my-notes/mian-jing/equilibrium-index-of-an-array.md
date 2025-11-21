# Equilibrium index of an array

Equlibrium index of an array is an index such that the sum of elements at lower indexes is equal to the sum of elements at higher indexes. For example, in an array A: \[-7, 1, 5, 2, -4, 3, 0], index 3 is an equilibrium index, because A\[0] + A\[1] + A\[2] = A\[4] + A\[5] + A\[6] = -1. 6 is also an equlibrium index.

这题解法用leftSum和totalSum来判断。

```java
public equilibrium(int arr[]) {
    int totalSum = 0;// init sum of whole array
    int leftSum = 0;// init leftsum to 0, start from first element of array

    // calculate teh whole array sum
    for (int i = 0; i < arr.length; i++) {
        totalSum += arr[i];
    }

    for (int i = 0; i < arr.length; i++) {
        totalSum = totalSum - arr[i];

        if (leftSum == totalSum) {
            return i;// can also add to res list if we want all the indexs
        }

        leftSum = leftSum + arr[i];
    }

    return -1;
}
```
