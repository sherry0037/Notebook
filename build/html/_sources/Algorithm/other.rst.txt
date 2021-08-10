=======
Other
=======

-------------------------------------------
153. Find Minimum in Rotated Sorted Array
-------------------------------------------

**Faster way is to do binary search. See the Binary Search page.**

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

    class Solution {
        public int findMin(int[] nums) {
            while (nums[nums.length-1] < nums[0]) {
                rotateBack(nums);
            }
            
            return nums[0];
        }
    
        private void rotateBack(int[] nums) {
            int temp = nums[0];
            for (int i=1; i<nums.length; i++) {
                nums[i-1] = nums[i];
            }
            nums[nums.length-1] = temp;
        }
    }

----------------------------------------------
154. Find Minimum in Rotated Sorted Array II
----------------------------------------------

Suppose an array of length n sorted in ascending order is rotated between 1 and n times. For example, the array nums = [0,1,4,4,5,6,7] might become:

[4,5,6,7,0,1,4] if it was rotated 4 times.

[0,1,4,4,5,6,7] if it was rotated 7 times.

Notice that rotating an array [a[0], a[1], a[2], ..., a[n-1]] 1 time results in the array [a[n-1], a[0], a[1], a[2], ..., a[n-2]].

Given the sorted rotated array nums that may contain duplicates, return the minimum element of this array.

You must decrease the overall operation steps as much as possible.


.. topic:: Example 1

    Input: nums = [1,3,5]

    Output: 1

.. topic:: Example 2

    Input: nums = [2,2,2,0,1]

    Output: 0

.. topic:: Constraints

    n == nums.length

    1 <= n <= 5000

    -5000 <= nums[i] <= 5000

    nums is sorted and rotated between 1 and n times.


.. code-block:: java

    public int findMin(int[] nums) {
        for (int i=1; i<nums.length; i++) {
            if (nums[i] < nums[i-1]) {
                return nums[i];
            }
        }
        return nums[0];
    }

-----------------------------
496. Next Greater Element I
-----------------------------

**See Stack for a better solution using stack.**

The next greater element of some element x in an array is the first greater element that is to the right of x in the same array.

You are given two distinct 0-indexed integer arrays nums1 and nums2, where nums1 is a subset of nums2.

For each 0 <= i < nums1.length, find the index j such that nums1[i] == nums2[j] and determine the next greater element of nums2[j] in nums2. If there is no next greater element, then the answer for this query is -1.

Return an array ans of length nums1.length such that ans[i] is the next greater element as described above.

 

.. topic:: Example 1

    Input: nums1 = [4,1,2], nums2 = [1,3,4,2]

    Output: [-1,3,-1]

    Explanation: The next greater element for each value of nums1 is as follows:

        - 4 is underlined in nums2 = [1,3,4,2]. There is no next greater element, so the answer is -1.

        - 1 is underlined in nums2 = [1,3,4,2]. The next greater element is 3.

        - 2 is underlined in nums2 = [1,3,4,2]. There is no next greater element, so the answer is -1.

.. topic:: Example 2

    Input: nums1 = [2,4], nums2 = [1,2,3,4]

    Output: [3,-1]

    Explanation: The next greater element for each value of nums1 is as follows:

        - 2 is underlined in nums2 = [1,2,3,4]. The next greater element is 3.

        - 4 is underlined in nums2 = [1,2,3,4]. There is no next greater element, so the answer is -1.
 

.. topic:: Constraints

    1 <= nums1.length <= nums2.length <= 1000

    0 <= nums1[i], nums2[i] <= 104

    All integers in nums1 and nums2 are unique.

    All the integers of nums1 also appear in nums2.
     

Follow up: Could you find an O(nums1.length + nums2.length) solution?

.. code-block:: java

    public int[] nextGreaterElement(int[] nums1, int[] nums2) {
        int[] rst = new int[nums1.length];
        
        
        for (int i=0; i<nums1.length; i++) {
            boolean found = false;
            rst[i] = -1;
            for (int j : nums2) {
                if (!found && j!=nums1[i]) {
                    continue;
                }
                
                if (j == nums1[i]) {
                    found = true;
                    continue;
                }
                
                if (j > nums1[i]) {
                    rst[i] = j;
                    break;
                }
            }
        }

        return rst;
    }

