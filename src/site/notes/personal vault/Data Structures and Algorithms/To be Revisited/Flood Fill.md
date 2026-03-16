---
{"dg-publish":true,"permalink":"/personal-vault/data-structures-and-algorithms/to-be-revisited/flood-fill/"}
---



We have used DFS for flood fill , if you wanna check BFS approach we have done it in the previous questions , mark here that we are not using visited array , because we are checking all the conditions before going to dfs , so dfs call will not happen indefinitely. 

```cpp
class Solution {
public:
    void dfs(int sr , int sc , vector<vector<int>> & image , int &color ,int &inicolor, vector<pair<int,int>> &directions){

        image[sr][sc] = color;
        int n = image.size();
        int m = image[0].size();
        for(auto d : directions){
            int nrow = sr + d.first;
            int ncol = sc + d.second;

            if(nrow >=0 and nrow < n && ncol >=0 and ncol <m && 
            image[nrow][ncol]== inicolor and image[nrow][ncol] != color){
                dfs(nrow,ncol,image,color,inicolor,directions);
            }
        }


    }
    vector<vector<int>> floodFill(vector<vector<int>>& image, int sr, int sc, int color) {

        int inicolor = image[sr][sc];
        vector<pair<int,int>> directions = {{1,0},{-1,0},{0,1},{0,-1}};
        dfs(sr,sc,image,color ,inicolor,directions);
        return image;
        
    }
};
```