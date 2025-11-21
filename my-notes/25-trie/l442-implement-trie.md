# L442 Implement Trie

Implement a trie with insert, search, and startsWith methods.

## Notice

You may assume that all inputs are consist of lowercase letters a-z.

**Example**

```
insert("lintcode")
search("code")
>>> false
startsWith("lint")
>>> true
startsWith("linterror")
>>> false
insert("linterror")
search("lintcode)
>>> true
startsWith("linterror")
>>> true
```

经典实现，阅读并背诵全文。

```java
class TrieNode {
    // Initialize your data structure here.
    HashMap<Character, TrieNode> children;
    boolean end;
    public TrieNode() {
        children = new HashMap<>();
    }
}

public class Trie {
    private TrieNode root;

    public Trie() {
        root = new TrieNode();
    }

    // Inserts a word into the trie.
    public void insert(String word) {

        if (word == null || word.isEmpty()) {
            return;
        }

        TrieNode last = root;
        for (int i = 0; i < word.length(); i++) {
            TrieNode c = last.children.get(word.charAt(i));
            if (c == null) {
                c = new TrieNode();
                last.children.put(word.charAt(i), c);
            }
            last = c;
        }
        last.end = true;
    }

    // Returns if the word is in the trie.
    public boolean search(String word) {
        if (word == null || word.isEmpty()) {
            return true;
        }

        TrieNode last = root;
        for (int i = 0; i < word.length(); i++) {
            TrieNode c = last.children.get(word.charAt(i));
            if (c == null) {
                return false;
            }
            last = c;
        }

        return last.end;
    }

    // Returns if there is any word in the trie
    // that starts with the given prefix.
    public boolean startsWith(String prefix) {
        if (prefix == null || prefix.isEmpty()) {
            return true;
        }

        TrieNode last = root;
        for (int i = 0; i < prefix.length(); i++) {
            TrieNode c = last.children.get(prefix.charAt(i));
            if (c == null) {
                return false;
            }
            last = c;
        }

        return true;
    }
}
```

下面是cc189的实现, 递归版在[add and search word](../28-search/211-add-and-search-word-data-structure-design.md)里要用到。

```java
public class Trie {
    private TrieNode root;

    public Trie(ArrayList<String> list) {
        root = new TrieNode();
        for (String word : list) {
            root.addWord(word);
        }
    }

    public Trie(String[] list) {
        root = new TrieNode();
        for (String word : list) {
            root.addWord(word);
        }
    }

    public boolean contains(String prefix, boolean exact) {
        TrieNode lastNode = root;
        int i = 0;
        for (i = 0; i < prefix.length(); i++) {
            lastNode = lastNode.getChild(prefix.charAt(i));
            if (lastNode == null) {
                return false;
            }
        }

        return !exact || lastNode.terminates();
    }

    public boolean contains(String Prefix) {
        return contains(prefix, false);
    }

    public TrieNode getRoot() {
        return root;
    }
}

class TrieNode {
    private HashMap<Characer, TrieNode> children;
    private boolean terminates = false;

    private char character;// you can store other things here, say in typeahead, you can store top k words

    public TrieNode() {
        children = new HashMap<Character, TrieNode>();
    }

    public TrieNode() {
        this();
        this.character = character;
    }

    public char getChar() {
        return character;
    }

    public void addWord(String word) {
        if (word == null || word.isEmpty()) {
            return;
        }

        char firstChar = word.charAt(0);

        TrieNode child = getChild(firstChar);
        if (child == null) {
            child = new TrieNode(firstChar);
            children.put(firstChar, child);
        }

        if (word.length() > 1) {
            child.addWord(word.substring(1));
        } else {
            child.setTerminates(true);
        }

        public TrieNode getChild(char c) {
            return children.get(c);
        }

        public boolean terminates() {
            return terminates;
        }

        public void setTerminates(boolean t) {
            terminates = t;
        }
    }
}
```
