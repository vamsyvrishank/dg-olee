---
{"dg-publish":true,"permalink":"/personal-vault/dsa/to-be-revisited/diameter-of-a-binary-tree/"}
---


### Problem Statement:
Given the `root` of a binary tree, return _the length of the **diameter** of the tree_.

The **diameter** of a binary tree is the **length** of the longest path between any two nodes in a tree. This path may or may not pass through the `root`.

The **length** of a path between two nodes is represented by the number of edges between them.

>**Example 1:**
![[Screenshot 2024-06-06 at 12.05.54 PM.png \| 200]]
>
**Input:** root = [1,2,3,4,5]
**Output:** 3
**Explanation:** 3 is the length of the path [4,2,1,3] or [5,2,1,3].

>
**Example 2:**
**Input:** root = [1,2]
**Output:** 1

**Constraints:**

- The number of nodes in the tree is in the range `[1, 104]`.
- `-100 <= Node.val <= 100`

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
    int height(TreeNode* root ,  int &ans){
        if(root==nullptr) return 0;

        int lefth = height(root->left , ans);
        int righth = height(root->right , ans);

        ans = max(ans , lefth + righth);

        return 1 + max(lefth , righth);
    }
public:
    int diameterOfBinaryTree(TreeNode* root) {
        if(root==nullptr) return 0;

        int ans = INT_MIN;

        height(root , ans);

        return ans;
    }
};

```