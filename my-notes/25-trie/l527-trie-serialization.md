# L527 Trie Serialization

Serialize and deserialize a trie (prefix tree, search on internet for more details).

You can specify your own serialization algorithm, the online judge only cares about whether you can successfully deserialize the output from your own serialize function.

## Notice

You don't have to serialize like the test data, you can design your own format.

**Example**

```
str = serialize(old_trie)
>> str can be anything to represent a trie
new_trie = deserialize(str)
>> new_trie should have the same structure and values with old_trie
```

An example of test data: trie tree`<a<b<e<>>c<>d<f<>>>>`, denote the following structure:

```
     root
      /
     a
   / | \
  b  c  d
 /       \
e         f
```

九章答案：直接dfs，感觉比较简洁。

```java
/**
 * Definition of TrieNode:
 * public class TrieNode {
 *     public NavigableMap<Character, TrieNode> children;
 *     public TrieNode() {
 *         children = new TreeMap<Character, TrieNode>();
 *     }
 * }
 */
class Solution {
    /**
     * This method will be invoked first, you should design your own algorithm 
     * to serialize a trie which denote by a root node to a string which
     * can be easily deserialized by your own "deserialize" method later.
     */
    public String serialize(TrieNode root) {
        // Write your code here
        if (root == null)
            return "";

        StringBuffer sb = new StringBuffer();
        sb.append("<");
        Iterator iter = root.children.entrySet().iterator(); 
        while (iter.hasNext()) {
            Map.Entry entry = (Map.Entry)iter.next(); 
            Character key = (Character)entry.getKey(); 
            TrieNode child = (TrieNode)entry.getValue();
            sb.append(key);
            sb.append(serialize(child));
        }
        sb.append(">");
        return sb.toString();
    }

    /**
     * This method will be invoked second, the argument data is what exactly
     * you serialized at method "serialize", that means the data is not given by
     * system, it's given by your own serialize method. So the format of data is
     * designed by yourself, and deserialize it here as you serialize it in 
     * "serialize" method.
     */
    public TrieNode deserialize(String data) {
        // Write your code here
        if (data == null || data.length() == 0)
            return null;

        TrieNode root = new TrieNode();
        TrieNode current = root;
        Stack<TrieNode> path = new Stack<TrieNode>();
        for (Character c : data.toCharArray()) {
            switch (c) {
            case '<':
                path.push(current);
                break;
            case '>':
                path.pop();
                break;
            default:
                current = new TrieNode();
                path.peek().children.put(c, current);
            }
        }
        return root;
    }
```

这题serialize采用了直接dfs遍历树把单词逐个取出。然后deserialize再逐个插入。

```java
public String serialize(TrieNode root) {
    if (root == null) {
        return "";
    }

    ArrayList<String> res = new ArrayList<>();
    StringBuilder tmp = new StringBuilder();
    dfsHelper(res, tmp, root);

    StringBuilder retString = new StringBuilder();
    for (String s : res) {
        retString.append(s);
        retString.append(",");
    }
    return retString.toString();
}

private void dfsHelper(ArrayList<String> res, StringBuilder tmp, TrieNode root) {
    if (root.children.size() == 0) {
        res.add(tmp.toString());
        return;
    }

    for (Map.Entry<Character, TrieNode> entry : root.children.entrySet()) {
        tmp.append(entry.getKey());
        dfsHelper(res, tmp, entry.getValue());    
        tmp.deleteCharAt(tmp.length() - 1);
    }
}


/**
 * This method will be invoked second, the argument data is what exactly
 * you serialized at method "serialize", that means the data is not given by
 * system, it's given by your own serialize method. So the format of data is
 * designed by yourself, and deserialize it here as you serialize it in 
 * "serialize" method.
 */
public TrieNode deserialize(String data) {
    if (data == null || data.isEmpty()) {
        return null;
    }

    TrieNode root = new TrieNode();
    String[] words = data.split(",");
    for (String word : words) {
        insert(root, word);
    }

    return root;
}

private void insert(TrieNode root, String word) {
    if (word == null || word.isEmpty()) {
        return;
    }

    TrieNode last = root;
    for (int i = 0; i < word.length(); i++) {
        TrieNode child = last.children.get(word.charAt(i));
        if (child == null) {
            child = new TrieNode();
            last.children.put(word.charAt(i), child);
        }

        last = child;
    }
}
```
