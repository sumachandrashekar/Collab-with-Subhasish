# Trees

## Resources

[TechInterviewHandbook](https://www.techinterviewhandbook.org/algorithms/tree/)


### Techniques and edge cases:


#### Leetcode Problems and Solutions:

#### 226. [Invert Binary Tree](https://leetcode.com/problems/invert-binary-tree/description/) Easy

Given the root of a binary tree, invert the tree, and return its root.
Input: root = [4,2,7,1,3,6,9]
Output: [4,7,2,9,6,3,1]
Input: root = []
Output: []
Input: root = [4,2,7,1,3,6,9]
Output: [4,7,2,9,6,3,1]

Solution: 
TC: O(n), SC: O(n)
```python
# Recursive approach
class Solution:
    def invertTree(self, root: Optional[TreeNode]) -> Optional[TreeNode]:
        if not root: #if empty
            return None
        tmp = root.left
        root.left = root.right
        root.right = tmp

        self.invertTree(root.left) #recursion at left side from root
        self.invertTree(root.right) #recursion at right side from root
        return root

# Iterative approach:

```




#### 104. [Maximum Depth Of Binary Tree](https://leetcode.com/problems/maximum-depth-of-binary-tree/description/) Easy
Given the root of a binary tree, return its maximum depth.

A binary tree's maximum depth is the number of nodes along the longest path from the root node down to the farthest leaf node.
Input: root = [3,9,20,null,null,15,7]
Output: 3
Input: root = [1,null,2]
Output: 2

Solution:
 TC: O(n), SC: O(n) for worst case scenario(completely unbalance-each node has only one child node). Best case: O(log n) when tree is completely balanced, the height would be log(n). All three methods have the same TC and SC.

```python
# Recursion Depth For Search(DFS)
class Solution:
    def maxDepth(self, root: Optional[TreeNode]) -> int:
        if not root:
            return 0
        return 1 + max(self.maxDepth(root.left), self.maxDepth(root.right)) #root + max length of either left or right node

# Breath For Search(BFS): queue/dequq-level by level
class Solution: 
    def maxDepth(self, root: Optional[TreeNode]) -> int:
        if not root: #if empty
            return 0
        level = 0
        q = deque([root]) #take the root node as initial deque value
        while q: #loop through till q is empty
            for i in range(length(q)): #take snapshot
                node = q.pop(left) 
                if node.left: #if not null
                    q.append(node.left) #append its children
                if node.right:
                    q.append(node.right)
            level += 1 #level increment counter.
        return level

# Iterative Depth For Search(DFS): 
class Solution: 
    def maxDepth(self, root: Optional[TreeNode]) -> int:
        if not root: #if empty
            return 0
        level = 0
        stack = [[root], 1]
        res = 1
        while stack:
            node, depth = stack.pop()
            if node: #is not null
                res = max(res, depth)
                stack.append([node.left, depth + 1]) # add children
                stack.append([node.right, depth + 1])
        return res
```

#### 543. [Diameter Of Binary Tree](https://leetcode.com/problems/diameter-of-binary-tree/description/) Easy

Given the root of a binary tree, return the length of the diameter of the tree.

The diameter of a binary tree is the length of the longest path between any two nodes in a tree. This path may or may not pass through the root.

The length of a path between two nodes is represented by the number of edges between them.
Input: root = [1,2,3,4,5]
Output: 3
Explanation: 3 is the length of the path [4,2,1,3] or [5,2,1,3].
Input: root = [1,2]
Output: 1






Balanced Binary Tree

Same Tree

Subtree Of Another Tree

Lowest Common Ancestor Of A Binary Search Tree

Binary Tree Level Order Traversal

Binary Tree Right Side View

Count Good Nodes In Binary Tree

Validate Binary Search Tree 

Kth Smallest Element In a BST

Construct Binary Tree From Preorder and Inorder Traversal 

Binary Tree Maximum Path Sum

Serialize and Deserialize Binary Tree

