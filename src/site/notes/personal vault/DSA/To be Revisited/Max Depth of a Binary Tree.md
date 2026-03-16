---
{"dg-publish":true,"permalink":"/personal-vault/dsa/to-be-revisited/max-depth-of-a-binary-tree/"}
---


Given the `root` of a binary tree, return _its maximum depth_.

A binary tree's **maximum depth** is the number of nodes along the longest path from the root node down to the farthest leaf node.

**Example 1:**

![[Pasted image 20240607191956.png \| 600]]

**Input:** root = [3,9,20,null,null,15,7]
**Output:** 3

**Example 2:**

**Input:** root = [1,null,2]
**Output:** 2

**Constraints:**

- The number of nodes in the tree is in the range `[0, 104]`.
- `-100 <= Node.val <= 100`



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
private:
    int traversal(TreeNode* root , int ans){
        if(root==nullptr) return 0;

        int lefth = 0;
        int righth =0;
        if(root->left!=nullptr){
            lefth = 1 + traversal(root->left, ans);
        }
        if(root->right != nullptr){
            righth = 1 + traversal(root->right , ans);
        }

        ans = max(ans , max(lefth , righth));

        return ans;
    }
public:
    int maxDepth(TreeNode* root) {

        if(root==nullptr) return 0;
        int ans = 1 ;

        return traversal(root , ans);

    }
};


```