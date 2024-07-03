# Sliding Window

## Resources

- [ ] [Sliding Window Algorithmic mental models-YT](https://www.youtube.com/watch?v=MK-NZ4hN7rs)

[Aditya Verma's Sliding window playlist](https://www.youtube.com/watch?v=EHCGAZBbB88&list=PL_z_8CaSLPWeM8BDJmIYDaoQ5zuwyxnfj&pp=iAQB)


[Udemy DSA Bootcamp](https://www.udemy.com/course/data-structures-and-algorithms-bootcamp-in-python/)

[Algorithms Pricenton book](https://algs4.cs.princeton.edu/home/)

[Sliding windown leetcode discussion](https://leetcode.com/problems/frequency-of-the-most-frequent-element/solutions/1175088/c-maximum-sliding-window-cheatsheet-template/comments/1427583/)

https://www.reddit.com/r/leetcode/comments/w5ui9x/for_sliding_window_problems_how_do_you_know_when/

https://www.reddit.com/r/leetcode/comments/15a7ezt/sliding_window_technique/


### Techniques and edge cases: 

- Remember that you usually declare two pointers initialized to 0 and need two loops
- You need an outer for loop to increment right pointer at each iteration.
- You need an inner while loop to increment the left pointer whenever the window/subarray is invalid.

Three types of sliding window: 
- Fixed(medium)
- Dynamic(medium)
- Dynamic with auxillary data structure(hard)



#### Leetcode Problems and Solutions:

#### 121. [Best Time To Buy And Sell Stock](https://leetcode.com/problems/best-time-to-buy-and-sell-stock/) Easy

You are given an array prices where prices[i] is the price of a given stock on the ith day.
You want to maximize your profit by choosing a single day to buy one stock and choosing a different day in the future to sell that stock.
Return the maximum profit you can achieve from this transaction. If you cannot achieve any profit, return 0.

Input: prices = [7,1,5,3,6,4]
Output: 5

Input: prices = [7,6,4,3,1]
Output: 0


Solution: TC: o(n) since single pass needed; SC: O(1) since two variables used.

```python
class Solution:
    def maxProfit(self, prices: List[int]) -> int:
        res = 0
        
        lowest = prices[0]
        for price in prices:
            if price < lowest:
                lowest = price
            res = max(res, price - lowest)
        return res
```


#### 3. [Longest substring without repeating characters](https://leetcode.com/problems/longest-substring-without-repeating-characters/description/) Medium

Given a string s, find the length of the longest substring without repeating characters. s consists of English letters, digits, symbols and spaces.

Input: s = "abcabcbb"
Output: 3

Input: s = "bbbbb"
Output: 1

Input: s = "pwwkew"
Output: 3

Solution: TC: O(n), SC: O(1)

```python
class Solution:
    def lengthOfLongestSubstring(self, s: str) -> int:
        repeat = dict()
        start = longest = 0
        
        for i, char in enumerate(s):
            if char in repeat and repeat[char] >= start:
                start = repeat[char] + 1
            
            repeat[char] = i

            if longest < i - start + 1:
                longest = i - start + 1

        return longest
        
```

#### 424. [Longest Repeating Character Replacement](https://leetcode.com/problems/longest-repeating-character-replacement/description/) Medium

You are given a string s and an integer k. You can choose any character of the string and change it to any other uppercase English character. You can perform this operation at most k times.
Return the length of the longest substring containing the same letter you can get after performing the above operations. s consists of only uppercase English letters.0 <= k <= s.length

Input: s = "ABAB", k = 2
Output: 4

Input: s = "AABABBA", k = 1
Output: 4

Solution: TC: O(n), SC: O()

```python
class Solution:
    def characterReplacement(self, s: str, k: int) -> int:
        counts = {}
        maxf = 0
        l = 0
        for i, char in enumerate(s):
            counts[char] = 1 + counts.get(char,0)
            maxf = max(maxf, counts[char])

            if maxf + k < i - l + 1:
                counts[s[l]] -= 1
                l += 1

        return len(s) - l
```


#### 567. [Permutation in string](https://leetcode.com/problems/permutation-in-string/description/) Medium

Given two strings s1 and s2, return true if s2 contains a permutation of s1, or false otherwise.
In other words, return true if one of s1's permutations is the substring of s2.s1 and s2 consist of lowercase English letters.

Input: s1 = "ab", s2 = "eidbaooo"
Output: true

Input: s1 = "ab", s2 = "eidboaoo"
Output: false

Solution: TC: O(n), SC: O(1)

```python
class Solution:
    def checkInclusion(self, s1: str, s2: str) -> bool:
        cntr, w, matched = Counter(s1), len(s1), 0   

        for i in range(len(s2)):
            if s2[i] in cntr: 
                cntr[s2[i]] -= 1
                if cntr[s2[i]] == 0:
                    matched += 1
            if i >= w and s2[i-w] in cntr: 
                if cntr[s2[i-w]] == 0:
                    matched -= 1
                cntr[s2[i-w]] += 1

            if matched == len(cntr):
                return True

        return False
```

#### 76. [Minimum Window Substring](https://leetcode.com/problems/minimum-window-substring/description/) Hard

Given two strings s and t of lengths m and n respectively, return the minimum window substring  of s such that every character in t (including duplicates) is included in the window. If there is no such substring, return the empty string "".
The testcases will be generated such that the answer is unique.

Input: s = "ADOBECODEBANC", t = "ABC"
Output: "BANC"

Input: s = "a", t = "a"
Output: "a"

Input: s = "a", t = "aa"
Output: ""



##### 239. [Sliding Window Maximum](https://leetcode.com/problems/sliding-window-maximum/description/) Hard

