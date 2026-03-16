---
{"dg-publish":true,"permalink":"/personal-vault/data-structures-and-algorithms/to-be-revisited/binary-tree-bottom-view-and-top-view/"}
---









Top View ( we check if the map contains the key or not and then update only once)
```cpp
class Solution
{
public:

	/*
		A binary tree node is defined as:

		class Node
		{
		public:
			int data;									// data field
			Node* left = nullptr, *right = nullptr;		// pointer to the left and right child

			Node() {}
			Node(int data): data(data) {}
			Node(int data, Node *left, Node *right): data(data), left(left), right(right) {}
		};
	*/

	vector<int> findTopView(Node* root)
	{
		// Write your code here...
		vector<int>ans;
		if(root == NULL) return ans;
		map<int, int>mp; // level position, node val
		
		queue<pair<Node*, int>>Q; // Node , Level
		Q.push({root, 0});
		
		while(!Q.empty()){
			auto it = Q.front();
			Q.pop();
			Node* node = it.first;
			int level = it.second;
			
			if(mp.find(level)== mp.end()) mp[level] = node->data;
			if(node->left != NULL) Q.push({node->left, level-1});
			if(node->right != NULL) Q.push({node->right, level+1});
		}
		
		for(auto x:mp){
			ans.push_back(x.second);
		}
		return ans;
	}
};
```



Bottom View ( we don't need to check for existence as we need to keep updating map(col) till the value exists )
```cpp

class Solution
{
public:

	/*
		A binary tree node is defined as:

		class Node
		{
		public:
			int data;									// data field
			Node* left = nullptr, *right = nullptr;		// pointer to the left and right child

			Node() {}
			Node(int data): data(data) {}
			Node(int data, Node *left, Node *right): data(data), left(left), right(right) {}
		};
	*/

	vector<int> findBottomView(Node* root)
	{
		// Write your code here...
		vector<int> ans;
		if(root == NULL) return ans;
		
		map<int,int>mp; // col, val;
		queue<pair<Node*, int>>Q; // node, column values
		Q.push({root, 0});
		while(!Q.empty()){
			auto temp = Q.front();
			Q.pop();
			Node* node = temp.first;
			int col = temp.second;
			mp[col] = node->data;
			if(node->left != NULL) Q.push({node->left, col-1});
			if(node->right != NULL) Q.push({node->right, col+1});
		}
		for(auto x:mp){
			ans.push_back(x.second);
		}
		return ans;
	}
};
```