---
{"dg-publish":true,"permalink":"/personal-vault/data-structures-and-algorithms/to-be-revisited/is-balanced-binary-tree/"}
---


### Problem Statement:
Given a binary tree, determine if it is  **height-balanced**.
A **height-balanced** binary tree is a binary tree in which the depth of the two subtrees of every node never differs by more than one.

Example 1:
![[Screenshot 2024-06-06 at 10.56.13 AM.png \| 200]]
>**Input:** root = [3,9,20,null,null,15,7]
   **Output:** true

Example 2:
![[Screenshot 2024-06-06 at 10.56.59 AM.png \| 200]]
>**Input:** root = [1,2,2,3,3,null,null,4,4]
   **Output:** false

**Example 3:**

>**Input:** root = []
**Output:** true

#### Solution:

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
    int height(TreeNode* root){
        if(root==nullptr) return 0;

        return  (1 + max(height(root->left) , height(root->right)));
    }
    bool check(TreeNode* root){
        if(root==nullptr) return true;

        int lefth = height(root->left);
        int righth = height(root->right);
		
        if( abs(lefth - righth) > 1) return false;
        
		//this automatically states that diff <= 1 ; so we check the subtrees.
        return check(root->left) and check(root->right);

        
    }
public:
    bool isBalanced(TreeNode* root) {
        return check(root);
    }
};
```
