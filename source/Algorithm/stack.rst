=======
Stack
=======

-----------------------------
496. Next Greater Element I
-----------------------------

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

**Approach:** 

- We set up a LIFO stack to get the next greater element for all of the numbers in nums2. The stack will be in a descending order (the bottom of the stack is always the largest). 

- Traverse from the right of nums2. Suppose the current index is j: 
    
    - If the stack is empty, we know that there's no element greater than the current value nums2[j].

    - If the top of the stack is greater than nums2[j], we know that this is the next greater element for j.

    - If the top of the stack is less than nums2[j], we need to remove the top of the stack until the top of the stack if greater than nums2[j] or is empty.

    - e.g. Suppose nums2 = [3, 4, 1]. When j = 2, the stack is empty so we put 1 in the stack and mark the nge of 1 as -1. Then when j = 1, the stack is [1]; 1 < 4; so we remove 1 and add 4 into the stack and mark 4 as -1. Then when j = 0, the stack is [4]; 3 < 4 so we mark the nge of 3 as 4 and the stack remains as [4]. We can remove the lesser numbers in the stack because say we have 4 at the bottom of the stack, any number to it's right that is less than 4 will not be considered as the nge of any number before 4. So it's safe to remove them when we are at 4.

.. code-block:: java

    public int[] nextGreaterElement(int[] nums1, int[] nums2) {
        int[] rst = new int[nums1.length];
        
        HashMap<Integer, Integer> nge = new HashMap<>();
        
        Stack<Integer> stack = new Stack<>();
        
        for (int j=nums2.length-1; j>=0; j--) {
            nge.put(nums2[j], -1);
            while (!stack.empty()) {
                if (stack.peek() > nums2[j]) {
                    nge.put(nums2[j], stack.peek());
                    break;
                } else {
                    stack.pop();
                }
            }
            
            stack.push(nums2[j]);
            
        } 
        
        // for (Map.Entry<Integer, Integer> entry : nge.entrySet()) {
        //     System.out.println(entry.getKey() + "/" + entry.getValue());
        // }
        
        for (int i=0; i< nums1.length; i++) {
            rst[i] = nge.get(nums1[i]);
        }

        return rst;
    }


------------------------------
503. Next Greater Element II
------------------------------ 

Given a circular integer array nums (i.e., the next element of nums[nums.length - 1] is nums[0]), return the next greater number for every element in nums.

The next greater number of a number x is the first greater number to its traversing-order next in the array, which means you could search circularly to find its next greater number. If it doesn't exist, return -1 for this number.

.. topic:: Example 1

    Input: nums = [1,2,1]

    Output: [2,-1,2]

    Explanation: The first 1's next greater number is 2; 

    The number 2 can't find next greater number. 

    The second 1's next greater number needs to search circularly, which is also 2.

.. topic:: Example 2

    Input: nums = [1,2,3,4,3]

    Output: [2,3,4,-1,4]
 
.. topic:: Constraints

    1 <= nums.length <= 104

    -109 <= nums[i] <= 109

**Approach:** The same idea as 496. Next Greater Element I. For circular check, just duplicate the input array. For example, if nums = [1, 2, 1], we just think of it as [1, 2, 1, 1, 2, 1] then do the calculation just as question 496.

.. code-block:: java

    public int[] nextGreaterElements(int[] nums) {
        int n = nums.length;
        int[] nge = new int[n];
        
        Stack<Integer> stack = new Stack<>();
        
        for (int j=2*n-1; j>=0; j--) {
            if (j<n) {
                 nge[j] = -1;
            }
           
            while (!stack.empty()) {
                if (stack.peek() > nums[j%n]) {
                    nge[j%n] = stack.peek();
                    break;
                } else {
                    stack.pop();
                }
            }
            
            stack.push(nums[j%n]);
            
        } 
        
        return nge;
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
 
Follow up: Can you solve it in O(n) time and O(1) space? See Array for an O(1) space solution. It uses more time and more space than this solution though.

**Approach**: Create two stacks and pass the strings to it. If we see a '#', remove the last added element from the stack. Then pop the elements in the two stacks to compare if they are equal.

.. code-block:: java

    public boolean backspaceCompare(String s, String t) {
        Stack<Character> stackS = new Stack<>();
        Stack<Character> stackT = new Stack<>();
        
        passStack(stackS, s);
        passStack(stackT, t);
        
        while (!stackS.empty() && !stackT.empty()) {
            Character s0 = stackS.pop();
            Character t0 = stackT.pop();
            if (s0 != t0) {
                return false;
            }
        }
        
        if (!stackS.empty() || !stackT.empty()) {
            return false;
        } else {
            return true;
        }
    }
    
    private void passStack(Stack<Character> stack, String s) {
        for (int i=0; i<s.length(); i++) {
            if (s.charAt(i) == '#') {
                if (!stack.empty()){
                    stack.pop();
                }
            } else {
                stack.push(s.charAt(i));
            }
        }
    }


-------------------------
739. Daily Temperatures
-------------------------

Given an array of integers temperatures represents the daily temperatures, return an array answer such that answer[i] is the number of days you have to wait after the ith day to get a warmer temperature. If there is no future day for which this is possible, keep answer[i] == 0 instead.

.. topic:: Example 1

    Input: temperatures = [73,74,75,71,69,72,76,73]

    Output: [1,1,4,2,1,1,0,0]

.. topic:: Example 2

    Input: temperatures = [30,40,50,60]

    Output: [1,1,1,0]

.. topic:: Example 3

    Input: temperatures = [30,60,90]

    Output: [1,1,0]

.. topic:: Constraints

    1 <= temperatures.length <= 105

    30 <= temperatures[i] <= 100

**Approach**: We keep a monotonically increasing stack that stores the index of temperature: when a new temperature with index i comes, check if it is greater than the top-most element j in the stack. If it is greater, we can pop j and set rst[j] to i-j, because that is how far away i is from j (i.e. how many days we need to wait after j to get to a greater temperature).

.. code-block:: java

    public int[] dailyTemperatures(int[] temperatures) {
        Stack<Integer> stack = new Stack<>();
        int[] answer = new int[temperatures.length];
        
        for (int i=0; i<temperatures.length; i++) {
            while (!stack.empty() && temperatures[stack.peek()] < temperatures[i]) {
                int j = stack.pop();
                answer[j] = i-j;
            }
            stack.push(i);
           
        }
        return answer;
    }

-------------------------
42. Trapping Rain Water
-------------------------

Given n non-negative integers representing an elevation map where the width of each bar is 1, compute how much water it can trap after raining.

.. topic:: Example 1

    Input: height = [0,1,0,2,1,0,1,3,2,1,2,1]

    Output: 6

    Explanation: The above elevation map (black section) is represented by array [0,1,0,2,1,0,1,3,2,1,2,1]. In this case, 6 units of rain water (blue section) are being trapped.

.. topic:: Example 2

    Input: height = [4,2,0,3,2,5]

    Output: 9
 
.. topic:: Constraints

    n == height.length

    1 <= n <= 2 * 104

    0 <= height[i] <= 105

**Approach**: use a monotonically decreasing stack storing the index of the bars. We have three situation for a new index i:

- The height of i is lower than the top of the stack: just push i into the stack.

- The height of i is the same as the top of the stack: refresh the top of the stack to i (pop and then push i). Because if two consecutive bars are at the same height, it cannot store waters in-between.

- The height of i is greater than the top of the stack:
    
    - the top of the stack now represents the lowest bar to the left of i. So we pop the top of the stack and call it lowest. 

    - then the next top of the stack is the left bar. We want to calculate how many water can store between left bar and right bar (i). So calculate the width (right-left-i) and height (the lower one between left and right bar minus the height of the lowest point), and multiply to get the water and add to rst.

        - if the stack is empty, i now becomes the only element of the stack and we can continue.

.. code-block:: java

    public int trap(int[] height) {
        Stack<Integer> stack = new Stack<>();
        int rst = 0;
        stack.push(0);
        
        for (int i=1; i<height.length; i++) {
            System.out.println("i: " + i);
            if (!stack.empty() && height[stack.peek()] > height[i]) {
                stack.push(i);
            } else if (!stack.empty() && height[stack.peek()] == height[i]) {
                stack.pop();
                stack.push(i);
            } else {
                while (!stack.empty() && height[stack.peek()] < height[i]) {
                     int lowest = stack.pop();
                     System.out.println("lowest: "+ lowest);
                     if (!stack.empty()) {
                         int left = stack.peek();
                         int w = i-left-1;
                         int h = Math.min(height[i], height[left]) - height[lowest];
                         System.out.println("w: "+ w + " h: " + h + " i:" + i + " left: " + left);
                         rst += w*h;
                     }
                 }
                stack.push(i);
            }
           
        }
        
        return rst;
        
    }

**Simplified version**:

.. code-block:: java

    public int trap(int[] height) {
        Stack<Integer> stack = new Stack<>();
        int rst = 0;
        stack.push(0);
        
        for (int i=1; i<height.length; i++) {
            //System.out.println("i: " + i);
            while (!stack.empty() && height[stack.peek()] <= height[i]) {
                int lowest = stack.pop();
                if (!stack.empty()) {
                    int left = stack.peek();
                    int w = i-left-1;
                    int h = Math.min(height[i], height[left]) - height[lowest];
                    rst += w*h;
                }
            }
            stack.push(i);
           
        }
        
        return rst;
        
    }