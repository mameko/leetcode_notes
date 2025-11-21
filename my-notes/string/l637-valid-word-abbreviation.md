# L637 Valid Word Abbreviation

## L637 Valid Word Abbreviation (408)

## Description

Given a **non-empty** string`word`and an abbreviation`abbr`, return whether the string matches with the given abbreviation.

A string such as`"word"`contains only the following valid abbreviations:

```
["word", "1ord", "w1rd", "wo1d", "wor1", "2rd", "w2d", "wo2", "1o1d", "1or1", "w1r1", "1o2", "2r1", "3d", "w3", "4"]
```

Notice that only the above abbreviations are valid abbreviations of the string`word`. Any other string is not a valid abbreviation of`word`

## Example

Example 1:

```
Given s = "internationalization", abbr = "i12iz4n":
Return true.
```

Example 2:

```
Given s = "apple", abbr = "a2e":
Return false.
```

这题用两个指针，边循环边做。虽然简单但中间有很多需要注意的地方。例如，在while里要记得判断是否越界，因为如果数字太大，然后s的下标一加就越界了。另外，要注意没有前导0的情况，就是05 ！= 5. 最后return时还得注意判断两个string的下标是否都到了最后。不到的话，证明不是perfect match不是abbr。

```java
    /**
     * @param word: a non-empty string
     * @param abbr: an abbreviation
     * @return: true if string matches with the given abbr or false
     */
    public boolean validWordAbbreviation(String word, String abbr) {
        // write your code here
        if (abbr == null) {
            return false;
        }

        int i = 0;
        int j = 0;
        char[] wa = word.toCharArray();
        char[] aa = abbr.toCharArray();
        while (i < wa.length && j < aa.length) {
            if (Character.isDigit(aa[j])) {
                int adv = 0;
                if (aa[j] == '0') {
                    return false;
                }
                while (j < aa.length && Character.isDigit(aa[j])) {
                    adv = adv * 10 + aa[j] - '0';
                    j++;
                }
                i = i + adv;
            } else {
                if (wa[i] != aa[j]) { 
                    return false;
                }
                i++;
                j++;
            }
        }

        return i == wa.length && j == aa.length;
    }
```

下面是九章的in place版本：

```java
    public boolean validWordAbbreviation(String word, String abbr) {
        int i = 0, j = 0;
        while (i < word.length() && j < abbr.length()) {
            if (word.charAt(i) == abbr.charAt(j)) {
                i++;
                j++;
            } else if ((abbr.charAt(j) > '0') && (abbr.charAt(j) <= '9')) {     //notice that 0 cannot be included
                int start = j;
                while (j < abbr.length() && Character.isDigit(abbr.charAt(j))) {
                    j++;
                }
                i += Integer.valueOf(abbr.substring(start, j));
            } else {
                return false;
            }
        }
        return (i == word.length()) && (j == abbr.length());
    }
```
