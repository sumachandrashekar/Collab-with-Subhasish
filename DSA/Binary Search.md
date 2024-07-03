# Binary Search

## Resources

[TechInterviewHandbook](https://www.techinterviewhandbook.org/algorithms/sorting-searching/)


### Techniques and edge cases:
Hint: Sorted array or solution needing a runtime of o(log n) implies binary search

#### Leetcode Problems and Solutions:

##### 704. [Binary Search](https://leetcode.com/problems/binary-search/description/) Easy

Given an array of integers `nums` which is sorted in ascending order, and an integer `target`, write a function to search `target` in `nums`. If target exists, then return its index. Otherwise, return `-1`.

You must write an algorithm with `O(log n)` runtime complexity.

Input: nums = [-1,0,3,5,9,12], target = 9
Output: 4
Explanation: 9 exists in nums and its index is 4

Input: nums = [-1,0,3,5,9,12], target = 2
Output: -1
Explanation: 2 does not exist in nums so return -1

Solution: 
Using two pointer approach, increment using the median.
TC: O(), SC: O(1)

```python
class Solution:
    def binSearch(self, nums: List[int], target:[int]) -> int:
        l, r = 0, len(nums) - 1
        
        while l <= r:
            m = (l + r) // 2
            if nums[m] == target:
                return m
            elif nums[m] < target:
                l += 1
            elif nums[m] > target:
                r -= 1
        return -1
```

##### 74. [Search in a 2D Matrix](https://leetcode.com/problems/search-a-2d-matrix/description/) Medium

You are given an `m x n` integer matrix `matrix` with the following two properties:

- Each row is sorted in non-decreasing order.
- The first integer of each row is greater than the last integer of the previous row.
Given an integer `target`, return `true` if `target` is in `matrix` or `false` otherwise.

You must write a solution in `O(log(m * n))` time complexity.

Input: matrix = [[1,3,5,7],[10,11,16,20],[23,30,34,60]], target = 3
Output: true

Input: matrix = [[1,3,5,7],[10,11,16,20],[23,30,34,60]], target = 13
Output: false

Solution: 


```python
class Solution:
    def searchMatrix(self, matrix: List[List[int]], target: int) -> bool:
        ROWS, COLS = len(matrix), len(matrix[0])
        top, bot = 0, ROWS - 1
        while top <= bot:
            mid = (top + bot) // 2
            if target > matrix[mid][-1]:
                top = mid + 1
            elif target < matrix[mid][0]:
                bot = mid - 1
            else:
                break
                
        if not (top <= bot):
            return False
        
        row = (top + bot) // 2
        l, r = 0, COLS -1
        while l <= r:
            m = (l + r) // 2
            if target > matrix[row][m]:
                l = m + 1
            elif target < matrix[row][m]:
                r = m - 1
            else:
                return True
        return False
```

##### 875. [Koko Eating Bananas](https://leetcode.com/problems/koko-eating-bananas/description/) Medium

Koko loves to eat bananas. There are `n` piles of bananas, the `ith` pile has `piles[i]` bananas. The guards have gone and will come back in `h` hours.

Koko can decide her bananas-per-hour eating speed of `k`. Each hour, she chooses some pile of bananas and eats `k` bananas from that pile. If the pile has less than `k` bananas, she eats all of them instead and will not eat any more bananas during this hour.

Koko likes to eat slowly but still wants to finish eating all the bananas before the guards return.

Return the minimum integer `k` such that she can eat all the bananas within `h` hours.


Input: piles = [3,6,7,11], h = 8
Output: 4

Input: piles = [30,11,23,4,20], h = 5
Output: 30

Input: piles = [30,11,23,4,20], h = 6
Output: 23

Solution: 
ref: <https://leetcode.com/problems/koko-eating-bananas/solutions/769702/python-clear-explanation-powerful-ultimate-binary-search-template-solved-many-problems/>

TC: O(n log n), SC: O(1)
```python
class Solution:
    def minEatingSpeed(self, piles: List[int], h: int) -> int:
        left = ceil(sum(piles) / h) # lower bound of Binary Search 
        right = max(piles) # upper bound of Binary Search 
        while left < right:
            mid = (left + right) // 2 # we check for k=mid
            total_time = 0
            for i in piles:
                total_time += ceil(i / mid)
                if total_time > h:
                    break
            if total_time <= h:
                right = mid # answer must lie to the left half (inclusive of current value ie mid)
            else:
                left = mid + 1 # answer must lie to the right half (exclusive of current value ie mid)
        return right
```

##### 153. [Find Minimum In A Rotated Sorted Array](https://leetcode.com/problems/find-minimum-in-rotated-sorted-array/description/) Medium

Suppose an array of length `n` sorted in ascending order is rotated between `1` and `n` times. For example, the array `nums = [0,1,2,4,5,6,7]` might become:

[4,5,6,7,0,1,2] if it was rotated `4` times.
[0,1,2,4,5,6,7] if it was rotated `7` times.
Notice that rotating an array `[a[0], a[1], a[2], ..., a[n-1]]` 1 time results in the array `[a[n-1], a[0], a[1], a[2], ..., a[n-2]]`.

Given the sorted rotated array `nums` of unique elements, return the minimum element of this array.

You must write an algorithm that runs in `O(log n) time`.(Hint here: anywhere O(log n) is mentioned, it would be binary search)

Input: nums = [3,4,5,1,2]
Output: 1
Explanation: The original array was [1,2,3,4,5] rotated 3 times.

Input: nums = [4,5,6,7,0,1,2]
Output: 0
Explanation: The original array was [0,1,2,4,5,6,7] and it was rotated 4 times.

Input: nums = [11,13,15,17]
Output: 11
Explanation: The original array was [11,13,15,17] and it was rotated 4 times.

Solution: TC: O(long n), SC: O(1)

```python
class Solution:
    def findMin(self, nums: List[int]) -> int:
        res = nums[0] #to store res. left most value initialized
        l , r = 0, len(nums) - 1
        while l <= r:
            if nums[l] < nums[r]: #if subarray is sorted, then update the result and break
                res = min(res, nums[l])
                break
            
            m = (l + r) // 2 #if subarray is not sorted
            res = min(res, nums[m])
            if nums[m] >= nums[l]: #if m is part of the `l` sorted array
                l = m + 1
            else:
                r = m - 1
        return res

```


##### 33. [Search In A Rotated Sorted Array](https://leetcode.com/problems/search-in-rotated-sorted-array/description/) Medium

There is an integer array `nums` sorted in ascending order (with distinct values).

Prior to being passed to your function, `nums` is possibly rotated at an unknown pivot index `k` `(1 <= k < nums.length)` such that the resulting array is `[nums[k], nums[k+1], ..., nums[n-1], nums[0], nums[1], ..., nums[k-1]]` (0-indexed). For example, `[0,1,2,4,5,6,7]` might be rotated at pivot index 3 and become `[4,5,6,7,0,1,2]`.

Given the array `nums` after the possible rotation and an integer `target`, return the index of `target` if it is in `nums`, or `-1` if it is not in `nums`.

You must write an algorithm with `O(log n)` runtime complexity.

Input: nums = [4,5,6,7,0,1,2], target = 0
Output: 4

Input: nums = [4,5,6,7,0,1,2], target = 3
Output: -1

Input: nums = [1], target = 0
Output: -1


Solution:
TC: O(log n), SC: O(1)

```python
class Solution:
    def search(self, nums: List[int], target: int) -> int:
        l, r = 0, len(nums) - 1

        while l <= r :
            mid = (l + r) // 2
            if target == nums[mid]:
                return mid

            if nums[l] <= nums[mid]:  #check if we are in the left sorted portion
                if target > nums[mid] or target < nums[l]:
                    l = mid + 1 #search the right most
                else:
                    r = mid - 1
                    
            else: #right sorted portion
                if target < nums[mid] or target > nums[r]:
                    r = mid - 1
                else: 
                    l = mid + 1
        return -1
```

##### 981. [Time Based Key Value Store](https://leetcode.com/problems/time-based-key-value-store/description/) Medium

Design a time-based key-value data structure that can store multiple values for the same key at different time stamps and retrieve the key's value at a certain timestamp.
key and value consist of lowercase English letters and digits. All the timestamps timestamp of set are strictly increasing.
Implement the `TimeMap` class:

`TimeMap()` Initializes the object of the data structure.
`void set(String key, String value, int timestamp)` Stores the key key with the value value at the given time timestamp.
`String get(String key, int timestamp)` Returns a value such that `set` was called previously, with `timestamp_prev <= timestamp`. If there are multiple such values, it returns the value associated with the largest `timestamp_prev`. If there are no values, it returns `""`.
Input
["TimeMap", "set", "get", "get", "set", "get", "get"]
[[], ["foo", "bar", 1], ["foo", 1], ["foo", 3], ["foo", "bar2", 4], ["foo", 4], ["foo", 5]]
Output
[null, null, "bar", "bar", null, "bar2", "bar2"]

Explanation
TimeMap timeMap = new TimeMap();
timeMap.set("foo", "bar", 1);  // store the key "foo" and value "bar" along with timestamp = 1.
timeMap.get("foo", 1);         // return "bar"
timeMap.get("foo", 3);         // return "bar", since there is no value corresponding to foo at timestamp 3 and timestamp 2, then the only value is at timestamp 1 is "bar".
timeMap.set("foo", "bar2", 4); // store the key "foo" and value "bar2" along with timestamp = 4.
timeMap.get("foo", 4);         // return "bar2"
timeMap.get("foo", 5);         // return "bar2"

Solution: TC: O(n log n), SC: O(n)
```python
class TimeMap:

    def __init__(self):
        self.store = {} #key: list of [value, timestamps]
        

    def set(self, key: str, value: str, timestamp: int) -> None:
        if key not in self.store:#if key does not exist in dictionary
            self.store[key] = []
        self.store[key].append([value, timestamp]) #store the pair in key bucket
        

    def get(self, key: str, timestamp: int) -> str:
        res = "" #if key does not exist in dict, return empty string
        values = self.store.get(key, [])
        l, r = 0, len(values) - 1
        while l <= r:
            m = (l + r) // 2
            if values[m][1] <= timestamp:
                res = values[m][0]
                l = m + 1
            else:
                r = m - 1
        return res

        


# Your TimeMap object will be instantiated and called as such:
# obj = TimeMap()
# obj.set(key,value,timestamp)
# param_2 = obj.get(key,timestamp)
```

Median Of Two Sorted Arrays


##### 35. [Search Insert Position](https://leetcode.com/problems/search-insert-position/description/) Easy

Given a sorted array of distinct integers and a target value, return the index if the target is found. If not, return the index where it would be if it were inserted in order. `nums` contains distinct values sorted in ascending order.

You must write an algorithm with O(log n) runtime complexity.

Input: nums = [1,3,5,6], target = 5
Output: 2
Input: nums = [1,3,5,6], target = 2
Output: 1
Input: nums = [1,3,5,6], target = 7
Output: 4

Solution: 
TC: O(log n), SC: O(1)

```python
class Solution:
    def searchInsert(self, nums: List[int], target: int) -> int:
        l, r = 0, len(nums) - 1
        while l <=r:
            mid = l + (r - l) // 2
            if target == nums[mid]:
                return mid
            elif target > nums[mid]:
                l = mid + 1
            else:
                r = mid - 1
        return l
```

##### 374. [Guess Number Higher Or Lower](https://leetcode.com/problems/guess-number-higher-or-lower/description/) Easy

We are playing the Guess Game. The game is as follows:

I pick a number from 1 to n. You have to guess which number I picked.

Every time you guess wrong, I will tell you whether the number I picked is higher or lower than your guess.

You call a pre-defined API int guess(int num), which returns three possible results:

-1: Your guess is higher than the number I picked (i.e. num > pick).
1: Your guess is lower than the number I picked (i.e. num < pick).
0: your guess is equal to the number I picked (i.e. num == pick).
Return the number that I picked.

Input: n = 10, pick = 6
Output: 6
Input: n = 1, pick = 1
Output: 1
Input: n = 2, pick = 1
Output: 1

Solution: 
TC: O(log n), SC: O(1)

```python
# The guess API is already defined for you.
# @param num, your guess
# @return -1 if num is higher than the picked number
#          1 if num is lower than the picked number
#          otherwise return 0
# def guess(num: int) -> int:

class Solution:
    def guessNumber(self, n: int) -> int:
        l, r = 1, n
        while l <= r:
            m = l + (r-l) //2
            res = guess(m)
            if res > 0:
                l = m + 1
            elif res < 0:
                r = m - 1
            else:
                return m
```

##### 441. [Arranging Coins](https://leetcode.com/problems/arranging-coins/description/) Easy

You have n coins and you want to build a staircase with these coins. The staircase consists of k rows where the ith row has exactly i coins. The last row of the staircase may be incomplete.

Given the integer n, return the number of complete rows of the staircase you will build.
Input: n = 5
Output: 2
Explanation: Because the 3rd row is incomplete, we return 2.
Input: n = 8
Output: 3
Explanation: Because the 4th row is incomplete, we return 3.

Solution: 
TC: O(log n), SC: O(1)
```python
class Solution:
    def arrangeCoins(self, n: int) -> int:
        l, r = 0, n
        
        while l <= r:
            k = l + (r - l) //2
            cur = k * (k + 1) //2
            if cur == n:
                return k
            if n < cur:
                r = k - 1
            else:
                l = k + 1
        return r
```

##### 977. [Squares Of Sorted array](https://leetcode.com/problems/squares-of-a-sorted-array/description/) Easy

Given an integer array nums sorted in non-decreasing order, return an array of the squares of each number sorted in non-decreasing order. Follow up: Squaring each element and sorting the new array is very trivial, could you find an O(n) solution using a different approach?
Input: nums = [-4,-1,0,3,10]
Output: [0,1,9,16,100]
Explanation: After squaring, the array becomes [16,1,0,9,100].
After sorting, it becomes [0,1,9,16,100].
Input: nums = [-7,-3,2,3,11]
Output: [4,9,9,49,121]

Solution:
TC: O(), SC: O()
```python
class Solution:
    def sortedSquares(self, nums: List[int]) -> List[int]:
        n = len(nums)
        res = [0] * n
        l, r = 0, n-1
        while l <=r:
            left, right = abs(nums[l]), abs(nums[r])
            if left > right:
                res[r-l] = left * left
                l += 1
            else:
                res[r-l] = right * right
                r -= 1
        return res
        
```

##### 367. [Valid Perfect Square](https://leetcode.com/problems/valid-perfect-square/description/) Easy
Given a positive integer num, return true if num is a perfect square or false otherwise.

A perfect square is an integer that is the square of an integer. In other words, it is the product of some integer with itself.

You must not use any built-in library function, such as sqrt.

Input: num = 16
Output: true
Explanation: We return true because 4 * 4 = 16 and 4 is an integer.
Input: num = 14
Output: false
Explanation: We return false because 3.742 * 3.742 = 14 and 3.742 is not an integer.

Solution: 
TC: O(log n), SC: O(1)
```python
class Solution:
    def isPerfectSquare(self, num: int) -> bool:
        if num < 2:
            return True
        l, r = 2, num // 2
        while l <= r:
            x = l + (r-l) //2
            guess = x * x
            if guess == num:
                return True
            if guess > num:
                r = x - 1
            else:
                l = x + 1
        return False
```

##### 69. [Sqrt(x)](https://leetcode.com/problems/sqrtx/) Easy
Given a non-negative integer x, return the square root of x rounded down to the nearest integer. The returned integer should be non-negative as well.

You must not use any built-in exponent function or operator.

For example, do not use pow(x, 0.5) in c++ or x ** 0.5 in python.

Input: x = 4
Output: 2
Explanation: The square root of 4 is 2, so we return 2.
Input: x = 8
Output: 2
Explanation: The square root of 8 is 2.82842..., and since we round it down to the nearest integer, 2 is returned.

Solution:
TC: O(), SC: O()
```python
class Solution:
    def mySqrt(self, x: int) -> int:
        l, r = 0, x
        while l <= r:
            mid = (l + r) // 2
            if mid * mid == x:
                return mid
            if mid * mid < x:
                l = mid + 1
            else:
                r = mid - 1
        return r
```

##### 278. [First Bad Version](https://leetcode.com/problems/first-bad-version/description/) Easy

You are a product manager and currently leading a team to develop a new product. Unfortunately, the latest version of your product fails the quality check. Since each version is developed based on the previous version, all the versions after a bad version are also bad.
Suppose you have n versions [1, 2, ..., n] and you want to find out the first bad one, which causes all the following ones to be bad.
You are given an API bool isBadVersion(version) which returns whether version is bad. Implement a function to find the first bad version. You should minimize the number of calls to the API.

Input: n = 5, bad = 4
Output: 4
Explanation:
call isBadVersion(3) -> false
call isBadVersion(5) -> true
call isBadVersion(4) -> true
Then 4 is the first bad version.

Input: n = 1, bad = 1
Output: 1

Solution: TC: O(log n) since the search is halved each time; SC: O(1)
```python
# The isBadVersion API is already defined for you.
# def isBadVersion(version: int) -> bool:

class Solution:
    def firstBadVersion(self, n: int) -> int:
        l, r = 1, n
        while l < r:
            m = (l + r) // 2
            if isBadVersion(m):
                r = m
            elif not isBadVersion(m):
                l = m + 1
        return l
```
