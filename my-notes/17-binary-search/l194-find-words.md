# L194 Find Words

Description

Given a string `str` and a dictionary `dict`, you need to find out which words in the dictionary are subsequences of the string and return those words.The order of the words returned should be the same as the order in the dictionary.

***

Wechat reply 【Two Sigma】 get the latest requent Interview questions. (wechat id : **jiuzhang1104**)

1. |str|<=1000
2. the sum of all words length in dictionary<=1000

(All characters are in lowercase)

Example

Example 1:

```
Input:
str="bcogtadsjofisdhklasdj"
dict=["book","code","tag"]Output:["book"]
Explanation:Only book is a subsequence of str
```

Example 2:

```
Input:
str="nmownhiterer"
dict=["nowhere","monitor","moniter"]
Output:["nowhere","moniter"]
```

Challenge

|str|<=100000

这题看完题就基本想出了二分的解法，所以放在二分里了。一开始就把str的字符所在的位置都存起来。然后当我们要判断是否是合法subsequence的时候，从头开始，一个个找下去。譬如，第一个字母出现在，\[1，4，5]这几个位置，那么我们就取第一个“1”，然后找第二个字母，譬如，出现在\[2, 3, 6]，那么我们取第一个符合条件的，“2”。如此类推，如果能reach end，那么这个词语就能加到返回的集合里了。因为所有dict里的都要过了一遍，m为dict里所有词语加起来的长度，所以T：O（m \* logn)，最坏情况就是，str=“aaaaa"，dict要找的也是，“aaaaa",我们的map里存的是\[a, (0, 1, 2, 3, 4)],每次都要二分这个n。S：O（n）所有位置。

另外，看了题解，还有一种美其名曰“序列自动机”的解法，感觉有点像用dp来建表，然后查询的时候做到O(1)来减少复杂度。因为我们只有26个字母，所以我们从后向前遍历str，记录当前位置后面第一个abcde....z的位置。然后我们check subsequence的时候，就直接查表判断，不用二分重复找。T：O（s \* n + m)，s是字符集的size，这里是26，建这个自动机表需要s \* n，然后每个dict里的词m都得来一遍。S: O（s \* n）自动机表的大小

```java

// 自己写了一遍
public List<String> findWords(String str, List<String> dict) {
    List<String> res = new ArrayList<>();
    if (str == null || str.length() == 0 || dict == null || dict.isEmpty()) {
        return res;
    }

    // 1. create table for next step
    // we only has 26 chars
    int[][] nextStep = findNextStep(str);

    // 2. loop and check subsequnces
    for (String word : dict) {
        if (isSubsequence(nextStep, word)) {
            res.add(word);
        }
    }

    return res;
}

boolean isSubsequence(int[][] nextStep, String word) {
    int indSoFar = 0; // start from beginning 
    char[] chars = word.toCharArray();
    int strLen = nextStep.length - 1;
    for (int i = 0; i < chars.length; i++) {
        int curCharInd = nextStep[indSoFar][chars[i] - 'a'];
        // 这里还是没搞懂为什么有没有等于都ok
        // 补充：其实第一个条件可以去掉
        // 虽然助教说这个是什么前一个，还有什么多开了一维，所有ok
        // 我觉得其实是因为第二个条件已经囊括了第一个条件
        if (indSoFar > strLen || curCharInd < indSoFar) {
            return false;
        }
        indSoFar = curCharInd + 1;
    }

    return true;
}

private int[][] findNextStep(String str) {
   int len = str.length();
    // like a dp, we go from back to front        
    int[][] table = new int[len + 1][26];
    // create one more space for init, -1, becasue nothing after last char
    for (int j = 0; j < 26; j++) { // 其实只用初始化最后一行
        table[len][j] = -1;
    }

    for (int i = len - 1; i >= 0; i--) {
        for (int j = 0; j < 26; j++) {
            char curChar = str.charAt(i);
            int charInd = curChar - 'a';
            table[i][j] = table[i + 1][j];
            if (charInd == j) {
                table[i][j] = i;
            }
        }
    }

    return table;
}

// 某同学写的自动机，明天再自己写
public List<String> findWords(String str, List<String> dict) {
    // write your code here.
    List<String> ans = new ArrayList<>();
    if (str == null || str.length() == 0 || dict == null || dict.size() == 0) {
        return ans;
    }
    int n = str.length();
    int[][] nextChar = getNextChar(str);

    for (String cur : dict) {
        if (isValid(nextChar, cur)) {
            ans.add(cur);
        }
    }
    return ans;
}

boolean isValid(int[][] nextChar, String cur) {
    int n = nextChar.length - 1;
    int id = 0;
    for (char a : cur.toCharArray()) {
        if (id >= n || nextChar[id][a - 'a'] < id) {
            return false;
        }
        id = nextChar[id][a - 'a'] + 1;
    }

    return true;
}

int[][] getNextChar(String str) {
    int n = str.length();
    int[][] nextChar = new int[n + 1][26];

    for (int i = n; i >= 0; i --) {
        for (int j = 0; j < 26; j ++) {
            nextChar[i][j] = -1;
        }
    }

    for (int i = n - 1; i >= 0; i --) {
        int index = str.charAt(i) - 'a';
        for (int j = 0; j < 26; j ++) {
            nextChar[i][j] = nextChar[i + 1][j];
            if (index == j) {
                nextChar[i][j] = i;
            }
        }
    }

    return nextChar;
}

// 二分
public List<String> findWords(String str, List<String> dict) {
    List<String> res = new ArrayList<>();
    if (str == null || str.length() == 0 || dict == null || dict.isEmpty()) {
        return res;
    }

    Map<Character, List<Integer>> charToLocation = new HashMap<>();
    char[] chars = str.toCharArray();
    for (int i = 0; i < chars.length; i++) {
        List<Integer> tmp;
        if (charToLocation.containsKey(chars[i])) {
            tmp = charToLocation.get(chars[i]);
        } else {
            tmp = new ArrayList<Integer>();
        }
        tmp.add(i);
        charToLocation.put(chars[i], tmp);
    }

    for (String word : dict) {
        if (isSubsequence(word, charToLocation)) {
            res.add(word);
        }
    }

    return res;
}

private boolean isSubsequence(String word, Map<Character, List<Integer>> map) {
    char[] chars = word.toCharArray();                        
    int ind = -1;
    for (int i = 0; i < chars.length; i++) {
        char c = chars[i];
        List<Integer> locs = map.get(c);
        ind = binarySearchForFirstBiggerOrEqual(locs, ind); 
        if (ind == -1)  {
            return false;
        }
    }

    return true;
}

private int binarySearchForFirstBiggerOrEqual(List<Integer> locs, int target) {
    if (locs == null || locs.isEmpty()) {
        return -1;
    }

    int start = 0;
    int end = locs.size() - 1;
    while (start + 1 < end) {
        int mid = start + (end - start) / 2; 
        if (locs.get(mid) == target) {
            start = mid;
        } else if (locs.get(mid) < target) {
            start = mid;
        } else {
            end = mid;
        }
    }

    if (locs.get(start) > target) {
        return locs.get(start);
    }

    if (locs.get(end) > target) {
        return locs.get(end);
    } 

    return -1;
}
```

