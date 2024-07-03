# Stack

## Resources

[TechInterviewHandbook](https://www.techinterviewhandbook.org/algorithms/stack/)


### Techniques and edge cases:


#### Leetcode Problems and Solutions:

##### 20. [Valid Parantheses](https://leetcode.com/problems/valid-parentheses/description/) Easy

Given a string s containing just the characters '(', ')', '{', '}', '[' and ']', determine if the input string is valid.

An input string is valid if:

Open brackets must be closed by the same type of brackets.
Open brackets must be closed in the correct order.
Every close bracket has a corresponding open bracket of the same type.

Input: s = "()"
Output: true

Input: s = "(]"
Output: false

Solution: TC: O(n), SC: O(1)

```python
class Solution: 
    def isValid(self, s: str) -> bool:
        Map = {")": "(", "}":"{", ")":"("} #map the closing with the opening, since you need to pop from the top
        stack = [] #initialize an empty array to store the parentheses

        for i in s:
            if i not in Map:
                stack.append(i)
                continue
            if not stack or stack[-1] != Map(c):
                return False
            stack.pop()

        return not stack
```


##### 155. [Min Stack](https://leetcode.com/problems/min-stack/description/) Medium

Design a stack that supports push, pop, top, and retrieving the minimum element in constant time.

Implement the `MinStack` class:

`MinStack()` initializes the stack object.
`void push(int val)` pushes the element val onto the stack.
`void pop()` removes the element on the top of the stack.
`int top()` gets the top element of the stack.
`int getMin()` retrieves the minimum element in the stack.
You must implement a solution with O(1) time complexity for each function.

Input
["MinStack","push","push","push","getMin","pop","top","getMin"]
[[],[-2],[0],[-3],[],[],[],[]]

Output
[null,null,null,null,-3,null,0,-2]

Solution: TC: O(n), SC: O(1)

```python
class Solution:
    def __init__(self):
        self.stack = []
        self.minStack = []

    def push(self, val: int) -> None:
        self.stack.append(val)
        val = min(val, self[minStack[-1] if self.minStack else val])
        self.minStack.append(val)

    def pop(self) -> None:
        self.stack.pop()
        self.minStack.pop()

    def top(self) -> int:
        return self.stack[-1]
    
    def getMin(self) -> int:
        return self.minStack[-1]

```

##### 150. [Evaluate Reverse Polish Notation](https://leetcode.com/problems/evaluate-reverse-polish-notation/description/) Medium

You are given an array of strings tokens that represents an arithmetic expression in a Reverse Polish Notation.
Evaluate the expression. Return an integer that represents the value of the expression.

The valid operators are '+', '-', '*', and '/'. There will not be any division by zero.

Input: tokens = ["2","1","+","3","*"]
Output: 9
Explanation: ((2 + 1) * 3) = 9

Input: tokens = ["4","13","5","/","+"]
Output: 6
Explanation: (4 + (13 / 5)) = 6

Input: tokens = ["10","6","9","3","+","-11","*","/","*","17","+","5","+"]
Output: 22
Explanation: ((10 * (6 / ((9 + 3) * -11))) + 17) + 5
= ((10 * (6 / (12 * -11))) + 17) + 5
= ((10 * (6 / -132)) + 17) + 5
= ((10 * 0) + 17) + 5
= (0 + 17) + 5
= 17 + 5
= 22

Solution: TC: O(n), SC: O(n)

```python
class Solution:
    def evalRPN(self, tokens: List[str]) -> int:
        stack = []
        for i in tokens:
            if i == '+':
                stack.append(stack.pop() + stack.pop())
            elif i == '-':
                a, b = stack.pop(), stack.pop()
                stack.append(b - a)
            elif i == '*':
                stack.append(stack.pop() * stack.pop())
            elif i == '/':
                a, b = stack.pop(), stack.pop()
                stack.append(int(b/a))
            else:
                stack.append(int(i))
        return stack[0]
```


##### 22. [Generate Parentheses](https://leetcode.com/problems/generate-parentheses/description/) Medium

Given n pairs of parentheses, write a function to generate all combinations of well-formed parentheses.

Input: n = 3
Output: ["((()))","(()())","(())()","()(())","()()()"]
Input: n = 1
Output: ["()"]

Solution: TC: O(n), SC: O(n)
```python
class Solution:
    def generateParenthesis(self, n: int) -> List[str]:
        stack = [] #holds paretheses
        res = [] #list of valid parentheses combinations

        def backtrack(openN, closeN): #done recursively
            if openN == closeN == n:
                res.append("".join(stack)) #take every element together and join into one string
                return

            if openN < n: #add only if open < count(n)
                stack.append("(")
                backtrack(openN + 1, closeN)
                stack.pop() #pop to clean up
            
            if closeN < openN: #add only if close < open count
                stack.append(")")
                backtrack(openN, closeN + 1)
                stack.pop()
            
        backtrack(0,0) #initialize 0, 0
        return res
        
```

##### 739. [Daily Temperature](https://leetcode.com/problems/daily-temperatures/) Medium

Given an array of integers temperatures represents the daily temperatures, return an array answer such that answer[i] is the number of days you have to wait after the ith day to get a warmer temperature. If there is no future day for which this is possible, keep answer[i] == 0 instead.

Input: temperatures = [73,74,75,71,69,72,76,73]
Output: [1,1,4,2,1,1,0,0]
Input: temperatures = [30,40,50,60]
Output: [1,1,1,0]
Input: temperatures = [30,60,90]
Output: [1,1,0]

Solution: TC: O(n), SC: O(n)
```python
class Solution:
    def dailyTemperatures(self, temperatures: List[int]) -> List[int]:
        res = [0] * len(temperatures) #create an array of len(temp) as initialization
        stack = [] #extra memory for computation

        for i, t in enumerate(temperatures): #index, temp pairs
            while stack and t > stack[-1][0]: #while stack is not empty, the top of the stack(-1) and first value (0) of pair of temp is true
                stackT, stackI = stack.pop() #pop the pair
                res[stackI] = (i - stackI) #number of days it took to find the higher temp.
            stack.append([t,i]) #append the pairs
        return res        
```

##### 853. [Car Fleet](https://leetcode.com/problems/car-fleet/description/) Medium

There are n cars at given miles away from the starting mile 0, traveling to reach the mile target.
You are given two integer array position and speed, both of length n, where position[i] is the starting mile of the ith car and speed[i] is the speed of the ith car in miles per hour.
A car cannot pass another car, but it can catch up and then travel next to it at the speed of the slower car.
A car fleet is a car or cars driving next to each other. The speed of the car fleet is the minimum speed of any car in the fleet.
If a car catches up to a car fleet at the mile target, it will still be considered as part of the car fleet.
Return the number of car fleets that will arrive at the destination.

Input: target = 12, position = [10,8,0,5,3], speed = [2,4,1,1,3], Output: 3
Explanation:
The cars starting at 10 (speed 2) and 8 (speed 4) become a fleet, meeting each other at 12. The fleet forms at target.
The car starting at 0 (speed 1) does not catch up to any other car, so it is a fleet by itself.
The cars starting at 5 (speed 1) and 3 (speed 3) become a fleet, meeting each other at 6. The fleet moves at speed 1 until it reaches target.

Input: target = 10, position = [3], speed = [3], Output: 1
Explanation: There is only one car, hence there is only one fleet.

Input: target = 100, position = [0,2,4], speed = [4,2,1], Output: 1
Explanation:
The cars starting at 0 (speed 4) and 2 (speed 2) become a fleet, meeting each other at 4. The car starting at 4 (speed 1) travels to 5.
Then, the fleet at 4 (speed 2) and the car at position 5 (speed 1) become one fleet, meeting each other at 6. The fleet moves at speed 1 until it reaches target.

Solution: TC: O(n log n) due to sorting operation, SC: O(n) due to stack. 
```python
class Solution:
    def carFleet(self, target: int, position: List[int], speed: List[int]) -> int:
        pair = [[p, s] for p, s in zip(position, speed)] #create an array of pairs using zip function
        stack = [] #ans, car fleets we will have in the end

        for p, s in sorted(pair)[::-1]: #sort the array of pairs and iterate in reverse order
            stack.append((target - p) / s)
            if len(stack) >= 2 and stack[-1] <= stack[-2]: #if stack has atleast two elements and the top of the stack collides with the next car before reaching the destination, then pop
                stack.pop()
        return len(stack)
```


##### 84. [Largest Rectangle in a Histogram](https://leetcode.com/problems/largest-rectangle-in-histogram/description/) Hard

Given an array of integers heights representing the histogram's bar height where the width of each bar is 1, return the area of the largest rectangle in the histogram.

Input: heights = [2,1,5,6,2,3]
Output: 10
Explanation: The above is a histogram where width of each bar is 1.
The largest rectangle is shown in the red area, which has an area = 10 units.
Input: heights = [2,4]
Output: 4

Solution: TC = SC = O(n)
```python
class Solution:
    def largestRectangleArea(self, heights: List[int]) -> int:
        maxArea = 0
        stack = []

        for i, h in enumerate(heights):
            start = i
            while stack and stack[-1][1] > h: #stack not empty, top value's height>height we just reached
                index, height = stack.pop()
                maxArea = max(maxArea, height * (i - index)) #check the poped value's maxarea
                start = index
            stack.append((start, h)) #add the index pushed backward

        for i, h in stack: #compute till end of histogram
            maxArea = max(maxArea, h * (len(heights) - i))
        return maxArea
```

