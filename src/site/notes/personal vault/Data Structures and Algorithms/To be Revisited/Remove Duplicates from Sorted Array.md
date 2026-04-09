---
{"dg-publish":true,"permalink":"/personal-vault/data-structures-and-algorithms/to-be-revisited/remove-duplicates-from-sorted-array/"}
---



Given an integer array `nums` sorted in **non-decreasing order**, remove the duplicates [**in-place**](https://en.wikipedia.org/wiki/In-place_algorithm) such that each unique element appears only **once**. The **relative order** of the elements should be kept the **same**.

Consider the number of _unique elements_ in `nums` to be `k**​​​​​​​**`​​​​​​​. After removing duplicates, return the number of unique elements `k`.

The first `k` elements of `nums` should contain the unique numbers in **sorted order**. The remaining elements beyond index `k - 1` can be ignored.

**Custom Judge:**

The judge will test your solution with the following code:

int[] nums = [...]; // Input array
int[] expectedNums = [...]; // The expected answer with correct length

int k = removeDuplicates(nums); // Calls your implementation

assert k == expectedNums.length;
for (int i = 0; i < k; i++) {
    assert nums[i] == expectedNums[i];
}

If all assertions pass, then your solution will be **accepted**.

**Example 1:**

**Input:** nums = [1,1,2]
**Output:** 2, nums = [1,2,_]
**Explanation:** Your function should return k = 2, with the first two elements of nums being 1 and 2 respectively.
It does not matter what you leave beyond the returned k (hence they are underscores).

**Example 2:**

**Input:** nums = [0,0,1,1,1,2,2,3,3,4]
**Output:** 5, nums = [0,1,2,3,4,_,_,_,_,_]
**Explanation:** Your function should return k = 5, with the first five elements of nums being 0, 1, 2, 3, and 4 respectively.
It does not matter what you leave beyond the returned k (hence they are underscores).

**Constraints:**

- `1 <= nums.length <= 3 * 104`
- `-100 <= nums[i] <= 100`
- `nums` is sorted in **non-decreasing** order.


count need to be added +1 because we are counting swaps but elements would be +1 , like two swaps means 3 elements are there in the list
```python


class Solution:
    def removeDuplicates(self, nums: List[int]) -> int:

        #efficient solutions using swaps 
        i = 0
        j = 0
        n = len(nums)
        count = 0
        while j < n:
            if nums[i]==nums[j]:
                j+=1
            else:
                # i+1 index need to be swapped with jth index element ( draw it and see from example)
                nums[i+1] , nums[j] = nums[j] , nums[i+1]
                i+=1
                j+=1
                count+=1
                
        count+=1
        return count
        
```

we are obviously doing swaps k-1 swaps, however we can do better just by keeping a track of the count and updating the values by overwriting it. 

```python
class Solution:
    def removeDuplicates(self, nums: List[int]) -> int:

        count = 0
        n = len(nums)
        for i in range(1,n):
            if nums[i]==nums[i-1]:
                continue
            count+=1
            nums[count] = nums[i]
        
    
        count+=1
        return count
        
```

t
his way also works 

```python
class Solution:
    def removeDuplicates(self, nums: List[int]) -> int:

        count = 0
        n = len(nums)
        for i in range(1, n):
            if nums[count] != nums[i]:
                count+=1
                nums[count] = nums[i]
        return count+1
        
```

Always take care of edge cases 

```python
class Solution:
    def removeDuplicates(self, nums: List[int]) -> int:

        count = 0
        n = len(nums)
        if n == 0:
            return count
        for i in range(1, n):
            if nums[count] != nums[i]:
                count+=1
                nums[count] = nums[i]
        return count+1
        
        
```