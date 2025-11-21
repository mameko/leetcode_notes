# 260 Single Number III

## 260 Single Number III

## Description

Given`2*n + 2`numbers, every numbers occurs twice except two, find them.

## Example

Given`[1,2,2,3,4,4,5,3]`return`1`and`5`

## Challenge

O(n) time, O(1) extra space.

其实是要把那两个数分到两组然后来一次Single number I。这题首先用single number I的方法找出两个数的xor。 然后找到它们中某位为1，然后分开两组。找1的方法是A & -A，找出最右一位1。然后把A里的数字分成两批，分别执行Single Number I。最后求出那两个数。

```java
public List<Integer> singleNumberIII(int[] A) {
    List<Integer> res = new ArrayList<>();
    if (A == null || A.length == 0) {
        return res;
    }

    int aXorb = 0;
    for (int i = 0; i < A.length; i++) {
        aXorb ^= A[i];
    }

    int a = 0;
    int b = 0;
    int diff = aXorb & -aXorb;// take the right most 1 out

    for (int i = 0; i < A.length; i++) {
        if ((diff & A[i]) == 0) {
            a ^= A[i];
        } else {
            b ^= A[i];
        }
    }

    res.add(a);
    res.add(b);

    return res;
}
```
