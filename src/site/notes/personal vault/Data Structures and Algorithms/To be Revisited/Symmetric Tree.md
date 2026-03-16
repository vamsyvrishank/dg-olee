---
{"dg-publish":true,"permalink":"/personal-vault/data-structures-and-algorithms/to-be-revisited/symmetric-tree/"}
---


Given the `root` of a binary tree, _check whether it is a mirror of itself_ (i.e., symmetric around its center).

**Example 1:**
![[Pasted image 20240607174659.png \| 600]]

**Input:** root = [1,2,2,3,4,4,3]
**Output:** true

**Example 2:**
**Input:** root = [1,2,2,null,3,null,3]
**Output:** false

**Constraints:**

- The number of nodes in the tree is in the range `[1, 1000]`.
- `-100 <= Node.val <= 100`


Recursive
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
    bool check(TreeNode* left , TreeNode* right){
        if(left==nullptr and right==nullptr) return true;
        if(left==nullptr || right==nullptr) return false;

        if(left->val==right->val){
            return (check(left->right , right->left) and check(left->left, right->right));
        }
	
        return false;
    }
public:
    bool isSymmetric(TreeNode* root) {
        if(root==nullptr) return true;

        return check(root->left , root->right);
    }
};


```


Iterative
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
    bool isSymmetric(TreeNode* root) {

        queue<TreeNode*> q;
        q.push(root->left);
        q.push(root->right);

        while(!q.empty()){
            TreeNode* leftNode = q.front();
            q.pop();
            TreeNode* rightNode = q.front();
            q.pop();

            if(leftNode==NULL and rightNode==NULL) continue;
            if (leftNode==NULL || rightNode ==NULL || leftNode->val != rightNode->val) return false;

            q.push(leftNode->left);
            q.push(rightNode->right);
            q.push(leftNode->right);
            q.push(rightNode->left);

        }

        return true;
        
    }
};

```