# 68 Text Justification

Given an array of words and a lengthL, format the text such that each line has exactlyLcharacters and is fully (left and right) justified.

You should pack your words in a greedy approach; that is, pack as many words as you can in each line. Pad extra spaces`' '`when necessary so that each line has exactlyLcharacters.

Extra spaces between words should be distributed as evenly as possible. If the number of spaces on a line do not divide evenly between words, the empty slots on the left will be assigned more spaces than the slots on the right.

For the last line of text, it should be left justified and no extra space is inserted between words.

For example,\
**words**:`["This", "is", "an", "example", "of", "text", "justification."]`\
**L**:`16`.

Return the formatted lines as:

```
[
   "This    is    an",
   "example  of text",
   "justification.  "
]
```

**Note:**&#x45;ach word is guaranteed not to exceedLin length.

[click to show corner cases.](https://leetcode.com/problems/text-justification/)

**Corner Cases:**

*   A line other than the last line might contain only one word. What should you do in this case?

    In this case, that line should be left-justified.

二刷决定直接看人家的答案了，这题就是考代码能力。每次用index跟last把一行能放的词找出来。

```java
public List<String> fullJustify(String[] words, int maxWidth) {
    List<String> res = new ArrayList<>();
    if (words == null || words.length == 0) {
        return res;
    }

    int index = 0;        
    while (index < words.length) {
        // 找这一行能fit in的词
        int last = index + 1; // last指着下一个词
        int count = words[index].length();
        while (last < words.length) { // check一下是否合法
            if (count + 1 + words[last].length() > maxWidth) {// 如果这个词加上1（空格）加上之前的一段大于maxWidth，就跳出来
                break;
            }
            count += words[last].length() + 1;// 不然就加上，然后继续数
            last++;
        }

        StringBuilder oneLine = new StringBuilder();
        int emptySlots = last - index - 1;// 这里算的是一行里有多少个空的地方要填，例如3个词中间会有2个格子
        if (emptySlots == 0 || last == words.length) { // 如果没空格一个词占一行，或者最后一行的话，左对齐
            // left justify
            for (int i = index; i < last; i++) { // 把所有词加上+空格
                oneLine.append(words[i] + " ");    
            }
            oneLine.deleteCharAt(oneLine.length() - 1);// 把最后多加的空格去掉
            for (int i = oneLine.length(); i < maxWidth; i++) {// 然后再用空格填到maxwidth
                oneLine.append(" ");    
            }                
        } else {
            // middle justify
            int spaces = (maxWidth - count) / emptySlots; //这里是算每一个格子会有多少空格
            int extra = (maxWidth - count) % emptySlots; // 这里算的是前几个格子要多填一个空格
            for (int i = index; i < last; i++) { // 开始把单词填进去
                oneLine.append(words[i]); 
                if (i < last - 1) { // 除了最后一个以外都得填空格
                    for (int j = 0; j <= spaces; j++) { // 先把原有的填进去；记得要加等于，因为count里面其实已经多算了一个空格
                        oneLine.append(" ");                            
                    }                    
                    if (i - index < extra) { // 前几个要多填一个空格
                        oneLine.append(" ");    
                    }
                }
            }
        }
        res.add(oneLine.toString());// 收集好一行就加到结果里
        index = last; // 把下标向后移进入下一行
    }

    return res;
}
```

写完花了两天调通，这题的corner case调得各种崩溃。我把这题分了两个函数写，第一个是把该放进一行里的词找出来。然后传进第二个函数，构造出一行。其中发现第一个函数，用还剩多少字符来做比算现在有多少好点，但最tricky的是控制end的下标。因为有的时候我们要后退一个，有时候不用。这个取决于while循环里的两个条件中的哪个导致循环退出。两个条件会交替出现，所以光判断i <= n 是不够的。当剩下空间不足下一个词+空格（或剩下的空间==下一个词的长度）的时候，如果曾经进入循环，我们要减一。（这时i < n)；如果没进入过循环，end就不用减一。这里调了半天。关于第二个函数，这里主要注意的是，空格得尽量平均分配，所以%和除以不行，这里我用了一列stringbuilder array。循环加空格进去的时候记得判断如果space已经没了的话，我们没到最后也跳出循环。还有一个条件是题目没有明确说的，就是如果有一个单词太大，放不进怎么办。我流了一个位置throw exception。感觉要是面试考到这题死定了。

```java
public List<String> fullJustify(String[] words, int maxWidth) {
    List<String> res = new ArrayList<>();
    if (words == null || words.length == 0 || maxWidth < 1) {
        res.add("");
        return res;
    }

    int n = words.length;
    int i = 0;

    while (i < n) { // count how many words in one line
        if (words[i].length() > maxWidth) {
            // throw exception "line width too small to fit this word"
            break;
        } else {
            int start = i;
            int end = i;
            int left = maxWidth;
            boolean extrai = false;

            while (i < n && left > words[i].length()) {
                if (i != start) {// count space
                    left--;
                }
                if (left <= 0) {
                    break;
                }
                left -= words[i++].length();
                extrai = true;

            }

            if (i <= n && extrai) {
                end = i - 1;
            } else {
                end = i;
                i++;
            }

            // once we found what words need to go in this line, we make a line
            res.add(justify(start, end, words, i == n, maxWidth));
        }
    }

    return res;
}

private String justify(int start, int end, String[] words, boolean finished, int maxWidth) {
    StringBuilder sb = new StringBuilder();
    if (start == end) {
        sb.append(words[start]);
        int space = maxWidth - words[start].length();
        while (space > 0) {
            sb.append(" ");
            space--;
        }
    } else {
        int charCnt = 0;
        for (int i = start; i <= end; i++) {
            charCnt += words[i].length();
        }
        int space = maxWidth - charCnt;

        if (finished) {
            for (int i = start; i <= end; i++) {
                sb.append(words[i]);
                sb.append(" ");
                space--;
            }

            while (space > 0) {
                sb.append(" ");
                space--;
            }
        } else {
            int wordNum = end - start + 1;
            StringBuilder[] sbs = new StringBuilder[wordNum - 1];

            for (int i = 0; i < sbs.length; i++) {
                sbs[i] = new StringBuilder();
            }
            while (space > 0) {
                for (int s = 0; s < sbs.length && space > 0; s++) {
                    sbs[s].append(" ");
                    space--;
                }
            }

            for (int i = start; i <= end; i++) {
                sb.append(words[i]);
                if (i != end) {
                    sb.append(sbs[i - start]);
                }
            }
        }
    }

    return sb.toString();
}
```
