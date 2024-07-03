# Linked List

## Resources

[TechInterviewHandbook](https://www.techinterviewhandbook.org/algorithms/linked-list/)


### Techniques and edge cases:


#### Leetcode Problems and Solutions:

##### 206. [Reverse Linked List](https://leetcode.com/problems/reverse-linked-list/description/) Easy

Given the head of a singly linked list, reverse the list, and return the reversed list. A linked list can be reversed either iteratively or recursively. Could you implement both?

Input: head = [1,2,3,4,5]
Output: [5,4,3,2,1]

Input: head = []
Output: []

Solution: 
TC: O(n), SC: O(1), if done iteratively
TC: O(n), SC: O(n), if done recursively.

```python
# using iterative function.
class Solution:
    def reverseList(self, head: Optional[ListNode]) -> Optional[ListNode]:
        pre, curr = None, head
        while curr:  #is not null
            nxt = curr
            curr = curr.next
            curr.next = prev
            prev = nxt
        return prev #return from the head

# using recursive function.
class Solution:
    def reverseList(self, head: Optional[ListNode]) -> Optional[ListNode]:
        if not head: #if head is empty
            return None
        newHead = head #new variable to store
        if head.next:
            newHead = self.reverseList(head.next) #recursive function
            head.next.next = head #reverse
        head.next = None
        return newHead
```

##### 21. [Merge Two Sorted Lists](https://leetcode.com/problems/merge-two-sorted-lists/description/) Easy

You are given the heads of two sorted linked lists `list1` and `list2`. 
Merge the two lists into one sorted list. The list should be made by splicing together the nodes of the first two lists.
Return the head of the merged linked list. Both list1 and list2 are sorted in non-decreasing order.

Input: list1 = [1,2,4], list2 = [1,3,4]
Output: [1,1,2,3,4,4]

Input: list1 = [], list2 = []
Output: []

Solution: 
TC: O(n+m), SC: O(1)
new dummy list with a dummy node and tail initialized to dummy, compare the pointers while they iterate through the lists.
```python
class Solution:
    def mergeTwoLists(self, list1: Optional[ListNode], list2: Optional[ListNode]) -> Optional[ListNode]:
        dummy = ListNode()
        tail = dummy
        while list1 and list2:
            if list1.val < list2.val:
                tail.next = list1
                list1 = list1.next
            else:
                tail.next = list2
                list2 = list2.next
            tail = tail.next #update the tail pointer.

        if list1:
            tail.next = list1
        elif list2:
            tail.next = list2
        return dummy.next
```

##### 141. [Linked List Cycle](https://leetcode.com/problems/linked-list-cycle/description/) Easy

Given `head`, the head of a linked list, determine if the linked list has a cycle in it.

There is a cycle in a linked list if there is some node in the list that can be reached again by continuously following the `next`pointer. Internally, `pos` is used to denote the index of the node that tail's `next` pointer is connected to. Note that `pos` is not passed as a parameter. 
Return `true` if there is a cycle in the linked list. Otherwise, return `false`. pos is -1 or a valid index in the linked-list.
Can you solve it using O(1) (i.e. constant) memory?

Input: head = [3,2,0,-4], pos = 1
Output: true
Explanation: There is a cycle in the linked list, where the tail connects to the 1st node (0-indexed).

Input: head = [1,2], pos = 0
Output: true
Explanation: There is a cycle in the linked list, where the tail connects to the 0th node.

Input: head = [1], pos = -1
Output: false
Explanation: There is no cycle in the linked list.

Solution:
Two pointer with slow and fast approach
TC: O(n+k) ~ O(n) since O(n) for slow while O(k) for fast, SC: O(1)

```python
class Solution:
    def hasCycle(self, head: Optional[ListNode]) -> bool:
        slow, fast = head, head
        while fast and fast.next:
            slow = slow.next
            fast = fast.next.next
            if slow == fast:
                return True
        return False
```

##### 143. [Reorder List](https://leetcode.com/problems/reorder-list/description/) Medium

You are given the head of a singly linked-list. The list can be represented as:

L0 → L1 → … → Ln - 1 → Ln
Reorder the list to be on the following form:

L0 → Ln → L1 → Ln - 1 → L2 → Ln - 2 → …
You may not modify the values in the list's nodes. Only nodes themselves may be changed.

Input: head = [1,2,3,4]
Output: [1,4,2,3]

Input: head = [1,2,3,4,5]
Output: [1,5,2,4,3]

Solution: 
two pointer slow/fast approach
TC: O(n), SC: O(1)
```python
class Solution:
    def reorderList(self, head: Optional[ListNode]) -> None:
        slow, fast = head, head.next
        while fast and fast.next:
            slow = slow.next
            fast = fast.next.next

        second = slow.next #at the beginning of the second pointer. 
        prev = slow.next = None#new temp var
        while second:
            tmp = second.next
            second.next = prev
            prev = second #update pointer
            second = tmp #next node in the list
        #merge two halfs of the list
        first, second = head, prev
        while second:
            tmp1, tmp2 = first.next, second.next
            first.next = second
            second.next = tmp1
            first, second = tmp1, tmp2

```



Remove Nth node from end of the list

Copy List With Random Pointer

Add Two Numbers



Find The Duplicate Number

LRU Cache

Merge K Sorted Lists

Reverse Nodes In K Group


