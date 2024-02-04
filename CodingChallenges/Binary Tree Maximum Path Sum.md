# 124. Binary Tree Maximum Path Sum

A path in a binary tree is a sequence of nodes where each pair of adjacent nodes in the sequence has an edge connecting them. A node can only appear in the sequence at most once. Note that the path does not need to pass through the root.

The path sum of a path is the sum of the node's values in the path.

Given the root of a binary tree, return the maximum path sum of any non-empty path.

 
![image](https://github.com/AnudeepBalla10/DoorDash/assets/153243289/6b375fed-8b51-40d3-9361-9b4cc8c1c202)

Example 1:


Input: root = [1,2,3]
Output: 6
Explanation: The optimal path is 2 -> 1 -> 3 with a path sum of 2 + 1 + 3 = 6.

```java
/**
 * Definition for a binary tree node.
 * public class TreeNode {
 *     int val;
 *     TreeNode left;
 *     TreeNode right;
 *     TreeNode() {}
 *     TreeNode(int val) { this.val = val; }
 *     TreeNode(int val, TreeNode left, TreeNode right) {
 *         this.val = val;
 *         this.left = left;
 *         this.right = right;
 *     }
 * }
 */
class Solution {
    private int maxSum;
    public int maxPathSum(TreeNode root) {
        maxSum = Integer.MIN_VALUE;
        getFromSubtree(root);
        return maxSum;
    }

    private int getFromSubtree(TreeNode root){
        if(root == null){
            return 0;
        }

        //add the path from the leftSubTree and 
        //if the path sum is -ve we can ignore it or count as 0.

        int getFromLeft = Math.max(getFromSubtree(root.left),0);

        int getFromRight = Math.max(getFromSubtree(root.right),0);
       
        // if left or right path sum are negative, they are counted
        // as 0, so this statement takes care of all four scenarios
        maxSum = Math.max(maxSum, getFromLeft+getFromRight+root.val);

        return Math.max(getFromLeft+root.val, getFromRight+root.val);
    }
}
```
