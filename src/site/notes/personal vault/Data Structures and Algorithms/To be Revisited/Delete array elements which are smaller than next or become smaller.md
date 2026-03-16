---
{"dg-publish":true,"permalink":"/personal-vault/data-structures-and-algorithms/to-be-revisited/delete-array-elements-which-are-smaller-than-next-or-become-smaller/"}
---




Asked in Zeta Interview. gave O(n^2) wanted O(n) - could have done it
```cpp
vector<int> deleteElement(int arr[],int n,int k)
{
    // complete the function
    
    stack<int> st;
    st.push(arr[0]);
    int count = 0;
    for(int i=1;i<n;i++){
        while(!st.empty() and st.top() < arr[i] and count < k){
            st.pop();
            count++;
        }
        st.push(arr[i]);
    }
    
    int m = st.size();
    vector<int> ans;
    while(!st.empty()){
        ans.push_back(st.top());
        st.pop();
    }
    
    reverse(ans.begin() , ans.end());
    
    return ans;
    
    
}
```