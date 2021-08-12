===============
Binary Search
===============

----------
Overview
----------

.

----------------------------
35. Search Insert Position
----------------------------

Given a sorted array of distinct integers and a target value, return the index if the target is found. If not, return the index where it would be if it were inserted in order.

You must write an algorithm with O(log n) runtime complexity.

.. topic:: Example 1

    Input: nums = [1,3,5,6], target = 5

    Output: 2

.. topic:: Example 2

    Input: nums = [1,3,5,6], target = 2

    Output: 1

.. topic:: Example 3

    Input: nums = [1,3,5,6], target = 7

    Output: 4

.. topic:: Example 4

    Input: nums = [1,3,5,6], target = 0

    Output: 0

.. topic:: Example 5

    Input: nums = [1], target = 0

    Output: 0
 
.. topic:: Constraints

    1 <= nums.length <= 104

    -104 <= nums[i] <= 104

    nums contains distinct values sorted in ascending order.

    -104 <= target <= 104

**Approach**: Invariant is rst (- [i, j). 

    - Hence j is initialized to nums.length. While loop is (i<j) and j = mid. Finally return j.

.. code-block:: java

    public int searchInsert(int[] nums, int target) {
        int i = 0;
        int j = nums.length;
        int mid;
        
        while (i<j) {
            mid = (i+j)/2;
            if (nums[mid] == target) {
                return mid;
            } else if (nums[mid] < target) {
                i = mid + 1;
            } else {
                j = mid;
            }
        }
        
        return j;
    }

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

--------------------------------
287. Find the Duplicate Number
--------------------------------

Given an array of integers nums containing n + 1 integers where each integer is in the range [1, n] inclusive.

There is only one repeated number in nums, return this repeated number.

You must solve the problem without modifying the array nums and uses only constant extra space.

 

.. topic:: Example 1

    Input: nums = [1,3,4,2,2]

    Output: 2

.. topic:: Example 2

    Input: nums = [3,1,3,4,2]

    Output: 3

.. topic:: Example 3

    Input: nums = [1,1]

    Output: 1

.. topic:: Example 4

    Input: nums = [1,1,2]

    Output: 1
 
.. topic:: Constraints

    1 <= n <= 105

    nums.length == n + 1

    1 <= nums[i] <= n

    All the integers in nums appear only once except for precisely one integer which appears two or more times.

**Approach**: Use binary search to find the number between 1 to n.

- Given a mid point m, we count the numbers that are less or equal to m. If the number is greater than m, that means our target number must be smaller than m.

    - e.g. For [1, 3, 4, 2, 2], we first test m = 2. There are 3 numbers (1,2,2) less or equal to 2. That means the target number is between 2 to 1 to 2. Because by pigeon hole principle if we want to fit 3 number into 2 boxes, there must be a repetition in them.

    - e.g. For [1, 3, 3, 2, 4], we again test with m = 2. There are 2 numbers (1, 2) less or equal to 2. That means we can fix these two numbers into 2 boxes perfectly and there's no repetition in them. Then repetition can only happen for the other part: from 3 to 4.

.. code-block:: java

    public int findDuplicate(int[] nums) {
        int i = 0;
        int j = nums.length-1;
        int mid;
        
        while (i<j) {
            mid = (i+j)/2;
            
            int count = 0;
            
            for (int k=0; k<nums.length; k++) {
                if (nums[k]<=mid) {
                    count++;
                }
            }
            
            if (count > mid) {
                j = mid;
            } else {
                i = mid+1;
            }
            
        }
        return i;
    }      

------------------------------
658. Find K Closest Elements
------------------------------

Given a sorted integer array arr, two integers k and x, return the k closest integers to x in the array. The result should also be sorted in ascending order.

An integer a is closer to x than an integer b if:

\|a - x\| < \|b - x\|, or
\|a - x\| == \|b - x\| and a < b
 

.. topic:: Example 1

    Input: arr = [1,2,3,4,5], k = 4, x = 3

    Output: [1,2,3,4]

.. topic:: Example 2

    Input: arr = [1,2,3,4,5], k = 4, x = -1

    Output: [1,2,3,4]
 

.. topic:: Constraints

    1 <= k <= arr.length

    1 <= arr.length <= 104

    arr is sorted in ascending order.

    -104 <= arr[i], x <= 104

**Approach**: Use binary search to find position of x in arr, or if it's not in the arr, find the position of the closest number. Then add this number to the rst. Create two pointers i, j, starting from the left and right of this number. Keep comparing the distance and add the closer one to the rst.

.. code-block:: java

    public List<Integer> findClosestElements(int[] arr, int k, int x) {
        
        if (x < arr[0]) {
            return toList(Arrays.copyOfRange(arr, 0, k));
        }
        
        if (x > arr[arr.length-1]) {
            return toList(Arrays.copyOfRange(arr, arr.length-k, arr.length));
        }
        
        int px = binarySearch(arr, x);
        
        int i = px-1;
        int j = px+1;
        int disI = -1;
        int disJ = -1;
        List<Integer> rst = new ArrayList<>();
        rst.add(arr[px]);
        
        while (rst.size() < k) {
            if (i >= 0) {
                disI = x - arr[i];
                //System.out.println("i: " + i + " px: "+px + " disI: " + disI);
            } else {
                disI = Integer.MAX_VALUE;
            }
            
            if (j <= arr.length - 1) {
                disJ = arr[j] - x;
                //System.out.println("j: " + j + " px: "+px + " disJ: " + disJ);
            } else {
                disJ = Integer.MAX_VALUE;
            }
            
            if (disI <= disJ) {
                rst.add(0, arr[i]);
                i--;
            } else {
                rst.add(arr[j]);
                j++;
            }   
        }
        
        return rst;
    }
    
    private int binarySearch(int[] arr, int x) {
        int i = 0;
        int j = arr.length;
        int mid;
        
        while (i<j) {
            mid = (i+j)/2;
            //System.out.println("i: " + i + " j: " + j + " mid: "+mid);
            if (arr[mid] < x) {
                i = mid + 1;
            } else if (arr[mid] > x){
                j = mid;
            } else {
                return mid;
            }
        }
        
        if (j==arr.length) {
            return -1;
        }
        
        if (arr[j] - x >= x-arr[j-1]) {
            return j-1;
        } else {
            return j;
        }
    }
    
    private List<Integer> toList(int[] ints) {
        List<Integer> intList = new ArrayList<Integer>(ints.length);
        for (int i : ints) {
            intList.add(i);
        }
        return intList;
    }

-------------------------------------------------------------
34. Find First and Last Position of Element in Sorted Array
-------------------------------------------------------------

Given an array of integers nums sorted in ascending order, find the starting and ending position of a given target value.

If target is not found in the array, return [-1, -1].

You must write an algorithm with O(log n) runtime complexity.

.. topic:: Example 1

    Input: nums = [5,7,7,8,8,10], target = 8

    Output: [3,4]

.. topic:: Example 2

    Input: nums = [5,7,7,8,8,10], target = 6

    Output: [-1,-1]

.. topic:: Example 3

    Input: nums = [], target = 0

    Output: [-1,-1]

.. topic:: Constraints

    0 <= nums.length <= 105

    -109 <= nums[i] <= 109

    nums is a non-decreasing array.

    -109 <= target <= 109

**Approach**: Use binary search to find the position of one of the target, then traverse to left and right to find the range of the positions.

.. code-block:: java

    public int[] searchRange(int[] nums, int target) {
        int i = 0;
        int j = nums.length;
        int mid;
        int t = -1;
        
        while (i<j) {
            mid = (i+j)/2;
            if (nums[mid] == target) {
                t = mid;
                break;
            } else if (nums[mid] < target) {
                i = mid + 1;
            } else {
                j = mid;
            }
        }
        
        //System.out.println("t: " + t);
        
        if (t == -1) {
            return new int[]{-1, -1};
        } else {
            int[] rst = new int[2];
            int k = 0;
            while (t-k >= 0 && nums[t-k] == target) {
                k++;
            }
            rst[0] = t-k+1;
            
            k = 0;
            while (t+k < nums.length && nums[t+k] == target) {
                k++;
            }
            rst[1] = t+k-1;
            return rst;
        }
    }

---------------------------
367. Valid Perfect Square
---------------------------

Given a positive integer num, write a function which returns True if num is a perfect square else False.

Follow up: Do not use any built-in library function such as sqrt.

.. topic:: Example 1

    Input: num = 16

    Output: true

.. topic:: Example 2

    Input: num = 14

    Output: false
 
.. topic:: Constraints

    1 <= num <= 2^31 - 1

.. code-block:: java

    public boolean isPerfectSquare(int num) {      
        int i = 1;
        int j = num+1;
        int mid;
    
        while (i<j) {
            mid = (i+j)/2;
            if (num/(double)mid == mid) {
                return true;
            } else if (num/(double)mid > mid) {
                i = mid+1;
            } else {
                j = mid;
            }
        }
        
        return false;
        
    }