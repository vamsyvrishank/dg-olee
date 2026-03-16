---
{"dg-publish":true,"permalink":"/personal-vault/dsa/to-be-revisited/acquire-and-release-strategy/"}
---


[Pepcoding Video Explaination](https://www.youtube.com/watch?v=x8O9XocEioI&list=PL-Jc9J83PIiEp9DKNiaQyjuDeg3XSoVMR&index=4)

*The generalized approach for Acquire and Release strategy :*

Certainly! Here's a generalized description of the acquire and release strategy:

1. **Initialization:**
  - Initialize two pointers, `start` and `end`, representing the window boundaries.
  - Initialize any necessary variables to track conditions within the window.
  - Initialize a result variable to store the maximum window size.

2. **Main Loop (While within array bounds):**
  - **Acquire:**
    - Move the `end` pointer forward to expand the window.
    - Update any relevant variables based on the newly added element.

  - **Release (if necessary):**
    - While the window is invalid or violates certain conditions:
      - Move the `start` pointer forward to release elements from the window.
      - Update relevant variables accordingly.

  - **Calculate Window Size:**
    - Determine the size of the current window using the `start` and `end` pointers.
    - Window Size is : ==`(end - 1) - (start + 1) - 1`== or ==`end - start - 1`==.

  - **Update Result:**
    - Update the result variable based on the current window size and the previous result.

3. **Return Result:**
  - The final result represents the desired outcome based on the window criteria.

This general strategy can be adapted for various problems that involve maintaining a valid window or subarray while iterating through an array. The specific conditions for acquisition, release, and window size calculation will depend on the requirements of the problem at hand.