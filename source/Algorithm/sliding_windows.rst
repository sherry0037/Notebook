==================================
Sliding Windows
==================================

-----------------------------------------
Overview
-----------------------------------------

Problems that require to find longest/shortest substring with some constraints can be solved by the sliding windows technique.

Approach: 

- Have two pointers, i, j. i is the right edge of the window, j is the left edge of the window.
- Have a hashmap or a counter to record some conditions.
- Have a placeholder for final result.
- Start by a "for" loop on i to increase window size to the right.
    - In a "while" loop, reduce the window size from the left (increase j) when certain constraint is met.
        - One condition is often ``j < n``.
        - Reduce the counter size
    - Compare and set the result accordingly.
- Return result.

