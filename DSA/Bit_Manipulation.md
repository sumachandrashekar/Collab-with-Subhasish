# Bit Manipulation

## Resources

[TechInterviewHandbook](https://www.techinterviewhandbook.org/algorithms/binary/)


### Techniques and edge cases:


#### Leetcode Problems and Solutions:

#### 136. [Single Number](https://leetcode.com/problems/single-number/description/) Easy

Given a non-empty array of integers nums, every element appears twice except for one. Find that single one.
You must implement a solution with a linear runtime complexity and use only constant extra space. Each element in the array appears twice except for one element which appears only once.
Input: nums = [2,2,1]
Output: 1
Input: nums = [4,1,2,1,2]
Output: 4
Input: nums = [1]
Output: 1

Solution: 
TC: O(n), SC: O(1); use hashset if SC: O(n) is allowed, else below solution for O(1)
```python
class Solution:
    def singleNum(self, nums: List[int]) -> int:
        res = 0 
        for i in nums:
            res = i ^ res #the odd/single number will be different when XOR'ed
        return res
```

#### 191. [Number of 1 Bits](https://leetcode.com/problems/number-of-1-bits/description/) Easy
Write a function that takes the binary representation of a positive integer and returns the number of 
set bits  it has (also known as the Hamming weight).
Input: n = 11
Output: 3
Explanation: The input binary string 1011 has a total of three set bits.
Input: n = 128
Output: 1
Explanation: The input binary string 10000000 has a total of one set bit.
Input: n = 2147483645
Output: 30
Explanation:The input binary string 1111111111111111111111111111101 has a total of thirty set bits.

Solution: 
TC: O(), SC: O()
```python
class Solution:
    def hammingWeight(self, n: int) -> int:
        res = 0
        while n:
            res += n % 2
            n = n >> 1
        return res
```

#### 338. [Counting Bits](https://leetcode.com/problems/counting-bits/description/) Easy

Given an integer `n`, return an array `ans` of length `n + 1` such that for each `i (0 <= i <= n)`, `ans[i]` is the number of 1's in the binary representation of `i`. Can you do it in linear time O(n) and possibly in a single pass?
Input: n = 2
Output: [0,1,1]
Explanation:
0 --> 0
1 --> 1
2 --> 10
Input: n = 5
Output: [0,1,1,2,1,2]
Explanation:
0 --> 0
1 --> 1
2 --> 10
3 --> 11
4 --> 100
5 --> 101

Solution: 
TC: O(), SC: O()

```python
class Solution:
    def countBits(self, n: int) -> List[int]:
        ans = [0] * (n + 1)
        for i in range(1, n + 1):
            ans[i] = ans[i & (i - 1)] + 1
        return ans
```

#### 190. [Reverse Bits](https://leetcode.com/problems/reverse-bits/description/) Easy
Reverse bits of a given 32 bits unsigned integer.
Input: n = 00000010100101000001111010011100
Output:    964176192 (00111001011110000010100101000000)
Explanation: The input binary string 00000010100101000001111010011100 represents the unsigned integer 43261596, so return 964176192 which its binary representation is 00111001011110000010100101000000.

Solution: 
TC: O(1), SC: O(1)

```python
class Solution:
    def reverseBits(self, n: int) -> int:
        res = 0
        for i in range(32):
            bit = (n >> i) & 1
            res += (bit << (31 - i))
        return res
```

#### 268. [Missing Number](https://leetcode.com/problems/missing-number/description/) Easy
Given an array `nums` containing `n` distinct numbers in the range `[0, n]`, return the only number in the range that is missing from the array.
Input: nums = [3,0,1]
Output: 2
Explanation: n = 3 since there are 3 numbers, so all numbers are in the range [0,3]. 2 is the missing number in the range since it does not appear in nums.
Input: nums = [9,6,4,2,3,5,7,0,1]
Output: 8
Explanation: n = 9 since there are 9 numbers, so all numbers are in the range [0,9]. 8 is the missing number in the range since it does not appear in nums.
Could you implement a solution using only O(1) extra space complexity and O(n) runtime complexity?

Solution:
TC: O(), SC: O()

```python
class Solution:
    def missingNumber(self, nums: List[int]) -> int:
        missing = len(nums)
        for i, num in enumerate(nums):
            missing ^= i ^ num
        return missing
```

#### 371. [Sum Of Two Integers](https://leetcode.com/problems/sum-of-two-integers/description/) Medium
Given two integers `a` and `b`, return the sum of the two integers without using the operators `+` and `-`.
Input: a = 1, b = 2
Output: 3
Input: a = 2, b = 3
Output: 5
Using XOR
```python
class Solution:
    def getSum(self, a: int, b: int) -> int:
        def add(a, b):
            if not a or not b:
                return a or b
            return add(a ^ b, (a & b) << 1)

        if a * b < 0:  # assume a < 0, b > 0
            if a > 0:
                return self.getSum(b, a)
            if add(~a, 1) == b:  # -a == b
                return 0
            if add(~a, 1) < b:  # -a < b
                return add(~add(add(~a, 1), add(~b, 1)), 1)  # -add(-a, -b)

        return add(a, b)  # a*b >= 0 or (-a) > b > 0
```
#### 7. [Reverse Integer](https://leetcode.com/problems/reverse-integer/description/) Medium

Given a signed 32-bit integer `x`, return `x` with its digits reversed. If reversing `x` causes the value to go outside the signed 32-bit integer range `[-231, 231 - 1]`, then return `0`.

Assume the environment does not allow you to store 64-bit integers (signed or unsigned).
Input: x = 123
Output: 321
Input: x = -123
Output: -321
Input: x = 120
Output: 21

Solution: 
TC: O(), SC: O()

```python
class Solution:
    def reverse(self, x: int) -> int:
        MIN = pow(-2, 31)  # -2^31,
        MAX = pow(2, 31)-1  #  2^31 - 1

        res = 0
        while x:
            digit = int(math.fmod(x, 10))  # (python dumb) -1 %  10 = 9
            x = int(x / 10)  # (python dumb) -1 // 10 = -1

            if res > MAX // 10 or (res == MAX // 10 and digit > MAX % 10):
                return 0
            if res < MIN // 10 or (res == MIN // 10 and digit < MIN % 10):
                return 0
            res = (res * 10) + digit

        return res
```


#### 67. [Add Binary](https://leetcode.com/problems/add-binary/description/) Easy
Given two binary strings a and b, return their sum as a binary string. Each string does not contain leading zeros except for the zero itself.
Input: a = "11", b = "1"
Output: "100"
Input: a = "1010", b = "1011"
Output: "10101"

Solution: 
TC: O(), SC: O()
```python taken from editorial of the problem
class Solution:
    def addBinary(self, a: str, b: str) -> str:
        x, y = int(a, 2), int(b, 2)
        while y:
            x, y = x ^ y, (x & y) << 1
        return bin(x)[2:]
```
