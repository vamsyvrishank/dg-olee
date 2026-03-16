---
{"dg-publish":true,"permalink":"/personal-vault/dsa/to-be-revisited/minimum-depth-of-binary-tree/"}
---


Given a binary tree, find its minimum depth.

The minimum depth is the number of nodes along the shortest path from the root node down to the nearest leaf node.

**Note:** A leaf is a node with no children.

**Example 1:**

![[Pasted image 20240607202254.png \| 600]]

**Input:** root = [3,9,20,null,null,15,7]
**Output:** 2

**Example 2:**

**Input:** root = [2,null,3,null,4,null,5,null,6]
**Output:** 5

**Constraints:**

- The number of nodes in the tree is in the range `[0, 105]`.
- `-1000 <= Node.val <= 1000`


```cpp
/**
 * Definition for a binary tree node.
 * struct TreeNode {
 *     int val;
 *     TreeNode *left;
 *     TreeNode *right;
 *     TreeNode() : val(0), left(nullptr), right(nullptr) {}
 *     TreeNode(int x) : val(x), left(nullptr), right(nullptr) {}
 *     TreeNode(int x, TreeNode *left, TreeNode *right) : val(x), left(left), right(right) {}
 * };
 */
class Solution {
public:
    int minDepth(TreeNode* root) {
        if(root==NULL) return 0;
        if(root->left==NULL && root->right==NULL) return 1;
        
        int leftT = minDepth(root->left);
        int rightT = minDepth(root->right);
        if(root->right==NULL) return (1+leftT);
        if(root->left==NULL) return (1+rightT);
        return 1 + min(leftT , rightT);
        
    }
};

```