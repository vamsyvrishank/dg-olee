---
{"dg-publish":true,"permalink":"/personal-vault/system-design/high-level-design/sort-colors/"}
---



Given an array ****A[]**** consisting of only ****0s****, ****1s,**** and ****2s****. The task is to sort the array, i.e., put all 0s first, then all 1s and all 2s in last.

Given an array `nums` with `n` objects colored red, white, or blue, sort them **[in-place](https://en.wikipedia.org/wiki/In-place_algorithm)** so that objects of the same color are adjacent, with the colors in the order red, white, and blue.
We will use the integers `0`, `1`, and `2` to represent the color red, white, and blue, respectively.
You must solve this problem without using the library's sort function.


- (Naive) Sorting – O(N * log(N)) Time and O(1) Space -> <mark class="hltr-green">Just sort</mark>
- Better Approach] Counting 0s, 1s and 2s – Two Pass – O(N) Time and O(1) Space
- 
```c++
class Solution {
public:
    void sortColors(vector<int>& nums) {
        int one=0 , two=0 , three=0;
        for(int i =0; i< nums.size();i++){
            if(nums[i]==0) one++;
            else if(nums[i]==1) two++;
            else if(nums[i]==2) three++;
        }
        for(int i =0; i< nums.size();i++){
            if(one>0){
                nums[i] = 0;
                one--;
            }
            else if(two>0){
                nums[i] = 1;
                two--;
            }
            else if(three>0){
                nums[i] = 2;
                three--;
            } 
        }
    }
};
```



```cpp

int c0=0,c1=0,c2=0;
for(int i =0 ; i < arr.size();i++){
	if(arr[i]==0)c0++;
	else if(arr[i]==1) c1++;
	else c2++;
}
int index = 0;
while(c0--) {
	arr[index] =0;
	index++;
}
while(c1--){
	arr[index] = 1;
	index++;
}
while(c2--){
	arr[index] = 2;
	index++;
}
```

- Expected Approach] Dutch National Flag Algorithm – One Pass – O(N) Time and O(1) Space

```cpp

class Solution {
public:
    void sortColors(vector<int>& nums) {

        //dutch partitioning algorithm.
        // 0 -> 0 to low-1 , 1 -> low to mid-1 , mid to high ( unsorted )  , 
        // 2 -> high + 1 to n-1;
        
        int low = 0 , mid = 0 , high = nums.size()-1;

        while(mid <= high){
            if(nums[mid]==0){
                swap(nums[low] , nums[mid]);
                low++;
                mid++;
            }
            else if( nums[mid]==1){
                mid++;
            }
            else{
                swap(nums[mid] , nums[high]);
                high--;
            }
        }
    }
};

```
