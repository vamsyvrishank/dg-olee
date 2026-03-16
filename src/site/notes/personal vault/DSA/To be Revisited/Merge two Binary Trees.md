---
{"dg-publish":true,"permalink":"/personal-vault/dsa/to-be-revisited/merge-two-binary-trees/"}
---






This is when we are updating the root1 node itself , what if interviewer asks to make a whole new tree ? 
```cpp
class Solution {
private:
    TreeNode* merge(TreeNode* root1 , TreeNode* root2){

        if(root1==nullptr and root2==nullptr) return nullptr;
        if(root1==nullptr) return root2;
        if(root2 == nullptr) return root1;

        root1->val = root1->val + root2->val;

        root1->left = merge(root1->left , root2->left);
        root1->right = merge(root1->right , root2->right);

        return root1;
    }
public:
    TreeNode* mergeTrees(TreeNode* root1, TreeNode* root2) {
        root1 = merge(root1 , root2 );

        return root1;
    }
};
```


then we tell him this approach -> we are creating a root3 ( new node) and assigning it the values.
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
    TreeNode* merge(TreeNode* &root1 , TreeNode* &root2 , TreeNode* &root3){

        if(root1==nullptr and root2==nullptr) return nullptr;
        if(root1==nullptr) return root2;
        if(root2 == nullptr) return root1;

        int value = root1->val + root2->val;

        root3 = new TreeNode(value);
        
        root3->left = merge(root1->left , root2->left ,root3->left);
        root3->right = merge(root1->right , root2->right , root3->right);

        return root3;

    }
public:
    TreeNode* mergeTrees(TreeNode* root1, TreeNode* root2) {
        TreeNode* root3;

        root3 = merge(root1 , root2 , root3);

        return root3;
        
    }
};
```