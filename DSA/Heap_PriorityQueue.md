# Heap And Priority Queue

## Resources

[TechInterviewHandbook](https://www.techinterviewhandbook.org/algorithms/linked-list/)


### Techniques and edge cases:


#### Leetcode Problems and Solutions:

##### 703. [Kth Largest Element In A Stream](https://leetcode.com/problems/kth-largest-element-in-a-stream/description/) Easy

Design a class to find the `kth` largest element in a stream. Note that it is the kth largest element in the sorted order, not the `kth` distinct element.

Implement `KthLargest` class:

`KthLargest(int k, int[] nums)` Initializes the object with the integer `k` and the stream of integers nums.
`int add(int val)` Appends the integer `val` to the stream and returns the element representing the `kth` largest element in the stream.

Input
["KthLargest", "add", "add", "add", "add", "add"]
[[3, [4, 5, 8, 2]], [3], [5], [10], [9], [4]]
Output
[null, 4, 5, 5, 8, 8]
KthLargest kthLargest = new KthLargest(3, [4, 5, 8, 2]);
kthLargest.add(3);   // return 4
kthLargest.add(5);   // return 5
kthLargest.add(10);  // return 5
kthLargest.add(9);   // return 8
kthLargest.add(4);   // return 8

Solution: 
TC: O(n-k log n), SC: O(n)

```python
class KthLargest:

    def __init__(self, k: int, nums: List[int]):
        self.minHeap, self.k = nums, k
        heapq.heapify(self.minHeap)
        while len(self.minHeap) > k:
            heapq.heappop(self.minHeap)
        

    def add(self, val: int) -> int:
        heapq.heappush(self.minHeap, val)
        if len(self.minHeap) > self.k:
            heapq.heappop(self.minHeap)
        return self.minHeap[0]
```


#### 1046. [Last Stone Weight](https://leetcode.com/problems/last-stone-weight/description/) Easy

You are given an array of integers `stones` where `stones[i]` is the weight of the `ith` stone.
We are playing a game with the stones. On each turn, we choose the heaviest two stones and smash them together. Suppose the heaviest two stones have weights `x` and `y` with `x <= y`. The result of this smash is:
If `x == y`, both stones are destroyed, and
If `x != y`, the stone of weight x is destroyed, and the stone of weight `y` has new weight `y - x`.
At the end of the game, there is at most one stone left.
Return the weight of the last remaining stone. If there are no stones left, return `0`

Input: stones = [2,7,4,1,8,1]
Output: 1
Explanation: 
We combine 7 and 8 to get 1 so the array converts to [2,4,1,1,1] then,
we combine 2 and 4 to get 2 so the array converts to [2,1,1,1] then,
we combine 2 and 1 to get 1 so the array converts to [1,1,1] then,
we combine 1 and 1 to get 0 so the array converts to [1] then that's the value of the last stone.

Input: stones = [1]
Output: 1

Solution:
TC: O(), SC: O()

```python
class Solution:
    def lastStoneWeight(self, stones: List[int]) -> int:
        stones = [-s for s in stones] # creating a minHeap(using -ve)
        heapq.heapify(stones) # convert into a heap
        while len(stones) > 1:
            first = heapq.heappop(stones)
            second = heapq.heappop(stones)
            if second > first:
                heapq.heappush(stones, first - second) #here since the values are -ve.
        stones.append(0)
        return abs(stones[0])
```

#### 973. [K Closest Points To Origin](https://leetcode.com/problems/k-closest-points-to-origin/description/) Medium

Given an array of `points` where `points[i] = [xi, yi]` represents a point on the X-Y plane and an integer `k`, return the `k` closest points to the origin `(0, 0)`.
The distance between two points on the X-Y plane is the Euclidean distance (i.e., `√(x1 - x2)2 + (y1 - y2)2`).
You may return the answer in any order. The answer is guaranteed to be unique (except for the order that it is in).

Input: points = [[1,3],[-2,2]], k = 1
Output: [[-2,2]]
Explanation:
The distance between (1, 3) and the origin is sqrt(10).
The distance between (-2, 2) and the origin is sqrt(8).
Since sqrt(8) < sqrt(10), (-2, 2) is closer to the origin.
We only want the closest k = 1 points from the origin, so the answer is just [[-2,2]]

Input: points = [[3,3],[5,-1],[-2,4]], k = 2
Output: [[3,3],[-2,4]]
Explanation: The answer [[-2,4],[3,3]] would also be accepted.

Solution: 
TC: O(k log n), SC: O(n)

```python
class Solution: 
    def kClosest(self, points: List[List[int]], k: int) -> List[List[int]]:
        minHeap = [] # create an empty array to add in the distances
        for x, y in points: #calculate dis
            dist = (x ** 2) + (y ** 2)
            minHeap.append([dist, x, y]) #append combination of dist and coordinates
        heapq.heapify(minHeap) #convert

        res = [] #array to store results
        while k > 0:
            dist , x, y = heapq.heappop(minHeap) #pop the min distance
            res.append([x, y])
            k -= 1 #decrement based on number of values
        return res
```

#### 215. [Kth Largest Element in an Array] (https://leetcode.com/problems/kth-largest-element-in-an-array/description/) Medium

Given an integer array nums and an integer k, return the kth largest element in the array.
Note that it is the kth largest element in the sorted order, not the kth distinct element.
Can you solve it without sorting?

Input: nums = [3,2,1,5,6,4], k = 2
Output: 5

Input: nums = [3,2,3,1,2,4,5,5,6], k = 4
Output: 4

Solution: 
TC: O(k log n), SC: O(n)

```python
class Solution:
    def findKthLargest(self, nums: List[int], k: int) -> int:
        heap = []
        for num in nums:
            heapq.heappush(heap, num)
            if len(heap) > k:
                heapq.heappop(heap)
        return heap[0]
```


#### 621. [Task Scheduler](https://leetcode.com/problems/task-scheduler/description/) Medium

You are given an array of CPU `tasks`, each represented by letters A to Z, and a cooling time, `n`. Each cycle or interval allows the completion of one task. Tasks can be completed in any order, but there's a constraint: identical tasks must be separated by at least `n` intervals due to cooling time.
​Return the minimum number of intervals required to complete all tasks.

Input: tasks = ["A","A","A","B","B","B"], n = 2

Output: 8

Explanation: A possible sequence is: A -> B -> idle -> A -> B -> idle -> A -> B.

After completing task A, you must wait two cycles before doing A again. The same applies to task B. In the 3rd interval, neither A nor B can be done, so you idle. By the 4th cycle, you can do A again as 2 intervals have passed.

Input: tasks = ["A","C","A","B","D","B"], n = 1

Output: 6

Explanation: A possible sequence is: A -> B -> C -> D -> A -> B.

With a cooling interval of 1, you can repeat a task after just one other task.

Input: tasks = ["A","A","A", "B","B","B"], n = 3

Output: 10

Explanation: A possible sequence is: A -> B -> idle -> idle -> A -> B -> idle -> idle -> A -> B.

There are only two types of tasks, A and B, which need to be separated by 3 intervals. This leads to idling twice between repetitions of these tasks.

Solution: 
TC: O(n), SC: O(n)

```python
class Solution:
    def leastInterval(self, tasks: List[str], n: int) -> int:
        count = Counter(tasks) #total counts of tasks to be done
        maxHeap = [-cnt for cnt in count.values()]
        heapq.heapify(maxHeap) 
        time = 0
        q = deque() #pair of [-cnt, idletime]
        while maxHeap or q:
            time += 1
            if not maxHeap:
                time = q[0][1]
            else:
                cnt = 1 + heapq.heappop(maxHeap)
                if cnt:
                    q.append([cnt, time + n])
            if q and q[0][1] == time:
                heapq.heappush(maxHeap, q.popleft()[0])
        return time

```