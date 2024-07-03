# Math & Geo

## Resources

[TechInterviewHandbook](https://www.techinterviewhandbook.org/algorithms/math/) and (https://www.techinterviewhandbook.org/algorithms/geometry/)


### Techniques and edge cases:

Math: 
    - if code has division or modulo, check for 0 case
    - Multiplication by 1
    - check and handle overflow/underflow
    - consider negative and floating point numbers

    Number is even: num % 2 == 0
    Sum of 1 to N = ((N+1)*N )/2
    Sum of Geometric progression = 2^(n+1) - 1
    Permutations of N = N! /(N-K)!
    Combinations of N = N! /(K! * (N-K)!)

    Techniques:
    multiples of a number: modulos operator
    comparing floats: use epsilon comparisons like abs(x - y) <= 1e-6 instead of x == y
    if we need to implement an operator like power, square root or division and be faster than O(n), go for either doubling(fast exponentiation) or halving(binary search). 

Geo: 
- check for zero edge cases
    Techniques:
    - Distance between two points: use dx^2 + dy^2(dont need square roots)
    - overlapping circles: check if distance between two centres of the circle is less than sum of their radii
    - overlapping rectangles: overlap is true when:
        ```python
            overlap = rect_a.left < rect_b.right and \
            rect_a.right > rect_b.left and \
            ect_a.top > rect_b.bottom and \
            rect_a.bottom < rect_b.top
            ```

#### Leetcode Problems and Solutions:

##### 202. [Happy Number](https://leetcode.com/problems/happy-number/) Easy
Write an algorithm to determine if a number `n` is happy.

A happy number is a number defined by the following process:

Starting with any positive integer, replace the number by the sum of the squares of its digits.
Repeat the process until the number equals 1 (where it will stay), or it loops endlessly in a cycle which does not include 1.
Those numbers for which this process ends in 1 are happy.
Return `true` if n is a happy number, and `false` if not.
Input: n = 19
Output: true
Explanation:
12 + 92 = 82
82 + 22 = 68
62 + 82 = 100
12 + 02 + 02 = 1
Input: n = 2
Output: false

Solution: 
TC: O(), SC: O(n)
```python
class Solution:
    def isHappy(self, n: int) -> bool:
        visit = set() #define a hashset
        while n not in visit:
            visit.add(n)
            n = self.SumOfSquares(n)
            if n == 1:
                return True
        return False

    def SumOfSquares(self, n: int) -> int:
        output = 0
        while n:
            digit = n % 10
            digit = digit ** 2
            output += digit
            n = n // 10 #move to next number
        return output
```

##### 66. [Plus One](https://leetcode.com/problems/plus-one/description/) Easy
You are given a large integer represented as an integer array `digits`, where each `digits[i]` is the `ith` digit of the integer. The digits are ordered from most significant to least significant in left-to-right order. The large integer does not contain any leading 0's.

Increment the large integer by one and return the resulting array of digits.

Input: digits = [1,2,3]
Output: [1,2,4]
Explanation: The array represents the integer 123.
Incrementing by one gives 123 + 1 = 124.
Thus, the result should be [1,2,4].

Input: digits = [4,3,2,1]
Output: [4,3,2,2]
Explanation: The array represents the integer 4321.
Incrementing by one gives 4321 + 1 = 4322.
Thus, the result should be [4,3,2,2].

Input: digits = [9]
Output: [1,0]
Explanation: The array represents the integer 9.
Incrementing by one gives 9 + 1 = 10.
Thus, the result should be [1,0].

Solution: 
TC: O(), SC: O()

```python

```

##### 9. [Palindrome Number](https://leetcode.com/problems/palindrome-number/description/) Easy
Given an integer x, return true if x is a palindrome, and false otherwise.
Could you solve it without converting the integer to a string?
Input: x = 121
Output: true
Explanation: 121 reads as 121 from left to right and from right to left.

Input: x = 10
Output: false
Explanation: Reads 01 from right to left. Therefore it is not a palindrome.

Input: x = -121
Output: false
Explanation: From left to right, it reads -121. From right to left, it becomes 121-. Therefore it is not a palindrome

