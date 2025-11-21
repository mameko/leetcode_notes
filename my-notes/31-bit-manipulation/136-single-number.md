# 136 Single Number

## 136 Single Number

## Description

Given`2*n + 1`numbers, every numbers occurs twice except one, find it.

## Example

Given`[1,2,2,1,3,4,3]`, return`4`

## Challenge

One-pass, constant extra space.

这题利用的是异或的性质。loop一遍O(n)做完。

```java
public int singleNumber(int[] A) {

    if(A==null||A.length==0){
        return 0;
    }

    int result = 0;
    for(int i=0;i<A.length;i++){
        result = result^A[i];
    }

    return result;
}
```
