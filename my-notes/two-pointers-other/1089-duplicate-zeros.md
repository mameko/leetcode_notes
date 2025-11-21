# 1089 Duplicate Zeros

Given a fixed-length integer array `arr`, duplicate each occurrence of zero, shifting the remaining elements to the right.

**Note** that elements beyond the length of the original array are not written. Do the above modifications to the input array in place and do not return anything.

&#x20;

**Example 1:**

<pre><code><strong>Input: arr = [1,0,2,3,0,4,5,0]
</strong><strong>Output: [1,0,0,2,3,0,0,4]
</strong><strong>Explanation: After calling your function, the input array is modified to: [1,0,0,2,3,0,0,4]
</strong></code></pre>

**Example 2:**

<pre><code><strong>Input: arr = [1,2,3]
</strong><strong>Output: [1,2,3]
</strong><strong>Explanation: After calling your function, the input array is modified to: [1,2,3]
</strong></code></pre>

&#x20;

**Constraints:**

* `1 <= arr.length <= 104`
* `0 <= arr[i] <= 9`

这题，如果可以用额外空间的话，很容易。但是，要S: O（1）的话，比较麻烦。一开始就想到了，数0，因为每个0会+2，所以只要数下标数到超过原arr长度就ok了。然后从后到前copy，见到0就copy 2个。但是有一个edge case想不通怎么特判掉，\[0, 0, 1]的时候，最后一个0其实是不用dup的。后来看了solution改良了一下自己的想法，用一个boolean判断掉。如果break的时候下标刚好等于的话，是可以dup的，如果大于，就表示最后一个0不用dup。

```java
public void duplicateZeros(int[] arr) {
    if (arr == null || arr.length == 0) {
        return;
    }

    int cnt = 0;
    int size = arr.length;
    int i = 0;
    boolean lastDup = true;
    // 数下标，大于等于size的时候就可以break了
    for (;i < size; i++) {
        if (arr[i] != 0) {
            cnt = cnt + 1;
        } else {
            cnt = cnt + 2;
        }
        if (cnt == size) {
            break; // 刚刚好，最后的0刚好装进去
        } else if (cnt > size) {
            lastDup = false; // 最后的0不能dup，dup就超出了size
            break;
        }
    }

    // 从后到前assign
    int j = size - 1;
    while (j >= 0) {
        // 最后一个0，特判一下
        if (j == size - 1 && arr[i] == 0) {
            if (lastDup) {//果可dup，多加一个
                arr[j--] = arr[i];
            }                
            arr[j--] = arr[i]; // dup不dup都得加上
        } else if (arr[i] == 0) {//见到0，assign两个
            arr[j--] = arr[i];
            arr[j--] = arr[i];
        } else {// 其他数字直接复制过去
            arr[j--] = arr[i];
        }
        i--;            
    }
    
}
```
