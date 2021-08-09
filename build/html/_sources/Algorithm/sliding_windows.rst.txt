=================
Sliding Windows
=================

----------
Overview
----------

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


--------------------------------------
1248. Count Number of Nice Subarrays
--------------------------------------

Given an array of integers nums and an integer k. A continuous subarray is called nice if there are k odd numbers on it.

Return the number of nice sub-arrays.


.. topic:: Example 1

    Input: nums = [1,1,2,1,1], k = 3

    Output: 2

    Explanation: The only sub-arrays with 3 odd numbers are [1,1,2,1] and [1,2,1,1].

.. topic:: Example 2

    Input: nums = [2,4,6], k = 1

    Output: 0

    Explanation: There is no odd numbers in the array.

.. topic:: Example 3

    Input: nums = [2,2,2,1,2,2,1,2,2,2], k = 2

    Output: 16
 

.. topic:: Constraints

    1 <= nums.length <= 50000

    1 <= nums[i] <= 10^5

    1 <= k <= nums.length

**Notes**:

1. We first expand the window to the right until we get k odd numbers.

2. Then we start to reduce the window from the left, at the same time increasing a local counter ``currentRst``.

    - For example, if our first window is [2, 2, 2, 1, 2, 2, 1]

    - As we move the left pointer, we get [2, 2, 1, 2, 2, 1], [2, 1, 2, 2, 1], [1, 2, 2, 1].

    - Our local counter is now 4.

3. Now our left counter is right after the first 1, making the window [2, 2, 1]. This means we start to move the right pointer again.

4. If nums[i] is even, that means we need to increase ``rst`` by ``currentRst``. This is because we can append the new even number to the previously obtained currentRst number of sequences to make new currentRst number of sequences.

    - For example, if ``i`` is again a 2, we add 4 more counts to ``rst``. The 4 new sequences are: [2, 2, 2, 1, 2, 2, 1, 2], [2, 2, 1, 2, 2, 1, 2], [2, 1, 2, 2, 1, 2], [1, 2, 2, 1, 2]. (note we add the 2 to the end of the previous 4 sequences)

5. If nums[i] is odd, we **cannot** increase ``rst`` by ``currentRst``. Instead, we need to reset ``currentRst`` to 0 because the previous sequences are useless now (if we append an odd number at the end, the count will exceed the limit)

    - For example, if ``i`` is a 1, we need to reset ``currentRst`` to 0. Because we cannot count [2, 2, 2, 1, 2, 2, 1, 1], [2, 2, 1, 2, 2, 1, 1], [2, 1, 2, 2, 1, 1], [1, 2, 2, 1, 1] to the result. 

    - In fact, now the window is a complete new window [2, 2, 1, 1], and in the while loop when we reduce the window, we will count the new sequences induced in this new window.

.. code-block:: java

    public int numberOfSubarrays(int[] nums, int k) {
        int j = 0;
        int rst = 0; 
        int currentOdd = 0;
        int currentRst = 0;
        
        for (int i =0; i<nums.length; i++) {
            //System.out.println("i: " + i);
       
            if (nums[i]%2 == 1) {
                currentOdd++;
                currentRst = 0;
            }
            
            //System.out.println("currentOdd!: "+currentOdd);
            
            while (currentOdd>=k && j < nums.length) {
                currentRst++;
                //System.out.println("j: " + j);
                //System.out.println("currentRst: "+currentRst);
                if (nums[j]%2 == 1) {
                    currentOdd--;
                    
                }
                j++;
            }
            rst += currentRst;
            
            //System.out.println("rst: "+rst);
        }
        return rst;
    }

