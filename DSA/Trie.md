# Trie

## Resources

[TechInterviewHandbook](https://www.techinterviewhandbook.org/algorithms/trie/)


### Techniques and edge cases:
m: length of string used in the operation.
Search, Insert, Removal: O(m)

Corner cases: 
- Searching for a string in an empty trie
- Inserting empty strings in a trie


#### Leetcode Problems and Solutions:

##### 208. [Implement Trie](https://leetcode.com/problems/implement-trie-prefix-tree/description/) Medium

    A trie (pronounced as "try") or prefix tree is a tree data structure used to efficiently store and retrieve keys in a dataset of strings. There are various applications of this data structure, such as autocomplete and spellchecker.

Implement the Trie class:

`Trie()` Initializes the trie object.
`void insert(String word)` Inserts the string `word` into the trie.
`boolean search(String word)` Returns `true` if the string `word` is in the trie (i.e., was inserted before), and `false` otherwise.
`boolean startsWith(String prefix)` Returns `true` if there is a previously inserted string `word` that has the prefix `prefix`, and `false` otherwise.
word and prefix consist only of lowercase English letters. At most `3 * 104` calls in total will be made to `insert`, `search`, and `startsWith`.

Input
["Trie", "insert", "search", "search", "startsWith", "insert", "search"]
[[], ["apple"], ["apple"], ["app"], ["app"], ["app"], ["app"]]
Output
[null, null, true, false, true, null, true]
Explanation:
Trie trie = new Trie();
trie.insert("apple");
trie.search("apple");   // return True
trie.search("app");     // return False
trie.startsWith("app"); // return True
trie.insert("app");
trie.search("app");     // return True

Solution: 
TC: O(), SC: O()
```python
class TrieNode: #needed to implement the trie constructor
    def __init__(self):
        self.children = [None] * 26 #initially 26 empty lowercase alphabets
        self.end = False #end of word


class Trie:
    def __init__(self):
        """
        Initialize your data structure here.
        """
        self.root = TrieNode() #root node and then we can get the rest of the nodes from this.

    def insert(self, word: str) -> None:
        """
        Inserts a word into the trie.
        """
        curr = self.root #start at root
        for c in word: #iterate through word
            i = ord(c) - ord("a") #
            if curr.children[i] == None:
                curr.children[i] = TrieNode() #insert if not present
            curr = curr.children[i] #if exists, then update and move on
        curr.end = True #set to last char of the word(end of word=true)

    def search(self, word: str) -> bool:
        """
        Returns if the word is in the trie.
        """
        curr = self.root
        for c in word:
            i = ord(c) - ord("a")
            if curr.children[i] == None:
                return False
            curr = curr.children[i] #move to child node of that character
        return curr.end

    def startsWith(self, prefix: str) -> bool:
        """
        Returns if there is any word in the trie that starts with the given prefix.
        """
        curr = self.root
        for c in prefix:
            i = ord(c) - ord("a")
            if curr.children[i] == None:
                return False
            curr = curr.children[i]
        return True
```

##### 211. [Design Add and Search Words Data Structure](https://leetcode.com/problems/design-add-and-search-words-data-structure/description/) Medium

Design a data structure that supports adding new words and finding if a string matches any previously added string.

Implement the `WordDictionary` class:

`WordDictionary()` Initializes the object.
`void addWord(word)` Adds `word` to the data structure, it can be matched later.
`bool search(word)` Returns `true` if there is any string in the data structure that matches `word` or `false` otherwise. `word` may contain dots `'.'` where dots can be matched with any letter.
word in addWord consists of lowercase English letters.
word in search consist of '.' or lowercase English letters.
There will be at most 2 dots in word for search queries.
At most 104 calls will be made to addWord and search.

Input
["WordDictionary","addWord","addWord","addWord","search","search","search","search"]
[[],["bad"],["dad"],["mad"],["pad"],["bad"],[".ad"],["b.."]]
Output
[null,null,null,null,false,true,true,true]
Explanation:
WordDictionary wordDictionary = new WordDictionary();
wordDictionary.addWord("bad");
wordDictionary.addWord("dad");
wordDictionary.addWord("mad");
wordDictionary.search("pad"); // return False
wordDictionary.search("bad"); // return True
wordDictionary.search(".ad"); // return True
wordDictionary.search("b.."); // return True

Solution: 
TC: O(), SC: O()

```python
class TrieNode:
    def __init__(self):
        self.children = {}  # a : TrieNode hashmap
        self.word = False #end of word


class WordDictionary:
    def __init__(self):
        self.root = TrieNode() #initialize

    def addWord(self, word: str) -> None:
        cur = self.root #start
        for c in word:
            if c not in cur.children: #if not inserted
                cur.children[c] = TrieNode() #then add
            cur = cur.children[c]
        cur.word = True

    def search(self, word: str) -> bool:
        def dfs(j, root):
            cur = root

            for i in range(j, len(word)): #index
                c = word[i]
                if c == ".": #26 different paths, hence recursion
                    for child in cur.children.values(): #hasmap values to be matched
                        if dfs(i + 1, child): #index since we are going down a child, so i+1, curr child
                            return True #if we find the path
                    return False #if we dont find a match in the path
                else: #regular character
                    if c not in cur.children:
                        return False
                    cur = cur.children[c]
            return cur.word #if curr.word is true, else false

        return dfs(0, self.root) #call dfs(beg of word, root node)
```

##### 212. [Word Search II](https://leetcode.com/problems/word-search-ii/description/) Hard
Given an `m x n` `board` of characters and a list of strings `words`, return all words on the board.

Each word must be constructed from letters of sequentially adjacent cells, where adjacent cells are horizontally or vertically neighboring. The same letter cell may not be used more than once in a word.
m == board.length
n == board[i].length
1 <= m, n <= 12
board[i][j] is a lowercase English letter.
1 <= words.length <= 3 * 104
1 <= words[i].length <= 10
All the strings of words are unique.

Input: board = [["o","a","a","n"],["e","t","a","e"],["i","h","k","r"],["i","f","l","v"]], words = ["oath","pea","eat","rain"]
Output: ["eat","oath"]

Input: board = [["a","b"],["c","d"]], words = ["abcb"]
Output: []

Solution: 
TC: O(), SC: O()
```python
class TrieNode:
    def __init__(self):
        self.children = {}
        self.isWord = False
        self.refs = 0

    def addWord(self, word):
        cur = self
        cur.refs += 1
        for c in word:
            if c not in cur.children:
                cur.children[c] = TrieNode()
            cur = cur.children[c]
            cur.refs += 1
        cur.isWord = True

    def removeWord(self, word):
        cur = self
        cur.refs -= 1
        for c in word:
            if c in cur.children:
                cur = cur.children[c]
                cur.refs -= 1


class Solution:
    def findWords(self, board: List[List[str]], words: List[str]) -> List[str]:
        root = TrieNode()
        for w in words:
            root.addWord(w)

        ROWS, COLS = len(board), len(board[0])
        res, visit = set(), set()

        def dfs(r, c, node, word):
            if (
                r not in range(ROWS) 
                or c not in range(COLS)
                or board[r][c] not in node.children
                or node.children[board[r][c]].refs < 1
                or (r, c) in visit
            ):
                return

            visit.add((r, c))
            node = node.children[board[r][c]]
            word += board[r][c]
            if node.isWord:
                node.isWord = False
                res.add(word)
                root.removeWord(word)

            dfs(r + 1, c, node, word)
            dfs(r - 1, c, node, word)
            dfs(r, c + 1, node, word)
            dfs(r, c - 1, node, word)
            visit.remove((r, c))

        for r in range(ROWS):
            for c in range(COLS):
                dfs(r, c, root, "")

        return list(res)
```