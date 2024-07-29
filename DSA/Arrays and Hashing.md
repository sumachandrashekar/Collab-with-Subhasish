# Arrays and Hashing

## Resources

[TechInterviewHandbook](https://www.techinterviewhandbook.org/algorithms/array/)
https://www.linkedin.com/posts/ishmeetsinghsethi_solving-dsa-problems-on-leetcode-during-my-activity-7196868695564976129-A6mJ?utm_source=share&utm_medium=member_desktop


### Notes: 

Same type of data stored. Easy to access elements using index. 
Subarray: a range of contiguous values. Eg: For [2,3,6,1,5,4], the subarray is [3,6,1]
Subsequence: deleting some/no elements without changing order. Eg: For [2,3,6,1,5, 4], the subsequence is [3,1,5]

Space complexity: O(n); n: # of elements in array
Time Complexity: 
Access, Insert(at end), Removal(at end): O(1)
Search/Removal: O(n)
Search a sorted array: O(log(n))


### Techniques and edge cases:

- Check for empty arrays
- Check for duplicates inside the array. 
- If using index, do not go out of bounds
- Be careful about slicing and concatenation(Takes O(n) time)

Edge cases:

- Empty sequence
- Sequence with 1 or 2 elements
- Sequence with repeated elements
- Duplicated values in the sequence

Techniques:

- Sliding Window
- Two Pointers
- Traverse from right
- Sorting the array
- Pre-computation
- Index as a hash key
- Traversing the array more than once
- Hashset


#### Leetcode Problems and Solutions:

##### 217. [Contains Duplicates](https://leetcode.com/problems/contains-duplicate/) Easy
Given an integer array nums, return true if any value appears at least twice in the array, and return false if every element is distinct.
Input: nums = [1,2,3,1]
Output: true
Input: nums = [1,2,3,4]
Output: false

Solution:
TC: O(n) since we iterate through entire int array once, SC: O(n) use a hashset
```python
class Solution: 
    def isDup(self, nums: List[int]) -> bool:
        hashset = set()
        for i in nums:
            if i in hashset():
                return True
            hashset.add(i)
        return False

```

##### 242. [Valid Anagram](https://leetcode.com/problems/valid-anagram/description/) Easy
Given two strings s and t, return true if t is an anagram of s, and false otherwise.

An Anagram is a word or phrase formed by rearranging the letters of a different word or phrase, typically using all the original letters exactly once.

Input: s = "anagram", t = "nagaram"
Output: true
Input: s = "rat", t = "car"
Output: false

Solution: 
TC: O(s+t), SC: O(s+t)
```python
#one answer, may not be accepted. Using counter
class Solution:
    def isAnagram(self, s: str, t: str) -> bool:
        return collections.Counter(s) == collections.Counter(t)

# accepted answer
class Solution:
    def isAnagram(self, s: str, t: str) ->bool:
        if len(s) != len(t):
            return False
        countS, countT = {}, {} #create a hashmap to keep track of key(alphabet): value(count of occurances)
        for i in range(len(s)):
            countS[s[i]] = 1 + CountS.get(s[i],0)
            countT[t[i]] = 1 + CountT.get(t[i],0)
        return countS == CountT
```



##### 1. [Two Sum](https://leetcode.com/problems/two-sum/description/) Easy
Given an array of integers nums and an integer target, return indices of the two numbers such that they add up to target.
You may assume that each input would have exactly one solution, and you may not use the same element twice.
You can return the answer in any order.

Eg: Input: nums = [2,7,11,15], target = 9
Output: [0,1]

Input: nums = [3,3], target = 6
Output: [0,1]

Solution: TC: O(n), SC: O(n)

```python
class Solution:
    def twoSum(self, nums: List[int], target: int) -> List[int]:
        hashmap = {}
        for i in range(len(nums)):
            complement = target - nums[i]
            if complement in hashmap:
                return [i, hashmap[complement]]
            hashmap[nums[i]] = i

```

##### 49. [Group Anagrams](https://leetcode.com/problems/group-anagrams/description/) Medium

Given an array of strings `strs`, group the anagrams together. You can return the answer in any order.
An Anagram is a word or phrase formed by rearranging the letters of a different word or phrase, typically using all the original letters exactly once.
strs[i] consists of lowercase English letters.

Input: strs = ["eat","tea","tan","ate","nat","bat"]
Output: [["bat"],["nat","tan"],["ate","eat","tea"]]

Input: strs = [""]
Output: [[""]]

Input: strs = ["a"]
Output: [["a"]]

Solution: TC: O(m*n), SC: O(m+n); m: #words in a string, n: avg # elements in a string

```python
class Solution: 
    def groupAnagrams(self, strs: List[strs]) -> List(List(str)):
        ans = defaultdict(list) #taking empty list
        for s in strs:
            count = [0] * 26
            for c in strs:
                count[ord(c) - ord("a")] += 1
            res[tuple(count)].append(s)
        return res.values()

```

##### 347. [Top K Frequent Elements](https://leetcode.com/problems/top-k-frequent-elements/description/) Medium

Given an integer array `nums` and an integer `k`, return the `k` most frequent elements. You may return the answer in any order.
k is in the range [1, the number of unique elements in the array].
It is guaranteed that the answer is unique.
Your algorithm's time complexity must be better than O(n log n), where n is the array's size.

Input: nums = [1,1,1,2,2,3], k = 2
Output: [1,2]

Input: nums = [1], k = 1
Output: [1]

Solution: TC: O(n), SC: O(n)

```python
class Solution:
    def topkFreq(self, nums: List[int], k: int) -> List[int]:
        count = {}
        freq = [[] for i in range(len(nums) + 1)]

        for n in nums:
            count[n] = 1 + count.get(n, 0)
            for n, c in count.items():
                freq[c].append(n)
            
            res = []

            for i in range(len(freq) - 1, 0, -1):
                for n in freq[i]:
                    res.append(n)
                    if len(res) == k:
                        return res
```


##### 271. [Encode and Decode Strings](https://leetcode.com/problems/encode-and-decode-strings/description/) Medium

Design an algorithm to encode a list of strings to a string. The encoded string is then sent over the network and is decoded back to the original list of strings. strs[i] contains any possible characters out of 256 valid ASCII characters. Could you write a generalized algorithm to work on any possible set of characters?

Machine 1 (sender) has the function:

string encode(vector<string> strs) {
  // ... your code
  return encoded_string;
}
Machine 2 (receiver) has the function:
vector<string> decode(string s) {
  //... your code
  return strs;
}
So Machine 1 does:

string encoded_string = encode(strs);
and Machine 2 does:

vector<string> strs2 = decode(encoded_string);
strs2 in Machine 2 should be the same as strs in Machine 1.

Implement the encode and decode methods.

You are not allowed to solve the problem using any serialize methods (such as eval).


Input: dummy_input = ["Hello","World"]
Output: ["Hello","World"]
Explanation:
Machine 1:
Codec encoder = new Codec();
String msg = encoder.encode(strs);
Machine 1 ---msg---> Machine 2

Machine 2:
Codec decoder = new Codec();
String[] strs = decoder.decode(msg);
Example 2:

Input: dummy_input = [""]
Output: [""]

Solution: 
TC: O(), SC: O()
```python
# Non-optimal solution. Using any character like '#' which is usually not used in ASCII strings. TC: O(n), SC: O(k) where k is space delimiter.
class Codec: 
    def encode(self, strs):
        return '#'.join(strs)
    
    def decode(self, s):
        return s.split('#)

#Optimal solution: Using Chunked Transfer encoding.Each string preceeded by it's length and delimiter. Even if the string has the delimiter, we know the string boundaries. 
class Codec:
    def encode(self, strs):
        # Initialize an empty string to hold the encoded string.
        encoded_string = ''
        for s in strs:
            # Append the length, the delimiter, and the string itself.
            encoded_string += str(len(s)) + '/:' + s
        return encoded_string

    def decode(self, s):
        # Initialize a list to hold the decoded strings.
        decoded_strings = []
        i = 0
        while i < len(s):
            # Find the delimiter.
            delim = s.find('/:', i)
            # Get the length, which is before the delimiter.
            length = int(s[i:delim])
            # Get the string, which is of 'length' length after the delimiter.
            str_ = s[delim+2 : delim+2+length]
            # Add the string to the list.
            decoded_strings.append(str_)
            # Move the index to the start of the next length.
            i = delim + 2 + length
        return decoded_strings
```

##### 238. [Product Of Array Except Self](https://leetcode.com/problems/product-of-array-except-self/description/) Medium

Given an integer array `nums`, return an array `answer` such that `answer[i]` is equal to the product of all the elements of `nums` except `nums[i]`.
The product of any prefix or suffix of `nums` is guaranteed to fit in a 32-bit integer.
You must write an algorithm that runs in `O(n)` time and without using the division operation.
Can you solve the problem in O(1) extra space complexity? (The output array does not count as extra space for space complexity analysis.

Input: nums = [1,2,3,4]
Output: [24,12,8,6]
Input: nums = [-1,1,0,-3,3]
Output: [0,0,9,0,0]

Solution: 
TC: O(n), SC: O(1)
```python
class Solution:
    def productExceptSelf(self, nums: List[int]) -> List[int]:
        res = [1] * (len(nums))

        for i in range(1, len(nums)):
            res[i] = res[i-1] * nums[i-1]
        postfix = 1
        for i in range(len(nums) - 1, -1, -1):
            res[i] *= postfix
            postfix *= nums[i]
        return res
```

##### 36. [Valid Sudoku](https://leetcode.com/problems/valid-sudoku/description/) Medium
Determine if a 9 x 9 Sudoku board is valid. Only the filled cells need to be validated according to the following rules:

Each row must contain the digits 1-9 without repetition.
Each column must contain the digits 1-9 without repetition.
Each of the nine 3 x 3 sub-boxes of the grid must contain the digits 1-9 without repetition.
A Sudoku board (partially filled) could be valid but is not necessarily solvable.
Only the filled cells need to be validated according to the mentioned rules.
board.length == 9
board[i].length == 9
board[i][j] is a digit 1-9 or '.'

Input: board = 
[["5","3",".",".","7",".",".",".","."]
,["6",".",".","1","9","5",".",".","."]
,[".","9","8",".",".",".",".","6","."]
,["8",".",".",".","6",".",".",".","3"]
,["4",".",".","8",".","3",".",".","1"]
,["7",".",".",".","2",".",".",".","6"]
,[".","6",".",".",".",".","2","8","."]
,[".",".",".","4","1","9",".",".","5"]
,[".",".",".",".","8",".",".","7","9"]]
Output: true

Input: board = 
[["8","3",".",".","7",".",".",".","."]
,["6",".",".","1","9","5",".",".","."]
,[".","9","8",".",".",".",".","6","."]
,["8",".",".",".","6",".",".",".","3"]
,["4",".",".","8",".","3",".",".","1"]
,["7",".",".",".","2",".",".",".","6"]
,[".","6",".",".",".",".","2","8","."]
,[".",".",".","4","1","9",".",".","5"]
,[".",".",".",".","8",".",".","7","9"]]
Output: false
Explanation: Same as Example 1, except with the 5 in the top left corner being modified to 8. Since there are two 8's in the top left 3x3 sub-box, it is invalid.

Solution: 
TC: O(9^2), SC: O(9^2)
```python
class Solution:
    def isValidSudoku(self, board: List[List[str]]) -> bool:
        cols = collections.defaultdict(set) #hashsets for row, col and 3*3 matrix
        rows = collections.defaultdict(set)
        squares = collections.defaultdict(set)

        for r in range(9):
            for c in range(9):
                if board[r][c] == ".":
                    continue #continue if empty
                if (board[r][c] in rows[r] or
                    board[r][c] in cols[c] or
                    board[r][c] in squares[(r // 3, c // 3)]):
                    return False #if already present in hashset, return false
                cols[c].add(board[r][c]) #adding elements in hashset
                rows[r].add(board[r][c])
                squares[(r //3, c //3)].add(board[r][c])
        return True
        
```

##### 128. [Longest Consecutive Sequence](https://leetcode.com/problems/longest-consecutive-sequence/description/) Medium
Given an unsorted array of integers nums, return the length of the longest consecutive elements sequence.
You must write an algorithm that runs in O(n) time.

Input: nums = [100,4,200,1,3,2]
Output: 4
Explanation: The longest consecutive elements sequence is [1, 2, 3, 4]. Therefore its length is 4.
Input: nums = [0,3,7,2,5,8,4,6,0,1]
Output: 9

Solution: 
TC: O(n), SC: O(n)
```python
class Solution:
    def longestConsecutive(self, nums: List[int]) -> int:
        numset = set(nums)
        longest_streak = 0

        for n in nums:
            if (n-1) not in numset:
                current_num = n
                current_streak = 1

                while current_num + 1 in numset:
                    current_num += 1
                    current_streak += 1
                longest_streak = max(longest_streak, current_streak)
        return longest_streak
```

##### 118. [Pascal's Triangle](https://leetcode.com/problems/pascals-triangle/description/) Easy
Given an integer numRows, return the first numRows of Pascal's triangle.
In Pascal's triangle, each number is the sum of the two numbers directly above it as shown:
Input: numRows = 5
Output: [[1],[1,1],[1,2,1],[1,3,3,1],[1,4,6,4,1]]
Input: numRows = 1
Output: [[1]]

