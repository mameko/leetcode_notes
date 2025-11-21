# 5 去重基本方法

首先通过排序把相同的数字排到一齐。

然后在选择放入集合时，挑第一个出现的。

具体做法参照[L16 Permutation II](../my-notes/3-left-right-tranvesal-max-array-stocks/l16-permutation-ii.md)

```java
//这一小段就是去重代码，如果我们在挑的数已经出现过，而且前面的那个还没被选进现在的buffer里。
//我们跳过不选这个，选前面后的。选前面的
if (i > 0 && nums[i] == nums[i - 1] && visited[i - 1] == false) {
    continue;
}
```

另外[3sum](../my-notes/2-3-4-sums/15-3-sum.md)也有去重代码，很类似

```java
for(int i = 0; i< len - 2;i++){ // remmeber to end when we were at the 3rd last element
    //skip duplicate here
    if(i!=0 && numbers[i] == numbers[i-1]){
    continue;
    .
    .
    .
    while (...) {
        ...
        //and here
        while(left<right&&numbers[left] == numbers[left-1]){
        left++;
        }
        //and here
        while(left<right&&numbers[right] == numbers[right+1]){
        right--;
        }
    }
}
```
