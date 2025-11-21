# L50 Product of Array Exclude itself

Given an array ofnintegers wheren> 1,`nums`, return an array`output`such that`output[i]`is equal to the product of all the elements of`nums`except`nums[i]`.

Solve it **without division** and in O(n).

For example, given`[1,2,3,4]`, return`[24,12,8,6]`.

**Follow up:**\
Could you solve it with constant space complexity? (Note: The output array **does not** count as extra space for the purpose of space complexity analysis.)

```java
public int[] productExceptSelf(int[] nums) {
    if (nums == null || nums.length < 2) {
        return nums;
    }

    int n = nums.length;
    int[] res = new int[n];
    res[0] = 1;
    for (int i = 0; i < n - 1; i++) {
        res[i + 1] = nums[i] * res[i];
    }

    int[] tmp = new int[n];
    tmp[n - 1] = 1;
    for (int j = n - 2; j >= 0; j--) {
        tmp[j] = tmp[j + 1] * nums[j + 1];
    }

    for (int i = 0; i < n; i++) {
        res[i] = res[i] * tmp[i];
    }
    return res;
    }
```

还有一个方法是另开一个变量：

```java
public int[] arrProduct(int[] nums) {
   if (nums == null || nums.length == 0) {
      return new int[0];
   }

   int product = 1;
   int[] temp = new int[nums.length];
   Arrays.fill(tmep, 1);

   // go form left to right
   for (int i = 0; i < nums.lnegth; i++) { 
      temp[i] = product * temp[i];
      product = product * nums[i];
   }

   // right to left
   product = 1;
   for (int j = nums.length - 1; j >= 0; j--) {
      temp[j] = temp[j] * product;
      product = product * nums[j];
   }

   return temp;
}
```
