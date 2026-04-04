---
{"dg-publish":true,"permalink":"/personal-vault/data-structures-and-algorithms/to-be-revisited/find-the-duplicate-number-287/"}
---




Given an array of integers `nums` containing `n + 1` integers where each integer is in the range `[1, n]` inclusive.

There is only **one repeated number** in `nums`, return _this repeated number_.

You must solve the problem **without** modifying the array `nums` and using only constant extra space.

**Example 1:**

**Input:** nums = [1,3,4,2,2]
**Output:** 2

**Example 2:**

**Input:** nums = [3,1,3,4,2]
**Output:** 3

**Example 3:**

**Input:** nums = [3,3,3,3,3]
**Output:** 3

**Constraints:**

- `1 <= n <= 105`
- `nums.length == n + 1`
- `1 <= nums[i] <= n`
- All the integers in `nums` appear only **once** except for **precisely one integer** which appears **two or more** times.

**Follow up:**

- How can we prove that at least one duplicate number must exist in `nums`?
- Can you solve the problem in linear runtime complexity?



Floyd cycle detection algorithm clever use , tortoise and hare 
```python

class Solution:
	def findDuplicate(self, nums: List[int]) -> int:
	
		fast = slow = nums[0]
		
		while True:
			slow = nums[slow]
			fast = nums[nums[fast]]
			
			if slow == fast:
				break
		
		slow = nums[0]
		
		while slow != fast:
			slow = nums[slow]
			fast = nums[fast]
		
		return slow

```



This is the mathematical core of Floyd's algorithm. After the first meeting point, if you reset one pointer to the start and advance **both at the same speed**, they meet exactly at the **cycle entry** — which is the duplicate number.

let F be distance from start to cycle entry
and C be cycle length

when they first meet, slow has travelled lets say F + a steps 
fast has travelled F + a + k*C steps 


$$  fast = 2 * slow $$

$$  F + a + k*C = 2 * (F + a )$$
$$F = k*C - a$$

so the distance from start to entry = distance from meeting point to entry , simplest case when k =1 , F =  C - a


```
[ start ] --F steps--> [ entry ] --a steps--> [ meeting point ]
                           ^                         |
                           |______ C - a steps _____|
```

start se F steps leke entry point pe aaogey , entry point se meeting point tak C - a steps hai and we have proved that F = C-a for  k= 1, easier to visualize, therefore ek ek step move karne se they will meet at entry point eventually. 


and we will return the entry point.