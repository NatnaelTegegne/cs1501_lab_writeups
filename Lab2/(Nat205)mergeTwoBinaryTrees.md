Implementaion (Nat205)

```
# Definition for a binary tree node.
# class TreeNode:
#     def __init__(self, val=0, left=None, right=None):
#         self.val = val
#         self.left = left
#         self.right = right
class Solution:
    def mergeTrees(self, root1: TreeNode | None, root2: TreeNode | None) -> TreeNode | None:
        if(root1 == None):
            return root2
        elif(root2 == None):
            return root1
        else:
            new_root = TreeNode(root1.val + root2.val)
            new_root.left = self.mergeTrees(root1.left, root2.left)
            new_root.right = self.mergeTrees(root1.right, root2.right)
            return new_root
        
```

## Description
The goal of the problem was to merge two binary trees. To do this, we need to consider what the edge cases are and make sure to choose an approach that can easly tackle those edge cases. We can see here we are doing repeted work over and over again, which is going down the left or the right side of the tree and adding the values. For this a good approach is using a recurssive method. This helps us to keep track of the state of the pointers easily as we build up the new tree. The edge cases are:
- Both trees can be empty, or null
- Either can be empty and the other could have nodes
- at each node, both tree might have a child, or just one of them have a child or either has a child

Keeping these in mind, first my implementation checks if root1 (the current node we are checking) is empty, if so we return the second one as our answer. We do the same for root2.
Other wise, we make a new root and use a different variable called new_root (to make sure we don't re-assign the new node to one of the curr nodes (which I did in my first implementation), this will mess up our already existing tree). Then recurssively merge the two trees and assign them to our new_root over and over again. Finally return our new_root.

## Analysis
This implementation have different run times based on which if else statement gets executed. If the first two (the base cases) gets executed, we are not doing anywork, we are just returning the remaining subtree which is just ann O(1) operation. If the last one gets executed, that is where we are doing work by calling a function repetedly. So that will take O(N) time, w/r N is the number of overlapping nodes in both trees. Therefore, the runtime is O(N)