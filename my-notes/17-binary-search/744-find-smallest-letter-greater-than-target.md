# 744 Find Smallest Letter Greater Than Target

Given a list of sorted characters `letters` containing only lowercase letters, and given a target letter `target`, find the smallest element in the list that is larger than the given target.

Letters also wrap around. For example, if the target is `target = 'z'` and `letters = ['a', 'b']`, the answer is `'a'`.

**Examples:**

```
Input:
letters = ["c", "f", "j"]
target = "a"
Output: "c"

Input:
letters = ["c", "f", "j"]
target = "c"
Output: "f"

Input:
letters = ["c", "f", "j"]
target = "d"
Output: "f"

Input:
letters = ["c", "f", "j"]
target = "g"
Output: "j"

Input:
letters = ["c", "f", "j"]
target = "j"
Output: "c"

Input:
letters = ["c", "f", "j"]
target = "k"
Output: "c"
```

**Note:**

1. `letters` has a length in range `[2, 10000]`.
2. `letters` consists of lowercase letters, and contains at least 2 unique letters.
3. `target` is a lowercase letter.

```java
public char nextGreatestLetter(char[] letters, char target) {
    if (letters == null || letters.length == 0) {
        return target;
    }

    int loc = findLoc(letters, target);

    if (loc >= letters.length) {
        return letters[0];
    } else {
        return letters[loc];
    }        
}

private int findLoc(char[] nums, char target) {
    int start = 0;
    int end = nums.length - 1;

    while (start + 1 < end) {
        int mid = start + (end - start) / 2;
        if (nums[mid] > target) {
            end = mid;
        } else {
            start = mid;
        }
    }

    if (nums[start] > target) {
        return start;
    } else if (nums[end] > target) {
        return end;
    } else {
        return end + 1;
    }
}
```
