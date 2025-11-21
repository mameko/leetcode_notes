# 25 Trie and N-ary Tree

Trie的用途有，autocomplete/typeahead，spell check，IP Routing/longest prefix matching

可以在节点处保存前缀与值的关系，例如：【ab，abc，abd，abcd】然后建Trie的时候，每插一个就把字母++，然后需要查询以ab开头的有多少个的时候，只要遍历到ab节点，然后把数字返回即可。

**基本实现：**

[L559 Trie Service](l559-trie-service.md)

[L442 Implement Trie](l442-implement-trie.md) (208) -- iterative来search和add

[L527 Trie Serialization](l527-trie-serialization.md)

**应用：**

[L473 Add and Search Word](l473-add-and-search-word.md) (211) -- 递归写search和递归写add

[L132 Word Search II](l132-word-search-ii.md) (212) -- 用trie来剪枝

336 Palindrome Pairs

421 maximum XOR of Two Numbers in an Array

[L231 Type ahead](../design/l231-typeahead.md)

425 Word Squares

472 Concatenated Words

**N-Ary Tree:**

[1506 Find Root of N-Ary Tree](1506-find-root-of-n-ary-tree.md)

**other related :**

[L123 Word Search](../28-search/79-word-search.md) -- 搜索
