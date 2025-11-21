# 395 Longest Substring with At Least K Repeating Characters

Find the length of the longest substring **T** of a given string (consists of lowercase letters only) such that every character in **T** appears no less than k times.

**Example 1:**

```
Input:
s = "aaabb", k = 3

Output:
3

The longest substring is "aaa", as 'a' is repeated 3 times.
```

**Example 2:**

```
Input:
s = "ababbc", k = 2

Output:
5

The longest substring is "ababb", as 'a' is repeated 2 times and 'b' is repeated 3 times.
```

这题用sliding window好难控制左边界，所以用递归来解决了。T: O（nlogn）要loop一次所有找不合法的位置n，然后每个不合法的会2分一下logn

```java
public int longestSubstring(String s, int k) {
        char[] cs = s.toCharArray();
        return helper(cs, 0, cs.length, k);
    }

    private int helper(char[] cs, int start, int end, int k) {
        if (end - start < k) {
            return 0;
        }

        int[] cnts = new int[26];
        for (int i = start; i < end; i++) {
            int index = cs[i] - 'a';
            cnts[index]++;
        }

        for (int i = 0; i < 26; i++) {
            if (cnts[i] <= 0 || cnts[i] >= k) {
            // <= 0 are the chars that is not in here, bigger than k, valid, so don't need to deal with 
                continue;
            }
            // if we found invalid char, find the invalid char location j
            // then recur to find max in left & right side
            for (int j = start; j < end; j++) {
                if (cs[j] == i + 'a') {
                    int left = helper(cs, start, j, k);
                    int right = helper(cs, j + 1, end, k);
                    return Math.max(left, right);
                }
            }
        }

        // if we didn't return in the inner for, all chars are >= k
        return end - start;
    }
```
