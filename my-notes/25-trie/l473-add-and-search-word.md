# L473 Add and Search Word

Design a data structure that supports the following two operations:`addWord(word)`and`search(word)`

`search(word)`can search a literal word or a regular expression string containing only letters`a-z`or`.`.

A`.`means it can represent any one letter.

## Notice

You may assume that all words are consist of lowercase letters a-z.

**Example**

```
addWord("bad")
addWord("dad")
addWord("mad")
search("pad")  // return false
search("bad")  // return true
search(".ad")  // return true
search("b..")  // return true
```

感觉是很直接的变相考Trie的实现。只是查询的时候如果遇到"."的话，要全部找一遍。这里要用递归dfs来搜。

```java
public class WordDictionary {
    TrieNode root;

    public WordDictionary() {
       root = new TrieNode();
    }

    // Adds a word into the data structure.
    public void addWord(String word) {
        root.addWord(word);
    }

    // Returns if the word is in the data structure. A word could
    // contain the dot character '.' to represent any one letter.
    public boolean search(String word) {
        return searchR(word, root, 0);
    }

    private boolean searchR(String word, TrieNode node, int index) {
        if (word.length() == index) {
            return node.isEnd;
        }

        Character cur = word.charAt(index);
        if (cur == '.') {
            for (Map.Entry<Character, TrieNode> entry : node.children.entrySet()) {
                if (searchR(word, entry.getValue() ,index + 1)) {
                    return true;
                }
            }

            return false;
        } else {
            TrieNode nextNode = node.children.get(cur);
            if (nextNode == null) {
                return false;
            } else {
                return searchR(word, nextNode, index + 1);
            }
        }
    }
}

// Your WordDictionary object will be instantiated and called as such:
// WordDictionary wordDictionary = new WordDictionary();
// wordDictionary.addWord("word");
// wordDictionary.search("pattern");

class TrieNode {
        public HashMap<Character, TrieNode> children;
        public boolean isEnd;

        public TrieNode() {
            children = new HashMap<>();
            isEnd = false;
        }

        public void addWord(String word) {
            if (word == null || word.isEmpty()) {
                return;
            }
            Character first = word.charAt(0);

            TrieNode child = children.get(first);
            if (child == null) {
                child = new TrieNode();
                children.put(first, child);
            }

            if (word.length() > 1) {
                child.addWord(word.substring(1));
            } else {
                child.isEnd = true;
            }
        }
    }
```
