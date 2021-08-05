=====================
Hash Table
=====================

.. contents::
    :depth: 2

----------------------------
41. First Missing Positive
----------------------------

Given an unsorted integer array nums, return the smallest missing positive integer.

You must implement an algorithm that runs in O(n) time and uses constant extra space.

.. topic:: Example 1

	Input: nums = [1,2,0]

	Output: 3


.. topic:: Example 2

	Input: nums = [3,4,-1,1]
	
	Output: 2

.. topic:: Example 3

	Input: nums = [7,8,9,11,12]

	Output: 1
 

.. topic:: Constraints

	1 <= nums.length <= 5 * 105

	-231 <= nums[i] <= 231 - 1

**Approach**: Given an array of length n, the missing positive integer must be 1 - n+1. For example, for length 5, to get the largest missing positive integer, the array must be [1,2,3,4,5], then the rst is 6. So we can create a array of length n, where index i means i+1 is not missing. For example, rst[0] = 1 means 1 is given. rst[1] = 0 means 2 is not given. Then we traverse nums once to fill in rst, then traverse rst to get the first index with 0. 

.. code-block:: java

    public int firstMissingPositive(int[] nums) {
        int n = nums.length;
        int[] rst = new int[n];
        
        for (int i : nums) {
            if (i<=n && i >= 1) {
                rst[i-1] = 1;
            }
        }
        
        for (int j=0; j<n; j++) {
            if (rst[j] == 0) {
                return j+1;
            }
        }
        
        return n+1;
    }
