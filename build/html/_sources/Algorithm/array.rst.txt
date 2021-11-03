=======
Array
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

--------------------
27. Remove Element
--------------------

Given an integer array nums and an integer val, remove all occurrences of val in nums in-place. The relative order of the elements may be changed.

Since it is impossible to change the length of the array in some languages, you must instead have the result be placed in the first part of the array nums. More formally, if there are k elements after removing the duplicates, then the first k elements of nums should hold the final result. It does not matter what you leave beyond the first k elements.

Return k after placing the final result in the first k slots of nums.

Do not allocate extra space for another array. You must do this by modifying the input array in-place with O(1) extra memory.

.. topic:: Example 1

    Input: nums = [3,2,2,3], val = 3

    Output: 2, nums = [2,2,_,_]

    Explanation: Your function should return k = 2, with the first two elements of nums being 2.

    It does not matter what you leave beyond the returned k (hence they are underscores).

.. topic:: Example 2

    Input: nums = [0,1,2,2,3,0,4,2], val = 2

    Output: 5, nums = [0,1,4,0,3,_,_,_]

    Explanation: Your function should return k = 5, with the first five elements of nums containing 0, 0, 1, 3, and 4.

    Note that the five elements can be returned in any order.

    It does not matter what you leave beyond the returned k (hence they are underscores).
     
.. topic:: Constraints

    0 <= nums.length <= 100

    0 <= nums[i] <= 50

    0 <= val <= 100


**Approach**: use two pointers. One to traverse the input nums, one to set the value whenever it's not equal to val.

.. code-block:: java

    public int removeElement(int[] nums, int val) {
        int j = 0;
        for (int i=0; i<nums.length; i++) {
            if (nums[i] != val) {
                nums[j] = nums[i];
                j++;
            }
        }
        return j;
    }

------------------
283. Move Zeroes
------------------

Given an integer array nums, move all 0's to the end of it while maintaining the relative order of the non-zero elements.

Note that you must do this in-place without making a copy of the array.

.. topic:: Example 1

    Input: nums = [0,1,0,3,12]

    Output: [1,3,12,0,0]

.. topic:: Example 2

    Input: nums = [0]

    Output: [0]
 
.. topic:: Constraints

    1 <= nums.length <= 104

    -231 <= nums[i] <= 231 - 1

**Approach 1**: Idea is the same as 27. Remove Element: use two pointers, one to traverse the input nums, one to set the value whenever it's not equal to val.

.. code-block:: java

    public void moveZeroes(int[] nums) {
        int j = 0;
        for (int i=0; i<nums.length; i++) {
            if (nums[i] != 0) {
                nums[j] = nums[i];
                j++;
            }
        }
        
        while (j<nums.length) {
            nums[j] = 0;
            j++;
        }
    }

**Approach 2** This is **slower** than approach 1. Traverse from right to left. Find the first zero, then mark the position just right to the first zero as ``nonZeroPos``. Then keep traverse until meet another nonzero. Then we move all the things from the right forward.

    - e.g. For [1, 0, 0, 0, 2, 3], we first find position 4, then find position 1, which means we need to move all the elements starting from 4 forward to position 1, then fill in zeros to the right. This should return [1, 2, 3, 0, 0, 0].

.. code-block:: java

    public void moveZeroes(int[] nums) {
        int nonZeroPos = -1;
        for (int i = nums.length-1; i>=0; i--) {
            if (nums[i]!=0 && nonZeroPos!=-1) {
                // find the leftmost position of zeros
                moveForward(nums, nonZeroPos, i+1);
                nonZeroPos = -1;
            } else if (nums[i] == 0 && nonZeroPos == -1) {
                // find the first zero
                nonZeroPos = i+1;
            }
        }
        
        if (nonZeroPos != -1) {
            moveForward(nums, nonZeroPos, 0);
        }
    }
    
    private void moveForward(int[] nums, int from, int to) {
        // [1, 0, 0, 0, 2, 3], from=4, to=1
        // return [1, 2, 3, 0, 0, 0]
        
        if (from < 0 || from >= nums.length || to < 0 || to >= nums.length
           || from < to) {
            return;
        }
        
        for (int i=0; i<nums.length-to; i++) {
            if (i<nums.length-from) {
                nums[to+i] = nums[from+i];
            } else {
                nums[to+i] = 0;
            }
        }
    }

-------------------------------
844. Backspace String Compare
-------------------------------

Given two strings s and t, return true if they are equal when both are typed into empty text editors. '#' means a backspace character.

Note that after backspacing an empty text, the text will continue empty.

.. topic:: Example 1

    Input: s = "ab#c", t = "ad#c"

    Output: true

    Explanation: Both s and t become "ac".

.. topic:: Example 2

    Input: s = "ab##", t = "c#d#"

    Output: true

    Explanation: Both s and t become "".

.. topic:: Example 3

    Input: s = "a##c", t = "#a#c"

    Output: true

    Explanation: Both s and t become "c".

.. topic:: Example 4

    Input: s = "a#c", t = "b"

    Output: false

    Explanation: s becomes "c" while t becomes "b".

.. topic:: Constraints

    1 <= s.length, t.length <= 200

    s and t only contain lowercase letters and '#' characters.
 
Follow up: Can you solve it in O(n) time and O(1) space?

**Approach**: Use two pointers, start from the right of the two strings and move leftward. If sees a '#', keep moving until all the backspace are dealt with. 

.. code-block:: java

    public boolean backspaceCompare(String s, String t) {
        int i = s.length()-1;
        int j = t.length()-1;
        
        while (i>=0 && j>=0) {
            //System.out.println("i: "+i + " j: "+j);
            if (i>=0 && s.charAt(i)=='#') {
                i = moveLeft(s, i);
                //System.out.println("i moved to: "+i);
            }
            
            if (j>=0 && t.charAt(j)=='#') {
                j = moveLeft(t, j);
                //System.out.println("j moved to: "+j);
            }
            
            if (i>=0 && j>=0) {
                if (s.charAt(i)==t.charAt(j)) {
                    i--;
                    j--;
                } else {
                    return false;
                }
            }
        }
        
        if (i<0 && j>=0 && t.charAt(j)=='#') {
            j = moveLeft(t, j);
        } else if (j<0 && i>=0 && s.charAt(i)=='#'){
            i = moveLeft(s, i);
        }
        
        return (i==-1 && j==-1);
    
    }
    
    private int moveLeft(String s, int i) {
        int count = 0;
        for (int p=i; p>=0; p--) {
            if (s.charAt(p) == '#') {
                count++;
            } else {
                count--;
                if (count==0) {
                    if (p>0 && s.charAt(p-1) != '#') {
                       return p-1; 
                    }
                }
            }
        }
        
        return -1;
    }

--------------------------------
977. Squares of a Sorted Array
--------------------------------

**Another solution using binary search. It's about the same time and space.**

Given an integer array nums sorted in non-decreasing order, return an array of the squares of each number sorted in non-decreasing order.

.. topic:: Example 1

    Input: nums = [-4,-1,0,3,10]

    Output: [0,1,9,16,100]

    Explanation: After squaring, the array becomes [16,1,0,9,100].

    After sorting, it becomes [0,1,9,16,100].

.. topic:: Example 2

    Input: nums = [-7,-3,2,3,11]

    Output: [4,9,9,49,121]
 
.. topic:: Constraints

    1 <= nums.length <= 104

    -104 <= nums[i] <= 104

    nums is sorted in non-decreasing order.
     

Follow up: Squaring each element and sorting the new array is very trivial, could you find an O(n) solution using a different approach?

**Approach**: First squaring all of the elements in nums in place. Then start from the left most and right most, fill in the rst with the larger one from the right of rst.

.. code-block:: java

    public int[] sortedSquares(int[] nums) {
        int[] rst = new int[nums.length];
        for (int i=0; i<nums.length; i++){
            nums[i] = nums[i]*nums[i];
        }
        
        int i=0;
        int j=nums.length-1;
        
        int k=nums.length-1;
        while (k>=0 && i<=j) {
            if (nums[i] >= nums[j]) {
                rst[k] = nums[i];
                i++;
            } else {
                rst[k] = nums[j];
                j--;
            }
            k--;
        }
        
        return rst;
    }

--------------------------------------------------------
1481. Least Number of Unique Integers after K Removals
--------------------------------------------------------

Given an array of integers arr and an integer k. Find the least number of unique integers after removing exactly k elements.

.. topic:: Example 1:

    Input: arr = [5,5,4], k = 1

    Output: 1

    Explanation: Remove the single 4, only 5 is left.

.. topic:: Example 2:

    Input: arr = [4,3,1,1,3,3,2], k = 3

    Output: 2

    Explanation: Remove 4, 2 and either one of the two 1s or three 3s. 1 and 3 will be left.

.. topic:: Constraints

    1 <= arr.length <= 10^5

    1 <= arr[i] <= 10^9

    0 <= k <= arr.length

**Note**: this is an example of using Arrays comparator. Using PriorityQueue is probably faster.

.. code-block:: java

    public int findLeastNumOfUniqueInts(int[] arr, int k) {
        HashMap<Integer, Integer> counts = new HashMap<>();
        Integer[] nums = new Integer[arr.length];
        
        int j = 0;
        for (int i : arr) {
            nums[j] = i;
            j++;
            counts.put(i, counts.getOrDefault(i, 0)+1);
        }
        
        Arrays.sort(nums, new Comparator<Integer>() {
            public int compare(Integer a, Integer b) {
                int c1 = counts.get(a);
                int c2 = counts.get(b);
                return (c1 != c2) ? (c1 - c2) : (a - b);
            }
        });  
        
        for (int i=0; i<k; i++) {
            counts.put(nums[i], counts.get(nums[i])-1);
            if (counts.get(nums[i]) == 0) {
                counts.remove(nums[i]);
            }
        }

        return counts.keySet().size();
    }

----------------------------------------------------------------------------
1509. Minimum Difference Between Largest and Smallest Value in Three Moves
----------------------------------------------------------------------------

Given an array nums, you are allowed to choose one element of nums and change it by any value in one move.

Return the minimum difference between the largest and smallest value of nums after perfoming at most 3 moves.

.. topic:: Example 1

    Input: nums = [5,3,2,4]

    Output: 0

    Explanation: Change the array [5,3,2,4] to [2,2,2,2].

    The difference between the maximum and minimum is 2-2 = 0.

.. topic:: Example 2

    Input: nums = [1,5,0,10,14]

    Output: 1

    Explanation: Change the array [1,5,0,10,14] to [1,1,0,1,1]. 

    The difference between the maximum and minimum is 1-0 = 1.

.. topic:: Example 3

    Input: nums = [6,6,0,1,1,4,6]

    Output: 2

.. topic:: Example 4

    Input: nums = [1,5,6,14,15]

    Output: 1

.. topic:: Constraints

    1 <= nums.length <= 10^5

    -10^9 <= nums[i] <= 10^9

.. code-block:: java

    public int minDifference(int[] nums) {
        if (nums.length <= 4) {
            return 0;
        }
        Arrays.sort(nums);
        int n = nums.length;
        
        // Remove 0, 1, 2
        int rst = nums[n-1] - nums[3];
        // Remove 0, 1, -1
        rst = Math.min(nums[n-2] - nums[2], rst);
        // Remove 0, -1, -2
        rst = Math.min(nums[n-3] - nums[1], rst);
        // Remove -1, -2, -3
        rst = Math.min(nums[n-4] - nums[0], rst);
        
        return rst;
    }
    