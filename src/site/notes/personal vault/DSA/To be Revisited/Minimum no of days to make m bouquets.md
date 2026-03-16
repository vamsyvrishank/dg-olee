---
{"dg-publish":true,"permalink":"/personal-vault/dsa/to-be-revisited/minimum-no-of-days-to-make-m-bouquets/"}
---




```cpp
class Solution {
public:
    bool canbloom(vector<int> &bloomDay , int m , int k, int day){
        int numofBk = 0;
        int count = 0;

        for(int i =0; i< bloomDay.size();i++){
            if(bloomDay[i] <=day) count++;
            else count = 0;
            if(count==k){
                numofBk++;
                count =0;
            }
        }

        if(numofBk >= m) return true;
        return false;
    }
    int minDays(vector<int>& bloomDay, int m, int k) {

        //binary search on answer.
        int start = *min_element(bloomDay.begin(),bloomDay.end());
        int end = *max_element(bloomDay.begin(),bloomDay.end());
        int ans = -1;
        while(start<=end){
            int mid = start + (end-start)/2;
            if(canbloom(bloomDay,m,k,mid)){
                ans = mid;
                end = mid-1;
            }
            else{
                start = mid+1;
            }
        }
        return ans;
    }
};
```