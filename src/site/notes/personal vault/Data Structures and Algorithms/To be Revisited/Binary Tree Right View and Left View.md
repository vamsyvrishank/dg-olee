---
{"dg-publish":true,"permalink":"/personal-vault/data-structures-and-algorithms/to-be-revisited/binary-tree-right-view-and-left-view/"}
---


Given the `root` of a binary tree, imagine yourself standing on the **right side** of it, return _the values of the nodes you can see ordered from top to bottom_.

**Example 1:**

![[Pasted image 20240608123341.png \| 500]]

**Input:** root = [1,2,3,null,5,null,4]
**Output:** [1,3,4]

**Example 2:**

**Input:** root = [1,null,3]
**Output:** [1,3]

**Example 3:**

**Input:** root = []
**Output:** []

**Constraints:**

- The number of nodes in the tree is in the range `[0, 100]`.
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
public:
    vector<int> rightSideView(TreeNode* root) {

        if(root==nullptr) return {};

        vector<int> ans;
        
        queue<TreeNode*> q;
        q.push(root);

        while(!q.empty()){
            int size = q.size();

            for(int i =0 ; i < size ; i++){
                TreeNode* temp = q.front();
                q.pop();

                if(i==0) ans.push_back(temp->val);
                
                if(temp->right != nullptr) q.push(temp->right);
                if(temp->left != nullptr) q.push(temp->left);
            }
        }

        return ans;
    }
};

```


### Left View of a Binary Tree

Left view me either you reverse the way you push the elements  in the queue or do  i== n-1 for left view if you are putting right element first.

```cpp
//{ Driver Code Starts

//Function to return a list containing elements of left view of the binary tree.
vector<int> leftView(Node *root)
{
   // Your code here
   if(root==nullptr) return {};
   vector<int> ans;
   queue<Node*> q;
   q.push(root);
   
   while(!q.empty()){
       
       int size = q.size();
       
       for(int i =0; i< size;i++){
           Node* temp = q.front();
           q.pop();
           
           if(i==0) ans.push_back(temp->data);
           
           if(temp->left !=nullptr) q.push(temp->left);
           if(temp->right !=nullptr) q.push(temp->right);
       }
   }
   
   return ans;
}


```