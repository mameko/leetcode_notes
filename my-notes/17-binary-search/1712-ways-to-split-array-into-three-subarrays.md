# 1712 Ways to Split Array Into Three Subarrays

A split of an integer array is **good** if:

* The array is split into three **non-empty** contiguous subarrays - named `left`, `mid`, `right` respectively from left to right.
* The sum of the elements in `left` is less than or equal to the sum of the elements in `mid`, and the sum of the elements in `mid` is less than or equal to the sum of the elements in `right`.

Given `nums`, an array of **non-negative** integers, return _the number of **good** ways to split_ `nums`. As the number may be too large, return it **modulo** `109 + 7`.

**Example 1:**

```
Input: nums = [1,1,1]
Output: 1
Explanation: The only good way to split nums is [1] [1] [1].
```

**Example 2:**

```
Input: nums = [1,2,2,2,5,0]
Output: 3
Explanation: There are three good ways of splitting nums:
[1] [2] [2,2,5,0]
[1] [2,2] [2,5,0]
[1,2] [2,2] [5,0]
```

**Example 3:**

```
Input: nums = [3,2,1]
Output: 0
Explanation: There is no good way to split nums.
```

**Constraints:**

* `3 <= nums.length <= 105`
* `0 <= nums[i] <= 104`

嘛，這題，跟著提示大体想出了做法。但特么的二分分了两个星期还没分对。其实难点是，我们二分找的范围和找到的是什么。一开始，没想清楚二分的条件，以为是leftSum <= midSum <= rightSum。所以很多corner case逃不掉，以为如果不能切的地方我们也要同时判断，所以很麻烦。最后看了答案，发现只要简单数学转换一下，就知道，我们要找的是min i >= leftSum和max i <= remain/2。然后把range规定好就能套九章模板了。

这里，思路是这样的。基本上我们要在数组里切两刀。先把问题缩小，我们定好左边一刀切哪里。然后找右边那一刀的可切范围。譬如，\[1, 2, 2, 2, 5, 0]，如果我们第一刀且在1后面，那么，我们第二刀的范围是第一个2后面，或者第 二个2后面。这里我们就有2个解。因为是正数，所以我们在搜范围的时候能运用二分+prefixSum

首先，我们一个一个loop第一刀。因为每一节都要有数字，所以最后loop到n - 2。因为我们起码要三段等于，所以如果切完第一刀，发现后面不能切成两段，那么我们就知道这个误解，break掉。然后，二分找第二刀的min和max。算个和就ok了。T:O(nlogn) S:O(n)

```java
	public int waysToSplit(int[] nums) {
		if (nums == null || nums.length < 3) {
			return 0;
		}

        int MOD = 1000000007;
		int n = nums.length;
		// calculate prefixSum, so we can get range sum easily
		int[] prefixSum = new int[n + 1];
		for (int i = 1; i <= n; i++) {
			prefixSum[i] = prefixSum[i - 1] + nums[i - 1];
		}

		long total = 0;
		for (int i = 0; i < n - 2; i++) {
            int leftSum = prefixSum[i + 1];
            int remain = prefixSum[n] - leftSum;
            if (remain < leftSum * 2) {
                break;
            }
            
            int first = findLeft(i, n, leftSum, prefixSum);
            int last = findLast(i, n, remain / 2, prefixSum);
            
            total += Math.max(last - first + 1, 0);		
		}

		return (int) (total % MOD);
	}
    
    // find fist i that is >= leftSum
    private int findLeft(int i, int n, int target, int[] prefixSum) {
        int left = i + 1;
        int right = n - 2;
        while (left + 1 < right) {
            int mid = left + (right - left) / 2;
            int cur = prefixSum[mid + 1] - prefixSum[i + 1];
            if (cur >= target) {
                right = mid;            
            } else {
                left = mid;
            }
        }
        
        if (prefixSum[left + 1] - prefixSum[i + 1] >= target) {
            return left;
        } else {
          return right;  
        }
    }

    // find last i that is <= remain/2
    private int findLast(int i, int n, int target, int[] prefixSum) {
        int left = i + 1;
        int right = n - 2;
        while (left + 1 < right) {
            int mid = left + (right - left) / 2;
            int cur = prefixSum[mid + 1] - prefixSum[i + 1];
            if (cur <= target) {
                left = mid;
            } else {
                right = mid;
            }
        }
        
        if (prefixSum[right + 1] - prefixSum[i + 1] <= target) {
            return right;
        } else {
          return left;  
        }
    }
```
