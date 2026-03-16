---
{"dg-publish":true,"permalink":"/personal-vault/data-structures-and-algorithms/to-be-revisited/sum-root-to-leaf-numbers/"}
---




You are given the `root` of a binary tree containing digits from `0` to `9` only.

Each root-to-leaf path in the tree represents a number.

- For example, the root-to-leaf path `1 -> 2 -> 3` represents the number `123`.

Return _the total sum of all root-to-leaf numbers_. Test cases are generated so that the answer will fit in a **32-bit** integer.

A **leaf** node is a node with no children.

>**Example 1:**
![[Pasted image 20240608140250.png  \| 300]]
**Input:** root = [1,2,3]
**Output:** 25
**Explanation:**
The root-to-leaf path `1->2` represents the number `12`.
The root-to-leaf path `1->3` represents the number `13`.
Therefore, sum = 12 + 13 = `25`.

>**Example 2:**
![[Pasted image 20240608140336.png \| 300]]
**Input:** root = [4,9,0,5,1]
**Output:** 1026
**Explanation:**
The root-to-leaf path `4->9->5` represents the number 495.
The root-to-leaf path `4->9->1` represents the number 491.
The root-to-leaf path `4->0` represents the number 40.
Therefore, sum = 495 + 491 + 40 = `1026`.

**Constraints:**

- The number of nodes in the tree is in the range `[1, 1000]`.
- `0 <= Node.val <= 9`
- The depth of the tree will not exceed `10`.
One approach
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
    void traversal(TreeNode* root , int &ans , string temp){
        if(root==nullptr) return;

        temp+= to_string(root->val);

        if(root->left == nullptr and root->right ==nullptr){
            int temp2 = stoi(temp);
            ans+=temp2;
        }

        traversal(root->left , ans ,temp);
        traversal(root->right , ans ,temp);
    }
public:
    int sumNumbers(TreeNode* root) {
        int ans =0 ;
        string temp;
        traversal(root , ans, temp);

        return ans;
    }
};
```


Another Approach 
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

    int dfs(TreeNode* root , int currsum)
    {
        if(root==nullptr) return 0;
        currsum = currsum*10 + root->val;

        if(root->left ==nullptr and root->right==nullptr) return currsum;

        int leftsum = dfs(root->left , currsum);
        int rightsum = dfs(root->right , currsum);

        return leftsum + rightsum;

    }
    int sumNumbers(TreeNode* root) {

        if(root==nullptr){
            return 0;
        }

        int sum = 0;
        return dfs(root , sum);


    }
};
```