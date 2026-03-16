---
{"dg-publish":true,"permalink":"/personal-vault/data-structures-and-algorithms/to-be-revisited/binary-tree-paths/"}
---


Given the `root` of a binary tree, return _all root-to-leaf paths in **any order**_.

A **leaf** is a node with no children.

**Example 1:**

![[Pasted image 20240608131041.png \| 300]]
**Input:** root = [1,2,3,null,5]
**Output:** ["1->2->5","1->3"]

**Example 2:**

**Input:** root = [1]
**Output:** ["1"]

**Constraints:**

- The number of nodes in the tree is in the range `[1, 100]`.
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
    void traversal(TreeNode* root , string temp , vector<string> &ans ){
        if (root ==nullptr) return;

        if(root->left==nullptr and root->right ==nullptr){
            temp+= to_string(root->val);
            ans.push_back(temp);
        }

        temp+=to_string(root->val) + "->";
        traversal(root->left , temp , ans);
        traversal(root->right, temp , ans);
    }
public:
    vector<string> binaryTreePaths(TreeNode* root) {

        string temp ;
        vector<string>ans;
        traversal(root , temp , ans);

        return ans;
        
    }
};
```