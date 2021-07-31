==================================
Dynamic Programming
==================================

.. contents::
    :depth: 2

---------------------------------------
Overview
---------------------------------------

.

---------------------------------------
55. Jump Game
---------------------------------------

You are given an integer array nums. You are initially positioned at the array's first index, and each element in the array represents your maximum jump length at that position.

Return true if you can reach the last index, or false otherwise.


.. topic:: Example 1

    Input: nums = [2,3,1,1,4]

    Output: true

    Explanation: Jump 1 step from index 0 to 1, then 3 steps to the last index.

.. topic:: Example 2

    Input: nums = [3,2,1,0,4]

    Output: false

    Explanation: You will always arrive at index 3 no matter what. Its maximum jump length is 0, which makes it impossible to reach the last index.

**Approach 1**: 

- Maintain an array of booleans where at index i, it means "can I jump from position i to the end?".
- Loop backwards; begin by filling in the array from the end.
- jump[n-1] = true; (it's always true at the end position).
- At position i, we can jump from i+1 to i+nums[i] (or til the end, whichever is first). So we check if that position is true, if so, it means we can jump from i to that position then to the end.


.. code-block:: java

    class Solution {
        // at each position i, can I jump to the end?
        // jump[n-1] = true
        // jump[i] = j from i+1 to i + nums[i], jump[j] == true
        
        public boolean canJump(int[] nums) {
            boolean[] jump = new boolean[nums.length];
            jump[nums.length - 1] = true;
            
            for (int i = nums.length - 2; i>=0; i--) {
                boolean canJ = false;
                // System.out.println("i: " + i);
                // System.out.println("i + nums[i]: " + (i + nums[i]));
                for (int j = i+1; j < Math.min(nums.length, i+nums[i]+1); j++) {
                    //System.out.print("j: " + j + " ");
                    if (jump[j]) {
                        canJ = true;
                    }
                    
                }
                // System.out.println("");
                jump[i] = canJ;
            }
            
            return jump[0];
        }    
        
    }

**Approach 2**

- Only need to maintain one index ``canJump`` indicating the location from where can jump to the end.
- Loop backwards, at current location i, if we can jump to ``canJump``, then update ``canJump`` to the current location.
- If at the end canJump == 0, return true.


.. code-block:: java

    class Solution {
        public boolean canJump(int[] nums) {
            int canJump = nums.length - 1;
            
            for (int i = nums.length -1; i>=0; i--) {
                if (i + nums[i] >= canJump) {
                    canJump = i;
                }
            }
            
            return canJump==0;
        }    
    }
