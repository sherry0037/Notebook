===============
Binary Search
===============

----------
Overview
----------

.

-------------
69. Sqrt(x)
-------------

Given a non-negative integer x, compute and return the square root of x.

Since the return type is an integer, the decimal digits are truncated, and only the integer part of the result is returned.

Note: You are not allowed to use any built-in exponent function or operator, such as pow(x, 0.5) or x ** 0.5.
 
.. topic:: Example 1

    Input: x = 4

    Output: 2

.. topic:: Example 2

    Input: x = 8

    Output: 2

    Explanation: The square root of 8 is 2.82842..., and since the decimal part is truncated, 2 is returned.
     
.. topic:: Constraints

    0 <= x <= 231 - 1

.. code-block:: java

    public int mySqrt(int x) {
        long i = 0;
        long j = x;
        int mid;
        
        if (x == 0) {
            return 0;
        }
        
        if (x == 1) {
            return 1;
        }
        
        while (i <= j) {
            mid = (int)(i + j)/2;
            if (x/mid == mid) { // Need to use x/mid instead of mid*mid
                return (int)mid;
            } else if (x/mid > mid) { 
                i = mid + 1;
            } else if (x/mid < mid) {
                j = mid - 1;
            }
        }
        return (int)j; // Output is j because when mid^2 > x and j-1, j is left of i
        
    }


-------------------------------------------
153. Find Minimum in Rotated Sorted Array
-------------------------------------------

Suppose an array of length n sorted in ascending order is rotated between 1 and n times. For example, the array nums = [0,1,2,4,5,6,7] might become:

[4,5,6,7,0,1,2] if it was rotated 4 times.

[0,1,2,4,5,6,7] if it was rotated 7 times.

Notice that rotating an array [a[0], a[1], a[2], ..., a[n-1]] 1 time results in the array [a[n-1], a[0], a[1], a[2], ..., a[n-2]].

Given the sorted rotated array nums of unique elements, return the minimum element of this array.

You must write an algorithm that runs in O(log n) time.


.. topic:: Example 1

    Input: nums = [3,4,5,1,2]

    Output: 1

    Explanation: The original array was [1,2,3,4,5] rotated 3 times.

.. topic:: Example 2

    Input: nums = [4,5,6,7,0,1,2]

    Output: 0

    Explanation: The original array was [0,1,2,4,5,6,7] and it was rotated 4 times.

.. topic:: Example 3

    Input: nums = [11,13,15,17]

    Output: 11

    Explanation: The original array was [11,13,15,17] and it was rotated 4 times. 
 
.. topic:: Constraints

    n == nums.length

    1 <= n <= 5000

    -5000 <= nums[i] <= 5000

    All the integers of nums are unique.

    nums is sorted and rotated between 1 and n times.

.. code-block:: java

    public int findMin(int[] nums) {
        int i = 0;
        int j = nums.length-1;
        int mid = 0;
        
        if (nums.length == 1) {
            return nums[0];
        }
        
        if (nums.length == 2) {
            return Math.min(nums[0], nums[1]);
        }
        
        while (i<j) {
            mid = (int) (i+j)/2;
            if (mid == 0) {
                return Math.min(nums[0], nums[1]);
            }
            
            if (nums[mid]<nums[mid-1]) {
                return nums[mid];
            }

            if (nums[mid] > nums[j]) {
                i = mid+1;
            } else if (nums[mid] < nums[j]) {
                j = mid-1;
            }
        
        }
        return nums[j];
        
    }
