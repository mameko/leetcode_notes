# 187 Repeated DNA Sequences

All DNA is composed of a series of nucleotides abbreviated as A, C, G, and T, for example: "ACGAATTCCG". When studying DNA, it is sometimes useful to identify repeated sequences within the DNA.

Write a function to find all the 10-letter-long sequences (substrings) that occur more than once in a DNA molecule.

For example,

```
Given s = "AAAAACCCCCAAAAACCCCCCAAAAAGGGTTT",

Return:
["AAAAACCCCC", "CCCCCAAAAA"].
```

这题一开始只想出了暴力的方法，要O(n^2)的时间和空间。后来看到一种rolling hash的解法，可以O(n)搞定。（待补代码）

```java
   public List<String> findRepeatedDnaSequences(String s) {
        List<String> result = new ArrayList<>();
        if (s == null || s.length() == 0) {
            return result;
        }

        HashMap<String, Integer> checkDup = new HashMap<>();
        for (int i = 0; i < s.length() - 10 + 1; i++) {
            String subString = s.substring(i, i + 10);
            if (checkDup.containsKey(subString)) {
                int times = checkDup.get(subString);
                if (times == 1) {
                    result.add(subString);
                }
                checkDup.put(subString, times + 1);
            } else {
                checkDup.put(subString, 1);
            }
        }

        return result;
    }
```
