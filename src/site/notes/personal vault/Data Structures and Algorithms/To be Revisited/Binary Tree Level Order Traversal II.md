---
{"dg-publish":true,"permalink":"/personal-vault/data-structures-and-algorithms/to-be-revisited/binary-tree-level-order-traversal-ii/"}
---


Given the `root` of a binary tree, return _the bottom-up level order traversal of its nodes' values_. (i.e., from left to right, level by level from leaf to root).

**Example 1:**


![[Pasted image 20240607172824.png \| 600]]
**Input:** root = [3,9,20,null,null,15,7]
**Output:** [[15,7],[9,20],[3\|15,7],[9,20],[3]]

**Example 2:**

**Input:** root = [1]
**Output:** [[1\|1]]

**Example 3:**

**Input:** root = []
**Output:** []

**Constraints:**

- The number of nodes in the tree is in the range `[0, 2000]`.
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
    vector<vector<int>> levelOrderBottom(TreeNode* root) {

        if (root == nullptr) return {};

        vector<vector<int>> ans;
        queue<TreeNode* > q; 

        q.push(root);

        while(!q.empty()){
            
            vector<int> v;
            int size = q.size();

            for(int i =0 ; i <size ;i++){

                TreeNode* temp = q.front();
                v.push_back(temp->val);
                q.pop();
                if( temp->left !=nullptr) q.push(temp->left);
                if(temp->right !=nullptr) q.push(temp->right);
            }
            ans.push_back(v);
        }
        
        reverse(ans.begin() , ans.end());

        return ans;
    }
};

```