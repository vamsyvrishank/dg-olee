---
{"dg-publish":true,"permalink":"/personal-vault/dsa/to-be-revisited/rod-cutting/"}
---

#### Question:

## Problem statement 

Given a rod of length ‘N’ units. The rod can be cut into different sizes and each size has a cost associated with it. Determine the maximum cost obtained by cutting the rod and selling its pieces.

**Note:**
1. The sizes will range from 1 to ‘N’ and will be integers.
2. The sum of the pieces cut should be equal to ‘N’.
3. Consider 1-based indexing.


>**Sample Input 1:**
2
5
2 5 7 8 10
8
3 5 8 9 10 17 17 20


>**Sample Output 1:**
12
24

>**Explanation of sample input 1:**
Test case 1:
All possible partitions are:
 1,1,1,1,1           max_cost=(2+2+2+2+2)=10
 1,1,1,2             max_cost=(2+2+2+5)=11
 1,1,3               max_cost=(2+2+7)=11
 1,4                 max_cost=(2+8)=10
 5                   max_cost=(10)=10
 2,3                 max_cost=(5+7)=12
 1,2,2               max _cost=(1+5+5)=12    

Clearly, if we cut the rod into lengths 1,2,2, or 2,3, we get the maximum cost which is 12.


### Solution:

##### 1.Recursive and Memoization
```java
public class Solution {
    public static int cutRod(int[] price, int n) {
        int[] maxRevenue = new int[n + 1]; //maxRevenue stored for each rode length.
        Arrays.fill(maxRevenue, -1);
        return cutRodUtil(price, n, maxRevenue);
    }

    public static int cutRodUtil(int[] price, int remainingSize, int[] maxRevenue) {
        if (remainingSize == 0) {
            return 0;
        }

        if (maxRevenue[remainingSize] != -1) {
            return maxRevenue[remainingSize];
        }

        int max = Integer.MIN_VALUE;
        for (int cutLength = 1; cutLength <= remainingSize; cutLength++) {
            int revenueIfCut = cutRodUtil(price, remainingSize - cutLength, maxRevenue) + price[cutLength - 1];//index adjusted
            max = Math.max(max, revenueIfCut);
        }

        return maxRevenue[remainingSize] = max;
    }
}
```

##### 2. Tabulation

[Pepcoding Tabulation: ](https://youtu.be/eQuJb5gBkkc?t=310)
```java
public class Solution {
    public static int cutRod(int[] price, int rodeLength) {
        int[] maxRevenue = new int[rodeLength + 1]; //maxRevenue stored for each rode length.

		for(int currentLength = 1; currentLength <= rodLength; currentLength++){
			
			int currentMaxRevenue = Integer.MIN_VALUE;
			
			for (int cutLength = 1; cutLength <= currentLength; cutLength++) {
				int revenueIfCut = price[cutLength - 1] + maxRevenue[currentLength - cutLength];
				currentMaxRevenue = Math.max(currentMaxRevenue, revenueIfCut);
			}
			maxRevenue[currentLength] = currentMaxRevenue;
		}
        return maxRevenue[rodeLength];
    }
}
```