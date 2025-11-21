# L49 Sort Letters by Case

Given a string which contains only letters. Sort it by lower case first and upper case second.

## Notice

It's \_NOT \_necessary to keep the original order of lower-case letters and upper case letters.

**Example**

For`"abAcD"`, a reasonable answer is`"acbAD"`

[**Challenge**](http://www.lintcode.com/en/problem/sort-letters-by-case/#challenge)

Do it in one-pass and in-place.

```java
public void sortLetters(char[] chars) {
    if (chars == null || chars.length == 0) {
        return;
    }

    int left = 0;
    int right = chars.length - 1;
    while (left <= right) {
        while (left <= right && Character.isLowerCase(chars[left])) {
            left++;
        }

        while (left <= right && Character.isUpperCase(chars[right])) {
            right--;
        }

        if (left <= right) {
            char tmp = chars[left];
            chars[left] = chars[right];
            chars[right] = tmp;
            left++;
            right--;
        }
    }
}
```
