---
{"dg-publish":true,"permalink":"/personal-vault/dsa/to-be-revisited/path-sum-ii/"}
---


Given the `root` of a binary tree and an integer `targetSum`, return _all **root-to-leaf** paths where the sum of the node values in the path equals_ `targetSum`_. Each path should be returned as a list of the node **values**, not node references_.

A **root-to-leaf** path is a path starting from the root and ending at any leaf node. A **leaf** is a node with no children.

**Example 1:**

![[Pasted image 20240608133524.png \| 400]]

**Input:** root = [5,4,8,11,null,13,4,7,2,null,null,5,1], targetSum = 22
**Output:** [[5,4,11,2],[5,8,4,5\|5,4,11,2],[5,8,4,5]]
**Explanation:** There are two paths whose sum equals targetSum:
5 + 4 + 11 + 2 = 22
5 + 8 + 4 + 5 = 22

**Example 2:**

![[Pasted image 20240608133540.png \| 400]]

**Input:** root = [1,2,3], targetSum = 5
**Output:** []

**Example 3:**

**Input:** root = [1,2], targetSum = 0
**Output:** []

**Constraints:**

- The number of nodes in the tree is in the range `[0, 5000]`.
- `-1000 <= Node.val <= 1000`
- `-1000 <= targetSum <= 1000`



Do the same code as path sum , return bool and stuff but don't save it , do the computation to calculate
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
    bool traversal(TreeNode* root , int targetSum , vector<vector<int>> &ans, vector<int> temp) {
        if(root==nullptr and targetSum !=0) return false;
        if(root==nullptr) return false;

        targetSum = targetSum - root->val;
        temp.push_back(root->val);
        if(targetSum==0 and root->left ==nullptr and root->right==nullptr){
            ans.push_back(temp);
            return true;
        }

        bool left = traversal(root->left , targetSum , ans, temp);
        bool right = traversal(root->right , targetSum , ans , temp);

        return left || right ;
    }
public:
    vector<vector<int>> pathSum(TreeNode* root, int targetSum) {

        if(root==nullptr) return {};

        vector<vector<int>> ans;
        vector<int> temp;
        traversal(root , targetSum , ans , temp);

        return ans;
        
    }
};
```