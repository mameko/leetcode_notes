# L559 Trie Service

Build tries from a list ofpairs. Save top 10 for each node.

**Example**

Given a list of

```
<"abc", 2>
<"ac", 4>
<"ab", 9>
```

Return`<a[9,4,2]<b[9,2]<c[2]<>>c[4]<>>>`, and denote the following tree structure:

```
         Root
         / 
       a(9,4,2)
      /    \
    b(9,2) c(4)
   /
 c(2)
```

这里只用建trie树用的循环跟i[Iplement Trie](l442-implement-trie.md)一样。

```java
public class TrieService {

    private TrieNode root = null;

    public TrieService() {
        root = new TrieNode();
    }

    public TrieNode getRoot() {
        // Return root of trie root, and 
        // lintcode will print the tree struct.
        return root;
    }

    // @param word a string
    // @param frequency an integer
    public void insert(String word, int frequency) {
        if (word == null || word.isEmpty()) {
            return;
        }

        TrieNode last = root;
        for (int i = 0; i < word.length(); i++) {
            Character c = word.charAt(i);
            TrieNode child = last.children.get(c);
            if (child == null) {
                child = new TrieNode();
                last.children.put(c, child);
            }

            List<Integer> top10 = child.top10;
            top10.add(frequency);
            Collections.sort(top10, Collections.reverseOrder());
            if (top10.size() > 10) {
                top10.remove(top10.size() - 1);
            }

            last = child;
        }
    }
 }
```
