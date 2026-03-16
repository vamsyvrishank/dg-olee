---
{"dg-publish":true,"permalink":"/personal-vault/data-structures-and-algorithms/to-be-revisited/pattern-template/"}
---

1. Constant window size (not asked many times)
	- arr = [-1, 3, 4, 5, -1] & k = 4
	- Max sum that you can get by picking up k=4 *consecutive* elements.
	- Here we'll keep the window size constant and move the window till `right < arr.length`.

```java
sum = 7, l = 0, r = k - 1

while(r < arr.length - 1){
	sum = sum - arr[l];
	l++;
	r++;
	sum += arr[r];
	maxSum = max(maxSum, sum);
}
```

2.  Longest subarray/substring where \<condition\>
	1. This is basically acquire and release strategy [[personal vault/Data Structures and Algorithms/To be Revisited/Acquire and Release Strategy\|Acquire and Release Strategy]] or Expand and Shrink strategy.
		1. [[personal vault/Data Structures and Algorithms/To be Revisited/1004. Max Consecutive Ones III\|1004. Max Consecutive Ones III]]
	2. Let's consider this question and see the different strategies to solve it.

![[AcquireNrelease_1.jpg \| 300]] ![[AcquireNrelease_2.jpg \| 300]] ![[AcquireNrelease_3.jpg \| 300]]



