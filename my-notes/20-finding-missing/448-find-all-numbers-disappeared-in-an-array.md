# 448 Find All Numbers Disappeared in an Array

Given an array of integers where 1 ≤ a\[i] ≤ n (n = size of array), some elements appear twice and others appear once.

Find all the elements of \[1, n] inclusive that do not appear in this array.

Could you do it without extra space and in O(n) runtime? You may assume the returned list does not count as extra space.

**Example:**

```
Input:
[4,3,2,7,8,2,3,1]

Output:
[5,6]
```

这题，如果用set的话很简单。直接把数组放里面。然后再来一个loop，不在set里的，就一定是missing的那些。或者用hashmap来数数。count等于0的就是missing的。

如果这题不是要求O(n)时间复杂度，也可以用sort，然后再过一遍来看哪些miss掉。

但是，这里有一个要求，是不用extra space，但返回数组不算，所以只能重复利用那个来做文章。另外发现这是有顺序的而且是1到n。所以可以用数组下标和所对应的数来判断是否miss。

具体做法如下：

首先把原来的数放到结果里，然后把，存在的数放到对应的下标。譬如，\[4, 3, 2, 7, 8, 2, 3, 1] => \[1, 2, 3, 4, X, X, 7, 8]，直接覆盖数组里的。最后，我们用一个loop来判断【i】的数字是否等于i+1，如果不等于，就是哪些miss掉的数了。这个例子里，是5，6。这题还有一个trap，就是要从后往前loop，不然下标就会不对，数组越来越短...其实我这个做法，S：可能没太O（1），solution里的解法是真O（1）。

解法一：

```java
public List<Integer> findDisappearedNumbers(int[] nums) {
        List<Integer> res = new ArrayList<>();
        if (nums == null || nums.length == 0) {
            return res;
        }
        
        int n = nums.length;
        for (int i = 0; i < n; i++) {
            res.add(nums[i]);
        }        
        
        for (int i = 0; i < n; i++) {
            int curNum = nums[i];
            res.set(curNum - 1, curNum);
        }
        
        // remove from the back
        for (int i = n - 1; i >= 0; i--) {
            if (res.get(i) == i + 1) {
                res.remove(i);
            } else {
                res.set(i, i + 1);
            }
        }
        
        return res;
    }
```

另外，看了solution，发现了另外一种做法，是修改输入array的做法。同样在下标做文章，从左到右把数组过一遍，遇到数字，我们就用abs（nums\[i]）-1算出下标的数，得到这个数字在排序的情况下的位置，然后把那个数字变成负数。判断的时候，用abs是因为我们可能把数组靠后的数设成负数了。最后过一遍判断时，我们就只要判断是否为证书就ok了，还是正数的那些格子就是missing的数字。

解法2：

```java
public List<Integer> findDisappearedNumbers(int[] nums) {
        
        // Iterate over each of the elements in the original array
        for (int i = 0; i < nums.length; i++) {
            
            // Treat the value as the new index
            int newIndex = Math.abs(nums[i]) - 1;
            
            // Check the magnitude of value at this new index
            // If the magnitude is positive, make it negative 
            // thus indicating that the number nums[i] has 
            // appeared or has been visited.
            if (nums[newIndex] > 0) {
                nums[newIndex] *= -1;
            }
        }
        
        // Response array that would contain the missing numbers
        List<Integer> result = new LinkedList<Integer>();
        
        // Iterate over the numbers from 1 to N and add all those
        // that have positive magnitude in the array
        for (int i = 1; i <= nums.length; i++) {
            
            if (nums[i - 1] > 0) {
                result.add(i);
            }
        }
        
        return result;
    }
```
