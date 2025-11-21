# L231 Typeahead

Implement typeahead. Given a string and a dictionary, return all words that contains the string as a substring. The dictionary will give at the initialize method and wont be changed. The method to find all words with given substring would be called multiple times.

**Example**

Given dictionary =`{"Jason Zhang", "James Yu", "Bob Zhang", "Larry Shi"}`

search`"Zhang"`, return`["Jason Zhang", "Bob Zhang"]`.

search`"James"`, return`["James Yu"]`.

上课老师说用trie，所以我用了trie，然后发现九章答案里用hashmap...囧

```java
public class Typeahead {
    Trie t;
    // @param dict A dictionary of words dict
    public Typeahead(Set<String> dict) {
        // do initialize if necessary
        t = new Trie();

        for (String word : dict) {
            for (int i = 0; i < word.length(); i++) {
                String sub = word.substring(i);
                t.insert(sub, word);
            }
        }
    }

    // @param str: a string
    // @return a list of words
    public List<String> search(String str) {
        return new ArrayList<>(t.prefix(str));
    }
}

class Trie {
    TrieNode root;

    public Trie() {
        root = new TrieNode();
    }

    public void insert(String word, String phrase) {
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
            cur.wordList.add(phrase);
            last = cur;
        }

        last.end = true;
    }

    public Set<String> prefix(String str) {
        Set<String> emptyList = new HashSet<>();
        if (str == null || str.isEmpty()) {
            return emptyList;
        }

        TrieNode last = root;
        for (int i = 0; i < str.length(); i++) {
            TrieNode cur = last.children.get(str.charAt(i));
            if (cur == null) {
                return emptyList;
            }

            last = cur;
        }

        return last.wordList;
    }
}

class TrieNode {
    HashMap<Character, TrieNode> children = new HashMap<>();
    boolean end;
    Set<String> wordList = new HashSet<>();
}
```
