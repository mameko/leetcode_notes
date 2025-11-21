# 211 Add and Search Word - Data structure Design

esign a data structure that supports the following two operations:

```
void addWord(word)
bool search(word)
```

search(word) can search a literal word or a regular expression string containing only letters`a-z`or`.`. A`.`means it can represent any one letter.

For example:

```
addWord("bad")
addWord("dad")
addWord("mad")
search("pad") -> false
search("bad") -> true
search(".ad") -> true
search("b..") -> true
```

**Note:**\
You may assume that all words are consist of lowercase letters`a-z`.

这题用trie，然后search时一定要递归，试了好久都没试出非递归版本。

```java
public class WordDictionary {
    private TrieNode root;
    /** Initialize your data structure here. */
    public WordDictionary() {
        root = new TrieNode();
    }

    /** Adds a word into the data structure. */
    public void addWord(String word) {
        if (word == null || word.isEmpty()) {
            return;
        }

        TrieNode last = root;
        for (int i = 0; i < word.length(); i++) {
            TrieNode cur = last.children.get(word.charAt(i));
            if (cur == null) {
                cur = new TrieNode();
                last.children.put(word.charAt(i), cur);
            }
            last = cur;
        }

        last.end = true;
    }

    /** Returns if the word is in the data structure. A word could contain the dot character '.' to represent any one letter. */
    public boolean search(String word) {
        return search(word, root, 0);
    }

    private boolean search(String word, TrieNode node, int start) {
        if (start == word.length()) {
            return node.end;
        }

        char c = word.charAt(start);
        if (c == '.') {
            for (TrieNode candidate : node.children.values()) {
                if (search(word, candidate, start + 1)) {
                    return true;
                }
            }
            return false;
        } else {
            TrieNode next = node.children.get(c);
            if (next == null) {
                return false;
            } else {
                return search(word, next, start + 1);
            }
        }
    }

    class TrieNode {
        Map<Character, TrieNode> children = new HashMap<>();
        boolean end;
    }
}


/**
 * Your WordDictionary object will be instantiated and called as such:
 * WordDictionary obj = new WordDictionary();
 * obj.addWord(word);
 * boolean param_2 = obj.search(word);
 */
```
