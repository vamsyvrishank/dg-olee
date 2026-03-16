---
{"dg-publish":true,"permalink":"/personal-vault/dsa/to-be-revisited/same-tree/"}
---


### Problem Statement

Given the roots of two binary trees `p` and `q`, write a function to check if they are the same or not.

Two binary trees are considered the same if they are structurally identical, and the nodes have the same value.

>**Example 1:**
![[Screenshot 2024-06-06 at 9.46.32 PM.png \| 300]]
**Input:** p = [1,2,3], q = [1,2,3]
**Output:** true

>**Example 2:**
![[Screenshot 2024-06-06 at 9.46.53 PM.png \| 300]]
**Input:** p = [1,2], q = [1,null,2]
**Output:** false


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
public:
    bool isSameTree(TreeNode* p, TreeNode* q) {

        if(p==nullptr and q==nullptr) return true;
        if(p==nullptr || q==nullptr) return false;

        return ( (p->val==q->val) and isSameTree(p->left , q->left) and isSameTree(p->right , q->right) );
        
    }
};

```