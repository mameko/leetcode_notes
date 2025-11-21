# 17 Letter Combination of a Phone Number

Given a digit string, return all possible letter combinations that the number could represent.

A mapping of digit to letters (just like on the telephone buttons) is given below.

![](http://upload.wikimedia.org/wikipedia/commons/thumb/7/73/Telephone-keypad2.svg/200px-Telephone-keypad2.svg.png)

```
Input:
Digit string "23"

Output:
 ["ad", "ae", "af", "bd", "be", "bf", "cd", "ce", "cf"].
```

**Note:**\
Although the above answer is in lexicographical order, your answer could be in any order you want.

这题也跟前面些题很像，这次我们递归的结束条件是k （输入的数字数目）。然后每次loop的字母表有所不同，通过start来记录现在该从哪个字母表开始。每次递归下去的时候+1表示取下一个数字表示的字母表。T:O(4^input len), S:O(input len),递归深度是string的长度。每个输入字符最多有4种选择，所以4的长度次方。

```java
public ArrayList<String> letterCombinations(String digits) {
    ArrayList<String> res = new ArrayList<>();
    if (digits == null || digits.length() == 0) {
        return res;
    }

    // put mapping in hashmap
    HashMap<Character, String> hm = new HashMap<>();
    hm.put('2', "abc");
    hm.put('3', "def");
    hm.put('4', "ghi");
    hm.put('5', "jkl");
    hm.put('6', "mno");
    hm.put('7', "pqrs");
    hm.put('8', "tuv");
    hm.put('9', "wxyz");

    // dfs search
    StringBuilder tmp = new StringBuilder();
    dfsHelper(tmp, res, digits, 0, hm);

    return res;
}

private void dfsHelper(StringBuilder tmp, ArrayList<String> res, String s, 
                       int start, HashMap<Character, String> hm) {
    if (start == s.length()) {
        res.add(tmp.toString());
        return;
    }

    String curDig = hm.get(s.charAt(start));//取当前数字的字母表
    for (int i = 0; i < curDig.length(); i++) {
        tmp.append(curDig.charAt(i));
        dfsHelper(tmp, res, s, start + 1, hm);// 下次递归找下一个数字，所以start+1
        tmp.deleteCharAt(tmp.length() - 1);
    }
}
```
