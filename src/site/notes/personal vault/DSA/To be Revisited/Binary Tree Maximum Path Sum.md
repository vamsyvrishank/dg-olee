---
{"dg-publish":true,"permalink":"/personal-vault/dsa/to-be-revisited/binary-tree-maximum-path-sum/"}
---

### Problem Statement:
A **path** in a binary tree is a sequence of nodes where each pair of adjacent nodes in the sequence has an edge connecting them. A node can only appear in the sequence **at most once**. Note that the path does not need to pass through the root.

The **path sum** of a path is the sum of the node's values in the path.

Given the `root` of a binary tree, return _the maximum **path sum** of any **non-empty** path_.

>**Example 1:**
![[Screenshot 2024-06-06 at 9.25.22 PM.png \| 200]]
**Input:** root = [1,2,3]
**Output:** 6
**Explanation:** The optimal path is 2 -> 1 -> 3 with a path sum of 2 + 1 + 3 = 6.


>**Example 2:**
![[Screenshot 2024-06-06 at 9.25.42 PM.png \| 200]]
**Input:** root = [-10,9,20,null,null,15,7]
**Output:** 42
**Explanation:** The optimal path is 15 -> 20 -> 7 with a path sum of 15 + 20 + 7 = 42.

**Constraints:**

- The number of nodes in the tree is in the range `[1, 3 * 104]`.
- `-1000 <= Node.val <= 1000`


#### Solution:
![[MaxPATHSum.jpg \| 300]]
```java
class Solution {
    // Main method to find the maximum path sum
    public int maxPathSum(TreeNode root) {
        int[] maxSum = {Integer.MIN_VALUE};
        maxPathSum(root, maxSum);
        return maxSum[0];
    }

    private int maxPathSum(TreeNode root, int[] maxSum) {
        if (root == null) {
            return 0;
        }

        // Recursively get the maximum path sums for the left and right subtrees
        // Only consider positive sums, as negative sums would decrease the overall sum
        int leftSum = Math.max(0, maxPathSum(root.left, maxSum));
        int rightSum = Math.max(0, maxPathSum(root.right, maxSum));

        // Calculate the maximum path sum for the current node
        // Option 1: Only the current node's value
        // Option 2: Current node's value plus the maximum of left or right subtree sums
        int temp = Math.max(root.val, root.val + Math.max(leftSum, rightSum));

        // Calculate the maximum path sum including both left and right subtrees
        // Option 3: Current node's value plus both left and right subtree sums
        int ans = Math.max(temp, root.val + leftSum + rightSum);

        // Update the global maximum path sum if the current path sum is greater
        maxSum[0] = Math.max(maxSum[0], ans);

        // Return the maximum path sum that can be extended to the parent node
        return temp;
    }
}
```