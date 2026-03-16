---
{"dg-publish":true,"permalink":"/personal-vault/dsa/sorting-algorithms/quick-sort/"}
---



**- Divide and Conquer Algorithm:** - Breaks down a large array into smaller sub-arrays. - Sorts the sub-arrays recursively. - Combines the sorted sub-arrays to achieve the final sorted array.

<mark style="background: #BBFABBA6;">***Key Steps*:**</mark>

1. **Choose a Pivot:**

	- Select an element from the array to serve as a reference point (pivot).

2. **Partition:**

	- Rearrange elements:
		
		- Elements less than the pivot move to its left.
		- Elements greater than the pivot move to its right.
		
	- Pivot ends up in its correct sorted position.

3. **Recursively Sort Sub-Arrays:**
	- Recursively apply Quicksort to the left and right sub-arrays (excluding the pivot).

4. **Base Case:**
	- If a sub-array has one or no elements, it's already sorted (recursion stops).

---

**- Efficiency:**

```
- Average-Case Time Complexity: O(n log n)

- Worst-Case Time Complexity: O(n^2) (rare, often mitigated with pivot selection strategies)

- Space Complexity: O(log n) due to recursion
```

**- Advantages:**

```
- Fast in practice for most real-world data sets.
- In-place sorting, requiring little extra memory.
```

**- Considerations:**

```
- Pivot selection can significantly impact performance.
- Can be less efficient for small arrays or already sorted data.
```

**Additional Notes:**

```
- Often used in standard sorting libraries due to its overall efficiency.
- Can be modified to handle various data types and sorting needs.
```
   
   ```java
   class Solution {
    public int[] sortArray(int[] nums) {
        quickSort(nums, 0, nums.length-1);
        return nums;
    }

    public void quickSort(int[] nums, int lo, int hi){
        if(lo >= hi){
            return;
        }
        int pivot = nums[hi];
        int pivotIndex = partition(nums, lo, hi, pivot);
        quickSort(nums, lo, pivotIndex-1);
        quickSort(nums, pivotIndex+1, hi);
    }

    public int partition(int[] arr, int lo, int hi, int pivot){
        int i = lo, j = lo;

        while(i <= hi){
            if(arr[i] > pivot){
                i++;
            } else{
                swap(arr, i, j);
                i++;
                j++;
            }
        }
        return j-1;
    }

    public void swap(int[] arr, int i, int j){
        int temp = arr[i];
        arr[i] = arr[j];
        arr[j] = temp;
    }
}
```