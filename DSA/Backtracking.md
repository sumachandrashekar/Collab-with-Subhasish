# Backtracking

## Resources

[TechInterviewHandbook]()


### Techniques and edge cases:

Permutations: N!
Combintaions: C^k base N = (N!)/(N-k)! ^K!
Subsets: 2^N

#### Leetcode Problems and Solutions:

##### 78. [Subsets](https://leetcode.com/problems/subsets/description/) Medium

Given an integer array nums of unique elements, return all possible `subsets` (the power set).

The solution set must not contain duplicate subsets. Return the solution in any order.
Input: nums = [1,2,3]
Output: [[],[1],[2],[1,2],[3],[1,3],[2,3],[1,2,3]]
Input: nums = [0]
Output: [[],[0]]

Solution: 
TC: O(n.2^n) since the max length of subset =n and subsets of n is 2^n, SC: O(n) since it maintains the subset without duplicates
```python
#Powerset is all possible combinations of all possible lengths from 0 to n.
class Solution:
    def subsets(self, nums: List[int]) -> List[List[int]]:
        res = [] #store results
        subset = [] #subset keeps modifying, so keep a copy

        def dfs(i): #using dfs for searching. i is the index value in nums. Build each subset.
            if i >= len(nums): #if i is out of bounds
                res.append(subset.copy())
                return
            # decision to include nums[i] on left branch
            subset.append(nums[i])
            dfs(i+1)
```

##### 39. [Combination Sum](https://leetcode.com/problems/combination-sum/description/) Medium
Given an array of distinct integers `candidates` and a target integer `target`, return a list of all unique combinations of `candidates` where the chosen numbers sum to `target`. You may return the combinations in any order.

The same number may be chosen from `candidates` an unlimited number of times. Two combinations are unique if the 
frequency
 of at least one of the chosen numbers is different.

The test cases are generated such that the number of unique combinations that sum up to `target` is less than `150` combinations for the given input.
Constraints: 
1 <= candidates.length <= 30
2 <= candidates[i] <= 40
All elements of candidates are distinct.
1 <= target <= 40

Input: candidates = [2,3,6,7], target = 7
Output: [[2,2,3],[7]]
Explanation:
2 and 3 are candidates, and 2 + 2 + 3 = 7. Note that 2 can be used multiple times.
7 is a candidate, and 7 = 7.
These are the only two combinations.

Input: candidates = [2,3,5], target = 8
Output: [[2,2,2,2],[2,3,3],[3,5]]
Input: candidates = [2], target = 1
Output: []

Solution:
TC: o(2^t) where t is the target value. SC: O()
```python
class Solution:
    def combinationSum(self, candidates: List[int], target: int) -> List[List[int]]:
        res = [] #result set

        def dfs(i, curr, total): #current pointer, current combination, total of combination
            if total == target:
                res.append(curr.copy()) #append a copy(since curr will undergo changes) to res
                return #break out

            if i >= len(candidates) or total > target:
                return 
            
            curr.append(candidates[i]) #add new element and then pass dfs
            dfs(i, curr, total + candidates[i])

            curr.pop() #remove/skip element of i
            dfs(i + 1, curr, total) #add the next element in the candidate list

        dfs(0, [], 0) #call dfs(beginning index, empty array, current total)
        return res
```

##### 46. [Permutations](https://leetcode.com/problems/permutations/description/) Medium

Given an array `nums` of distinct integers, return all the possible permutations. You can return the answer in any order.
Input: nums = [1,2,3]
Output: [[1,2,3],[1,3,2],[2,1,3],[2,3,1],[3,1,2],[3,2,1]]
Input: nums = [0,1]
Output: [[0,1],[1,0]]
Input: nums = [1]
Output: [[1]]
All the integers of nums are unique.

Solution: 
TC: O(), SC: O()
```python
class Solution:
    def permute(self, nums: List[int]) -> List[List[int]]:
        res = []

        if len(nums) == 1: #if only one value is present, return list of the value
            return [nums.copy()] #deep copy. List of lists

        for i in range(len(nums)): #iterate through each element
            n = nums.pop(0) #pop the 0th index
            perms = self.permute(nums) #recursive call 

            for perm in perms:
                perm.append(n) #add yhe popped value to the list above
            res.extend(perms) #add multiple lists to the result
            nums.append(n) #add the poped value back to the end of the list
        return res
```

##### 90. [Subsets II](https://leetcode.com/problems/subsets-ii/description/) Medium
Given an integer array `nums` that may contain duplicates, return all possible subsets  (the power set).

The solution set must not contain duplicate subsets. Return the solution in any order.
Input: nums = [1,2,2]
Output: [[],[1],[1,2],[1,2,2],[2],[2,2]]
Input: nums = [0]
Output: [[],[0]]

Solution: 
TC: O(n.2^n) because (n log n) is for sorting to find dups, SC: O(n)
```python
class Solution:
    def subsetsWithDup(self, nums: List[int]) -> List[List[int]]:
        res = []
        nums.sort() #sort to keep duplicates next to each other

        def backtrack(i, subset): #index, current subset
            if i == len(nums):
                res.append(subset[::])
                return

            # All subsets that include nums[i]
            subset.append(nums[i])
            backtrack(i + 1, subset) #pass in the next index
            subset.pop() #remove element added in previous append operation
            # All subsets that don't include nums[i]
            while i + 1 < len(nums) and nums[i] == nums[i + 1]: #while i + 1 is inbound and skipping for duplicates
                i += 1
            backtrack(i + 1, subset)  # move onto next index

        backtrack(0, []) # start code(initial run) by calling function with 0th index and empty array
        return res
```

##### 40. [Combination Sum - II](https://leetcode.com/problems/combination-sum-ii/description/) Medium
Given a collection of candidate numbers (`candidates`) and a target number (`target`), find all unique combinations in candidates where the `candidate` numbers sum to `target`.

Each number in `candidates` may only be used once in the combination.

Note: The solution set must not contain duplicate combinations.

Input: candidates = [10,1,2,7,6,1,5], target = 8
Output: 
[
[1,1,6],
[1,2,5],
[1,7],
[2,6]
]

Input: candidates = [2,5,2,1,2], target = 5
Output: 
[
[1,2,2],
[5]
]

Solution: 
TC: O(2^n), SC: O(n)
```python
class Solution:
    def combinationSum2(self, candidates: List[int], target: int) -> List[List[int]]:
        candidates.sort()
        res = []

        def backtrack(cur, pos, target):
            if target == 0:
                res.append(cur.copy())
                return
            
            if target <= 0:
                return
            
            prev = -1

            for i in range(pos, len(candidates)):
                if candidates[i] == prev:
                    continue
                cur.append(candidates[i])
                backtrack(cur, i+1, target - candidates[i])
                cur.pop()
                prev = candidates[i]
        backtrack([], 0, target)
        return res
```

##### 79. [Word Search](https://leetcode.com/problems/word-search/description/) Medium
Given an `m x n` grid of characters `board` and a string `word`, return `true` if `word` exists in the grid.

The word can be constructed from letters of sequentially adjacent cells, where adjacent cells are horizontally or vertically neighboring. The same letter cell may not be used more than once.

Input: board = [["A","B","C","E"],["S","F","C","S"],["A","D","E","E"]], word = "ABCCED"
Output: true
Input: board = [["A","B","C","E"],["S","F","C","S"],["A","D","E","E"]], word = "SEE"
Output: true
Input: board = [["A","B","C","E"],["S","F","C","S"],["A","D","E","E"]], word = "ABCB"
Output: false

Solution: 
TC: O(n * m * 4^n), SC: O(L), where L is length of the word to be matched
```python
class Solution:
    def exist(self, board: List[List[str]], word: str) -> bool:
        ROWS, COLS = len(board), len(board[0]) #$ rows, len of first row
        path = set() #to store current elements

        def dfs(r, c, i): #rows, cols, current char in target word we are looking for

            if i == len(word): 
                return True

            if (min(r,c) < 0 or r >= ROWS or c >= COLS  #if out of counds
            or word[i] != board[r][c] or (r,c) in path): #if wrong char is found or if tuple is inside the path set(revisiting the path twice)
                return False

            path.add((r, c)) #if found, add

            res = ((dfs((r + 1), c, i + 1) or #run res on all four adjacent positions
            dfs((r - 1), c, i + 1)) or
            dfs(r, (c + 1), (i + 1)) or
            dfs(r, (c - 1), (i + 1)))

            path.remove((r,c)) #cleanup since we no longer visit the position
            return res

        for r in range(ROWS): #run dfs brute force across all rows and cols
            for c in range(COLS):
                if dfs(r,c,0):
                    return True
        return False
```

##### 131. [Palindrome Partitioning](https://leetcode.com/problems/palindrome-partitioning/description/) Medium
Given a string `s`, partition `s` such that every substring  of the partition is a palindrome. Return all possible palindrome partitioning of `s`.

Input: s = "aab"
Output: [["a","a","b"],["aa","b"]]
Input: s = "a"
Output: [["a"]]

Solution: 
TC: O(2^n), SC: O()
```python
class Solution:
    def partition(self, s: str) -> List[List[str]]:
        res, part = [], [] # res, curr pos

        def dfs(i):
            if i >= len(s): #base case out of bounds
                res.append(part.copy()) #append rest of it
                return

            for j in range(i, len(s)): #iterate through every other character
                if self.isPali(s, i, j): #check for palindrome
                    part.append(s[i : j + 1]) #start of index i:end of index j
                    dfs(j + 1) #look for additional palindromes
                    part.pop()

        dfs(0)
        return res

    def isPali(self, s, l, r): #l, r are boundaries
        while l < r:
            if s[l] != s[r]:
                return False
            l, r = l + 1, r - 1 #update if both characters are true
        return True
```


##### 17. [Letter Combinations Of a Phone Number](https://leetcode.com/problems/letter-combinations-of-a-phone-number/description/) Medium
Given a string containing digits from `2-9` inclusive, return all possible letter combinations that the number could represent. Return the answer in any order.

A mapping of digits to letters (just like on the telephone buttons) is given below. Note that 1 does not map to any letters.
Input: digits = "23"
Output: ["ad","ae","af","bd","be","bf","cd","ce","cf"]
Input: digits = ""
Output: []
Input: digits = "2"
Output: ["a","b","c"]

Solution: 
TC: O(n.4^n) becasue the longest character for a number is 4(Eg: 9: wxyz), SC: O()

```python
class Solution:
    def letterCombinations(self, digits: str) -> List[str]:
        res = []
        digitToChar = { #mapping
            "2": "abc",
            "3": "def",
            "4": "ghi",
            "5": "jkl",
            "6": "mno",
            "7": "qprs",
            "8": "tuv",
            "9": "wxyz",
        }

        def backtrack(i, curStr): #index, current string like "a"
            if len(curStr) == len(digits): #base case
                res.append(curStr)
                return
            for c in digitToChar[digits[i]]: #for every c in the string from digittochar(map the number 2 to character set defined above)
                backtrack(i + 1, curStr + c) #increment, current string + current char we are visitng

        if digits: #call backtrack only if non-empty
            backtrack(0, "") #call backtrack

        return res
        
```

##### 51. [N-Queens](https://leetcode.com/problems/n-queens/description/) Hard

The n-queens puzzle is the problem of placing `n` queens on an `n x n` chessboard such that no two queens attack each other.

Given an integer `n`, return all distinct solutions to the n-queens puzzle. You may return the answer in any order.

Each solution contains a distinct board configuration of the n-queens' placement, where `'Q'` and `'.'` both indicate a queen and an empty space, respectively.

Input: n = 4
Output: [[".Q..","...Q","Q...","..Q."],["..Q.","Q...","...Q",".Q.."]]
Explanation: There exist two distinct solutions to the 4-queens puzzle as shown above
Input: n = 1
Output: [["Q"]]

Solution: 
TC: O(), SC: O()
```python
class Solution:
    def solveNQueens(self, n: int) -> List[List[str]]:
        col = set()
        posDiag = set()  # (r + c)
        negDiag = set()  # (r - c)

        res = []
        board = [["."] * n for i in range(n)]

        def backtrack(r):
            if r == n:
                copy = ["".join(row) for row in board]
                res.append(copy)
                return

            for c in range(n):
                if c in col or (r + c) in posDiag or (r - c) in negDiag:
                    continue

                col.add(c)
                posDiag.add(r + c)
                negDiag.add(r - c)
                board[r][c] = "Q"

                backtrack(r + 1)

                col.remove(c)
                posDiag.remove(r + c)
                negDiag.remove(r - c)
                board[r][c] = "."

        backtrack(0)
        return res
        
```
