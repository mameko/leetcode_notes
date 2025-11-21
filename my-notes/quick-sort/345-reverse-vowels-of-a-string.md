# 345 Reverse Vowels of a String

Write a function that takes a string as input and reverse only the vowels of a string.

**Example 1:**\
Given s = "hello", return "holle".

**Example 2:**\
Given s = "leetcode", return "leotcede".

**Note:**\
The vowels does not include the letter "y".

在写一次，发现，其实<和<=都行，还是按照模板来吧。然后再换一个高级点的java code。

```java
public String reverseVowels(String s) {
    if (s == null || s.length() == 0) {
        return s;
    }

    // 快速把array建起来，然后new一个set，不用一个一个地add
    Set<Character> vowels = new HashSet<>(Arrays.asList('a', 'e', 'i', 'o', 'u', 'A', 'E', 'I', 'O', 'U'));
    char[] cs = s.toCharArray();

    int left = 0;
    int right = cs.length - 1;
    while (left <= right) {
        while (left <= right && !vowels.contains(cs[left])) {
            left++;
        }

        while (left <= right && !vowels.contains(cs[right])) {
            right--;
        }

        if (left <= right) {
            char tmp = cs[left];
            cs[left] = cs[right];
            cs[right] = tmp;

            left++;
            right--;
        }
    }

    return String.valueOf(cs);
}
```

第一个想到的方法是很不2pointer的：

```java
public String reverseVowels(String s) {
    if (s == null || s.length() == 0) {
        return s;
    }

    // vowels dictionary
    HashSet<Character> vowels = new HashSet<>();
    vowels.add('a');
    vowels.add('e');
    vowels.add('i');
    vowels.add('o');
    vowels.add('u');
    vowels.add('A');
    vowels.add('E');
    vowels.add('I');
    vowels.add('O');
    vowels.add('U');

    // location of vowels
    ArrayList<Integer> loc = new ArrayList<>();

    StringBuilder sb = new StringBuilder();
    char[] cs = s.toCharArray();
    for (int i = 0; i < cs.length; i++) {
        char c = cs[i];
        if (vowels.contains(c)) {
            sb.append(c);
            loc.add(i);
        }
    }

    // reverse vowels
    sb = sb.reverse();

    // put it back to original string
    for (int j = 0; j < loc.size(); j++) {
        cs[loc.get(j)] = sb.charAt(j);
    }

    return new String(cs);
}
```

然后看了答案发现可以用quick sort模板 =\_=...

```java
public String reverseVowels(String s) {
    if (s == null || s.length() == 0) {
        return s;
    }

    // vowels dictionary
    HashSet<Character> vowels = new HashSet<>();
    vowels.add('a');
    vowels.add('e');
    vowels.add('i');
    vowels.add('o');
    vowels.add('u');
    vowels.add('A');
    vowels.add('E');
    vowels.add('I');
    vowels.add('O');
    vowels.add('U');

    int i = 0;
    int j = s.length() - 1;
    char[] cs = s.toCharArray();
    while (i < j) {
        while (i < j && !vowels.contains(cs[i])) {
            i++;
        }

        while (i < j && !vowels.contains(cs[j])) {
            j--;
        }

        if (i < j) {
            char tmp = cs[i];
            cs[i] = cs[j];
            cs[j] = tmp;

            i++;
            j--;
        }
    }

    return new String(cs);
}
```
