# 819 Most Common Word

Given a paragraph and a list of banned words, return the most frequent word that is not in the list of banned words. It is guaranteed there is at least one word that isn't banned, and that the answer is unique.

Words in the list of banned words are given in lowercase, and free of punctuation. Words in the paragraph are not case sensitive. The answer is in lowercase.

**Example:**

```
Input: 
paragraph = "Bob hit a ball, the hit BALL flew far after it was hit."
banned = ["hit"]
Output: "ball"

Explanation: 
"hit" occurs 3 times, but it is a banned word.
"ball" occurs twice (and no other word does), so it is the most frequent non-banned word in the paragraph. 
Note that words in the paragraph are not case sensitive,
that punctuation is ignored (even if adjacent to words, such as "ball,"), 
and that "hit" isn't the answer even though it occurs more because it is banned.
```

**Note:**

* `1 <= paragraph.length <= 1000`.
* `1 <= banned.length <= 100`.
* `1 <= banned[i].length <= 10`.
* The answer is unique, and written in lowercase (even if its occurrences in`paragraph` may have uppercase symbols, and even if it is a proper noun.)
* `paragraph`only consists of letters, spaces, or the punctuation symbols`!?',;.`
* There are no hyphens or hyphenated words.
* Words only consist of letters, never apostrophes or other punctuation symbols.

这题一看，不就是split然后数吗？后来发现split时的regex其实有点难度。而且这样做比较慢。

```java
public String mostCommonWord(String paragraph, String[] banned) {
    if (paragraph == null || paragraph.length() == 0 || banned == null) {
        return "";
    }

    String[] words = paragraph.split("!|\\?|\\'|,|;|\\.|\\s");
    Map<String, Integer> cnt = new HashMap<>();
    for (String word : words) {
        if (word.isEmpty()) {
            continue;
        }
        StringBuilder sb = new StringBuilder();
        for (int i = 0; i < word.length(); i++) {
            if (Character.isAlphabetic(word.charAt(i))) {
                sb.append(word.charAt(i));
            }
        }
        String key = sb.toString().toLowerCase();
        cnt.put(key, cnt.getOrDefault(key, 0) + 1);
    }

    for (String ban : banned) {
        cnt.remove(ban);
    }

    int max = 0;
    String res = "";
    for (Map.Entry<String, Integer> entry : cnt.entrySet()) {
        if (max < entry.getValue()) {
            max = entry.getValue();
            res = entry.getKey();
        }
    }

    return res;
}
```

正确的打开方式是一边看那些标点一边处理。自己没写，把答案先抄上来参考，有空再补。T\&S都是O(P + B) paragraph+banned的长度。

```java
public String mostCommonWord(String paragraph, String[] banned) {
    paragraph += ".";

    Set<String> banset = new HashSet();
    for (String word: banned) banset.add(word);
    Map<String, Integer> count = new HashMap();

    String ans = "";
    int ansfreq = 0;

    StringBuilder word = new StringBuilder();
    for (char c: paragraph.toCharArray()) {
        if (Character.isLetter(c)) {
            word.append(Character.toLowerCase(c));
        } else if (word.length() > 0) {
            String finalword = word.toString();
            if (!banset.contains(finalword)) {
                count.put(finalword, count.getOrDefault(finalword, 0) + 1);
                if (count.get(finalword) > ansfreq) {
                    ans = finalword;
                    ansfreq = count.get(finalword);
                }
            }
            word = new StringBuilder();
        }
    }

    return ans;
}
```
