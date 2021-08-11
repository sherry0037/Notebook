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

-------------------------------
556. Next Greater Element III
-------------------------------

Given a positive integer n, find the smallest integer which has exactly the same digits existing in the integer n and is greater in value than n. If no such positive integer exists, return -1.

Note that the returned integer should fit in 32-bit integer, if there is a valid answer but it does not fit in 32-bit integer, return -1.

.. topic:: Example 1

    Input: n = 12

    Output: 21

.. topic:: Example 2

    Input: n = 21

    Output: -1
 
.. topic:: Constraints

    1 <= n <= 231 - 1


**Approach:**

- First traverse from right to left to find the first digit that is less than the digit at it's right. We call it ``first``.

    - e.g. Given 12443322, the result is 13222344. We first locate 2 in 1 **2** 443322. 

- Then we need to find the next greater element to the right of 2.

    - e.g. We locate that 3 is the next greater element for 2: 1 **2** 443 **3** 22.

- Swap these two

    - The digits now becomes 1 **3** 443 **2** 22.

- Then all the numbers to the right of ``first`` need to be swapped to ascending order and we are done.

    -  13 **443222** needs to be changed to 13 **222344**.

- Catch 1: if the int is already in descending order, meaning it's already the largest, then return -1. So if we couldn't locate ``first``, we need to return -1.

- Catch 2: if result is greater than Integer.MAX_VALUE, we need to return -1. So we need to first convert the result into long, then compare the long version of Integer.MAX_VALUE, then output accordingly.

.. code-block:: java

    public int nextGreaterElement(int n) {
        
        int[] digits = Integer.toString(n).chars().map(c -> c-'0').toArray();
        
        if (digits.length <=1) {
            return -1;
        }
        
        int first = -1;
        
        for (int i=digits.length-2; i>=0; i--) {
            if (digits[i] < digits[i+1]) {
                first = i;
                break;
            }
        }
        
        if (first == -1) {
            return -1;
        }
    
        // Tried binary search but failed
    //         int i = first+1;
    //         int j = digits.length-1;
    //         int mid;
    //         while (i<j) {
    //             mid = (i+j)/2;
    //             System.out.println("i: "+i+" j: " + j + " mid: " + mid);
    //             if (digits[mid] > digits[first]) {
    //                 i = mid;
    //             } else {
    //                 j = mid-1;
    //             }
    //         }
                
    //         int second = i;  
        
        int j = digits.length - 1;
        
        while (j>first && digits[j]<=digits[first]) {
            j--;
        }
        
        int second = j;
        
        //System.out.println("first: "+first+" second: " + second);    
        int temp = digits[first];
        digits[first] = digits[second];
        digits[second] = temp;
        
        int N = first+digits.length;
        //System.out.println("N: "+N);
        for (int k = first+1; k<digits.length; k++) {
            temp = digits[k];
            //System.out.println("k: " + k + " N-k: "+(N-k));
            digits[k] = digits[N-k];
            digits[N-k] = temp;
            if ((N-k)-k == 1 || k == (N-k)) {
                break;
            }
        }
        
        long rst = arrayToInt(digits);
        long max = (long)Integer.MAX_VALUE;
        
        return rst>max?-1:(int)rst;
            
    }
    
    private long arrayToInt(int[] arr) {
        StringBuilder s = new StringBuilder(); 
        for (int i : arr) {
             s.append(i);
        }
        //System.out.println(s);
        return Long.parseLong(s.toString());
    }
