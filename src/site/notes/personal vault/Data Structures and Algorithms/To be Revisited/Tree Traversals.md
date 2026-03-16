---
{"dg-publish":true,"permalink":"/personal-vault/data-structures-and-algorithms/to-be-revisited/tree-traversals/"}
---


Inorder Recursive

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
    vector<int> v; 
    void fun(TreeNode* root){
        if(root== nullptr){
            return;
        }
        
        fun(root->left);
        v.push_back(root->val);
        fun(root->right);
    }
    vector<int> inorderTraversal(TreeNode* root) {
        fun(root);
        return v;
    }
};
```

Preorder Recursive
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
    vector<int>v;
    void preorder(TreeNode* root){
        if(root == nullptr ) return ;
        v.push_back(root->val);
        preorder(root->left);
        preorder(root->right);
    }
    vector<int> preorderTraversal(TreeNode* root) {
        preorder(root);
        return v;
    
    }
};
```

PostOrder Recursive
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
    vector<int> v;
    void postorder(TreeNode* root){
        if(root==nullptr) return;
        postorder(root->left);
        postorder(root->right);
        v.push_back(root->val);

    }
    vector<int> postorderTraversal(TreeNode* root) {
        postorder(root);
        
        return  v;
    }
};
```

Level Order Traversal
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
    vector<vector<int>> levelOrder(TreeNode *root) {
        if (root == nullptr) return {};

        queue<TreeNode *> q;
        q.push(root);
        vector<vector<int>> ans;
        while (q.size()) {
            int size = q.size();
            vector<int> temp;
            for (int i = 0; i < size; i++) {
                TreeNode *a = q.front();
                temp.push_back(a->val);
                if (a->left) {
                    q.push(a->left);
                }
                if (a->right) {
                    q.push(a->right);
                }
                q.pop();
            }
            ans.push_back(temp);
        }
        return ans;
    }
};

```