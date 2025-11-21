# 41 First Missing positive

Given an **unsorted** integer array, find the first **missing** positive integer.

**Example**

Given`[1,2,0]`return`3`,\
and`[3,4,-1,1]`return`2`.

[**Challenge**](http://www.lintcode.com/en/problem/first-missing-positive/#challenge)

Your algorithm should run in O(_n_) time and uses constant space.

这题的难度在于要constant space，如果不用constant的话，可以用set来check。其实pramp里有一题很像，就是少了重复出现数据，不会有负数之类的。基本一样。pramp里那题是从0开始，这题是从1开始，所以要i + 1判断，有点麻烦。把pramp的也贴上来作为参考。

```java
public int firstMissingPositive(int[] nums) {
    if (nums == null || nums.length == 0) {
        return 1;
    }

    for (int i = 0; i < nums.length; i++) {
        while (i + 1 != nums[i] && nums[i] <= nums.length && nums[i] > 0) {
            int tmp = nums[i];
            if (nums[tmp - 1] == tmp) {
                break;
            }
            nums[i] = nums[tmp - 1];
            nums[tmp - 1] = tmp;
        }
    }

    for (int i = 0; i < nums.length; i++) {
        if (nums[i] != i + 1) {
            return i + 1;
        }
    }

    return nums.length + 1;
}

// Pramp题，找第一个missing非负数
class Solution {

  /*static int getDifferentNumber(int[] arr) { // 用了set
    if (arr == null || arr.length == 0) {
      return -1;
    }

    Set<Integer> set = new HashSet<>();
    for (int num : arr) {
      set.add(num);
    }

    int i = 0;
    for (i = 0; i < arr.length(); i++) {
      if (!set.contains(i)) {
        return i;
      }
    }

    return i;
  }*/

  static int getDifferentNumber(int[] arr) { // constant space的做法
    if (arr == null || arr.length == 0) {
      return -1;
    }    


    for (int i = 0; i < arr.length; i++) {
      if (arr[i] < arr.length && i != arr[i]) {
        int tmp = arr[i]; // i = 1, tmp = 0
        arr[i] = arr[tmp];//  arr[1] = 3
        arr[tmp] = tmp;   //  arr[0] = 0
      }
    }
    //0,1,2,3,4,5,6
    //0,5,4,1,3,6,2
    //0,5,4,1,3,6,2
    //0,6,4,1,3,5,2
    //0,2,4,1,3,5,6
    //0,4,2,1,3,5,6
    //0,1,2,4,3,5,6



    //0,6,4,1,3,5,2
    //0,6,3,1,4,5,2
    //0,1,3,6,4,5,2
    //0,1,3,6,4,5,2
    //0,1,2,6,4,5,3
    int i = 0;
    for (i = 0; i < arr.length; i++) {
      if (arr[i] != i) {
        return i;
      }
    }

    return i;
  }
```

这题有点难度，好吧，把自己以前做错了的答案和解释也贴上来了。做法大概是，利用原来的数组，把A\[i]放到A\[A\[i] - 1]的位置，就是1 放到A\[0], 2放到A\[1] etc。其实不是很懂...

```java
    public int firstMissingPositive(int[] A) {
        if (A == null || A.length == 0 ){
            return 1;
        }
        int n = A.length;
        for(int i=0;i<n;i++){// loop一遍数组
            while(A[i] != i+1){//发现没有放好的数字，例如A[0]里放的不是1
                if(A[i] <=0 || A[i] > n){// 跳过负数和大于n的数，因为缺的那个正数不会比n大
                    break;
                }

                if(A[i] == A[A[i]-1]){
                    break;
                }

                //swap
                int temp = A[i];
                A[i] = A[temp - 1];
                A[temp -1] = temp;
            }
        }

        for(int i= 0; i < n; i++){
            if(A[i]!=i+1){
                return i+1;
            }
        }

        return n+1;
    }
```

```
solution: 
because the missing +number of element in array won't exceed array length, so use the array itself to store the elements.
skip -nums & nums > n
swap nums with element in the correct position 
in the end check from front to end, see which one is missing

-------------only works for array without dup element---------
--misunderstood requirement, 3,4,5 -> return 1
--won't work if there are no consecutive +num sequence, eg. [1, 5]
* loop through array, find max & min, sum of all the +num, total number of +number
if +number == max - min +1 means continue, if min == 1 return max + 1, else return min -1
else //means missing in between max & min
gauss (max + min)* (max - min +1)/2 - sum all+num

  public int firstMissingPositive(int[] A) {
    if (A == null || A.length == 0 ){
        return 1;
    }

    int max = Integer.MIN_VALUE;
    int min = Integer.MAX_VALUE;
    int count = 0;
    int sum = 0;
    for(int i = 0; i < A.length; i++){
        if(max < A[i]){
            max = A[i];
        }

        if(min > A[i]){
            min = A[i];
        }

        if(A[i] > 0){
            count++;
            sum = sum + A[i];
        }
    }

    if(count == 0){
        return 1;
    }

    int diff = max - min + 1;
    if (diff == count){
        if(min == 1){
            return max + 1;
        }else{
            return min - 1;
        }
    }else{
        int total = (max + min) * (diff)/2;
        return total - sum;
    }


}
```
