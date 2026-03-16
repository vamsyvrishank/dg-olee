---
{"dg-publish":true,"permalink":"/personal-vault/data-structures-and-algorithms/dsa-topics/binary-search/"}
---


https://serious-snowshoe-4f1.notion.site/Binary-Search-0db32448e980452cabaddfff755ef111



After finishing notion standard problems try different flavors of problems.

Types of Problems : 

1. Classic Questions - lower_bound , upper_bound
2. Binary search on answer
3. greedy + check function 
4. binary search with prefix / suffix 
5. binary search on continuous domain
6. ternary search ( unimodal functions)
7. advanced / embedded binary search ( BS + DP or BS + combinatorics)




Template approach - check(mid) - for binary search on answer

basically have a mid , check(mid) - whatever the implementation or logic inside check(mid) function


```cpp

// Suppose we want the minimum x that satisfies check(x)

auto check = [&](long long mid) -> bool {
    // implement feasibility test
    // return true if mid works, false otherwise
};

long long low = L, high = R;  // search space
long long ans = R;            // depends on whether we want min or max

while (low <= high) {
    long long mid = low + (high - low) / 2;
    if (check(mid)) {
        ans = mid;          // mid works, update answer
        high = mid - 1;     // look for smaller
    } else {
        low = mid + 1;      // mid doesn't work, need bigger
    }
}

// 'ans' is the final result

```

#### Classic Sorted Array Search 

1. CF706B - https://arc.net/l/quote/vekkkaxd

```cpp

// Author: Jarvis
#include <stdio.h>


// bits/stdc++.h custom replacement
#ifndef _GLIBCXX_BITS_STDCPP_H
#define _GLIBCXX_BITS_STDCPP_H

// All standard headers
#include <algorithm>
#include <array>
#include <bitset>
#include <cassert>
#include <cctype>
#include <cerrno>
#include <cfenv>
#include <cfloat>
#include <chrono>
#include <cinttypes>
#include <climits>
#include <cmath>
#include <complex>
#include <condition_variable>
#include <csetjmp>
#include <csignal>
#include <clocale>
#include <cstdarg>
#include <cstddef>
#include <cstdint>
#include <cstdio>
#include <cstdlib>
#include <cstring>
#include <ctime>
#include <deque>
#include <exception>
#include <fstream>
#include <functional>
#include <future>
#include <initializer_list>
#include <iomanip>
#include <ios>
#include <iosfwd>
#include <iostream>
#include <istream>
#include <iterator>
#include <limits>
#include <list>
#include <locale>
#include <map>
#include <memory>
#include <mutex>
#include <new>
#include <numeric>
#include <ostream>
#include <queue>
#include <random>
#include <ratio>
#include <regex>
#include <scoped_allocator>
#include <set>
#include <shared_mutex>
#include <sstream>
#include <stack>
#include <stdexcept>
#include <streambuf>
#include <string>
#include <strstream>
#include <system_error>
#include <thread>
#include <tuple>
#include <type_traits>
#include <typeindex>
#include <typeinfo>
#include <unordered_map>
#include <unordered_set>
#include <utility>
#include <valarray>
#include <vector>

#endif // _GLIBCXX_BITS_STDCPP_H

using namespace std;

// Fast IO
#define FAST_IO ios::sync_with_stdio(false); cin.tie(0); cout.tie(0)

// Aliases
using ll = long long;
using pii = pair<int, int>;
using vi = vector<int>;
using vll = vector<ll>;
using vpii = vector<pii>;

// Macros
#define all(x) (x).begin(), (x).end()
#define sz(x) (int)(x).size()
#define pb push_back
#define ff first
#define ss second
#define endl '\n'

// Debugging (disable for Codeforces)
#ifdef LOCAL
#define debug(x) cerr << #x << " = "; _print(x); cerr << endl;
#else
#define debug(x)
#endif

void _print(int x) {cerr << x;}
void _print(long x) {cerr << x;}
void _print(long long x) {cerr << x;}
void _print(unsigned x) {cerr << x;}
void _print(unsigned long x) {cerr << x;}
void _print(unsigned long long x) {cerr << x;}
void _print(float x) {cerr << x;}
void _print(double x) {cerr << x;}
void _print(long double x) {cerr << x;}
void _print(char x) {cerr << '\'' << x << '\'';}
void _print(const string &x) {cerr << '\"' << x << '\"';}
void _print(bool x) {cerr << (x ? "true" : "false");}

template<typename T, typename V>
void _print(const pair<T, V> &x) {cerr << '{'; _print(x.first); cerr << ','; _print(x.second); cerr << '}';}
template<typename T>
void _print(const vector<T> &x) {cerr << '['; for (auto &i : x) {_print(i); cerr << ' ';} cerr << ']';}
template<typename T>
void _print(const set<T> &x) {cerr << '{'; for (auto &i : x) {_print(i); cerr << ' ';} cerr << '}';}
template<typename T, typename V>
void _print(const map<T, V> &x) {cerr << '{'; for (auto &i : x) {_print(i); cerr << ' ';} cerr << '}';}



ll floor_bs( vector<ll> &shops , ll day){
    ll start = 0; int end = shops.size()-1;
    ll res = 0;

    while(start <= end){
        int mid = start + int((end-start)/2);
        if(shops[mid] <= day){
            res = mid+1;
            start = mid +1; 
        }
        else {
            end = mid-1;
        }
    }

    return res;
}

// 3 6 8 10 11

// Main solve function
void solve() {

    // each day need to find size of array where arr[i] <= mi ( money beecola can spend )

    ll n ; 
    cin>>n;
    vector<ll> shops(n);
    for(int i = 0 ; i < n ; i++){
        cin>>shops[i];
    }
    ll q ; cin>>q;
    vector<ll> days(q);
    for(int i = 0 ; i < q; i++) {cin>>days[i];}

    // params -> n size = 10^5 , x_i size = 10^5 , q size = 10^5 , m size = 10^9
    // sorting takes nlogn , 10^5 x 5 log(10) , searching then takes log(q) time , total time  = 10^5 log(10) 5 log(10), under 10^9

    sort(shops.begin() , shops.end());

    for(int i = 0 ; i< q ; i++) { 
        ll temp;
        temp  = floor_bs(shops, days[i]);
        cout<<temp<<endl;
    }

    
}

// Main entry
int main() {
    FAST_IO;
    solve();
    return 0;
}

```

Python3 solution 

```python

import sys
from bisect import bisect_right


def main():
    input = sys.stdin.readline

    n = int(input())
    shops = list( map( int , input().split()))
    q = int(input())
    days = [ int(input()) for i in range(q)]

    
    shops.sort()
    results = []

    for money in days:
        count = bisect_right(shops , money)
        results.append(str(count))


    sys.stdout.write("\n".join(results) + "\n")


if __name__ == "__main__":
    main()
```



 2. 600B - https://codeforces.com/problemset/problem/600/B

```c++

// Author: Jarvis
#include <stdio.h>


// bits/stdc++.h custom replacement
#ifndef _GLIBCXX_BITS_STDCPP_H
#define _GLIBCXX_BITS_STDCPP_H

// All standard headers
#include <algorithm>
#include <array>
#include <bitset>
#include <cassert>
#include <cctype>
#include <cerrno>
#include <cfenv>
#include <cfloat>
#include <chrono>
#include <cinttypes>
#include <climits>
#include <cmath>
#include <complex>
#include <condition_variable>
#include <csetjmp>
#include <csignal>
#include <clocale>
#include <cstdarg>
#include <cstddef>
#include <cstdint>
#include <cstdio>
#include <cstdlib>
#include <cstring>
#include <ctime>
#include <deque>
#include <exception>
#include <fstream>
#include <functional>
#include <future>
#include <initializer_list>
#include <iomanip>
#include <ios>
#include <iosfwd>
#include <iostream>
#include <istream>
#include <iterator>
#include <limits>
#include <list>
#include <locale>
#include <map>
#include <memory>
#include <mutex>
#include <new>
#include <numeric>
#include <ostream>
#include <queue>
#include <random>
#include <ratio>
#include <regex>
#include <scoped_allocator>
#include <set>
#include <shared_mutex>
#include <sstream>
#include <stack>
#include <stdexcept>
#include <streambuf>
#include <string>
#include <strstream>
#include <system_error>
#include <thread>
#include <tuple>
#include <type_traits>
#include <typeindex>
#include <typeinfo>
#include <unordered_map>
#include <unordered_set>
#include <utility>
#include <valarray>
#include <vector>

#endif // _GLIBCXX_BITS_STDCPP_H

using namespace std;

// Fast IO
#define FAST_IO ios::sync_with_stdio(false); cin.tie(0); cout.tie(0)

// Aliases
using ll = long long;
using pii = pair<int, int>;
using vi = vector<int>;
using vll = vector<ll>;
using vpii = vector<pii>;

// Macros
#define all(x) (x).begin(), (x).end()
#define sz(x) (int)(x).size()
#define pb push_back
#define ff first
#define ss second
#define endl '\n'

// Debugging (disable for Codeforces)
#ifdef LOCAL
#define debug(x) cerr << #x << " = "; _print(x); cerr << endl;
#else
#define debug(x)
#endif

void _print(int x) {cerr << x;}
void _print(long x) {cerr << x;}
void _print(long long x) {cerr << x;}
void _print(unsigned x) {cerr << x;}
void _print(unsigned long x) {cerr << x;}
void _print(unsigned long long x) {cerr << x;}
void _print(float x) {cerr << x;}
void _print(double x) {cerr << x;}
void _print(long double x) {cerr << x;}
void _print(char x) {cerr << '\'' << x << '\'';}
void _print(const string &x) {cerr << '\"' << x << '\"';}
void _print(bool x) {cerr << (x ? "true" : "false");}

template<typename T, typename V>
void _print(const pair<T, V> &x) {cerr << '{'; _print(x.first); cerr << ','; _print(x.second); cerr << '}';}
template<typename T>
void _print(const vector<T> &x) {cerr << '['; for (auto &i : x) {_print(i); cerr << ' ';} cerr << ']';}
template<typename T>
void _print(const set<T> &x) {cerr << '{'; for (auto &i : x) {_print(i); cerr << ' ';} cerr << '}';}
template<typename T, typename V>
void _print(const map<T, V> &x) {cerr << '{'; for (auto &i : x) {_print(i); cerr << ' ';} cerr << '}';}

// Main solve function

int upper_bound_bs(vector<int> &arr , int &target){
    int start = 0 , end = arr.size()-1;
    int res = 0;
    while(start <= end ){
        int mid = start + (end - start)/2;
        if(arr[mid] <= target){
            res = mid+1;
            start = mid+1;
        }
        else{
            end = mid-1;
        }   
    }
    return res;
}

void solve() {
    
    int n , m ; 
    cin>>n>>m;
    
    vector<int> a1(n) , b1(m);
    for(int i =0 ;i<n ; i++){
        cin>>a1[i];
    }
    sort(a1.begin() , a1.end());

    for(int i = 0; i < m; i++){
        cin>>b1[i];
        // auto it = upper_bound(a1.begin(), a1.end() , b1[i]);
        // int res = (it - a1.begin());
        int res = upper_bound_bs(a1 , b1[i]);
        cout<<res<<" ";
        
    }
}

// Main entry
int main() {
    FAST_IO;
    solve();
    return 0;
}

```

```python

```