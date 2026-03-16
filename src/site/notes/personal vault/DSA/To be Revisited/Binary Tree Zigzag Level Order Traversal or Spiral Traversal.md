---
{"dg-publish":true,"permalink":"/personal-vault/dsa/to-be-revisited/binary-tree-zigzag-level-order-traversal-or-spiral-traversal/"}
---


### Problem Statement:

Given the `root` of a binary tree, return _the zigzag level order traversal of its nodes' values_. (i.e., from left to right, then right to left for the next level and alternate between).

>**Example 1:**
![[Screenshot 2024-06-06 at 10.33.25 PM.png \| 200]]
**Input:** root = [3,9,20,null,null,15,7]
**Output:** \[[3],[20,9],[15,7\|3],[20,9],[15,7]]

>**Example 2:**
**Input:** root = [1]
**Output:** \[[1\|1]]

>**Example 3:**
**Input:** root = []
**Output:** []

**Constraints:**

- The number of nodes in the tree is in the range `[0, 2000]`.
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
public:
    vector<vector<int>> zigzagLevelOrder(TreeNode* root) {
        if(root==NULL) return vector<vector<int>> ();
        queue<TreeNode*> q;
        q.push(root);
        vector<vector<int>> ans;

        int level = 1;
        while(!q.empty())
        {
            int size = q.size();
            vector<int> v;
            for(int i =0 ; i < size;i++){
                TreeNode* temp = q.front();
                v.push_back(temp->val);
                q.pop();
                
                if(temp->left!=NULL) q.push(temp->left);
                if(temp->right!=NULL) q.push(temp->right);
            }

            if(level%2==0){
                reverse(v.begin(), v.end());
            }
            ans.push_back(v);
            level++;

        }
        return ans;
    }
};
```