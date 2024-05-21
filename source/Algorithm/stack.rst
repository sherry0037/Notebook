=============
Stack
=============

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

-------------------------
735. Asteroid Collision
-------------------------

We are given an array asteroids of integers representing asteroids in a row.

For each asteroid, the absolute value represents its size, and the sign represents its direction (positive meaning right, negative meaning left). Each asteroid moves at the same speed.

Find out the state of the asteroids after all collisions. If two asteroids meet, the smaller one will explode. If both are the same size, both will explode. Two asteroids moving in the same direction will never meet.

.. topic:: Example 1

    Input: asteroids = [5,10,-5]

    Output: [5,10]

    Explanation: The 10 and -5 collide resulting in 10. The 5 and 10 never collide.

.. topic:: Example 2

    Input: asteroids = [8,-8]

    Output: []

    Explanation: The 8 and -8 collide exploding each other.

.. topic:: Example 3

    Input: asteroids = [10,2,-5]

    Output: [10]

    Explanation: The 2 and -5 collide resulting in -5. The 10 and -5 collide resulting in 10.

.. topic:: Example 4

    Input: asteroids = [-2,-1,1,2]

    Output: [-2,-1,1,2]

    Explanation: The -2 and -1 are moving left, while the 1 and 2 are moving right. Asteroids moving the same direction never meet, so no asteroids will meet each other.
 

.. topic:: Constraints

    2 <= asteroids.length <= 104

    -1000 <= asteroids[i] <= 1000

    asteroids[i] != 0

.. code-block:: java
    
    public int[] asteroidCollision(int[] asteroids) {
        Stack<Integer> stack = new Stack<>();
        stack.push(asteroids[0]);
        
        for (int i=1; i<asteroids.length; i++) {
            Integer curr = asteroids[i];
            
            // No existing asteroid
            if (stack.empty()) {
                stack.push(asteroids[i]);
                continue;
            }
            Integer pre = (Integer) stack.peek();
            
            System.out.println("Pre: " + pre +  " Curr: " + curr );
            
            // Incoming asteroid is larger
            while (pre > 0 && -curr > pre && !stack.empty()) {
                stack.pop();
                if (stack.empty()) {
                    stack.push(curr);
                    break;
                }
                pre = (Integer) stack.peek();
                System.out.println("pre: " + pre);
            } 
            
            // Incoming asteroid is the same
            if (pre > 0 && -curr == pre) {
                stack.pop();
            } 
            
            // Asteroids are moving away from each other
            if (pre < 0 || curr > 0) {
                stack.push(curr);
            }
            
        }
        
        int[] rst = new int[stack.size()];
        for (int i=stack.size()-1; i>=0; i--) {
            rst[i] = stack.pop();
        }
        
        return rst;
    }

-----------------------------
394. Decode String (Medium)
-----------------------------

Given an encoded string, return its decoded string.

The encoding rule is: k[encoded_string], where the encoded_string inside the square brackets is being repeated exactly k times. Note that k is guaranteed to be a positive integer.

You may assume that the input string is always valid; there are no extra white spaces, square brackets are well-formed, etc. Furthermore, you may assume that the original data does not contain any digits and that digits are only for those repeat numbers, k. For example, there will not be input like 3a or 2[4].

The test cases are generated so that the length of the output will never exceed 105.


.. topic:: Example 1

    Input: s = "3[a]2[bc]"
    Output: "aaabcbc"

.. topic:: Example 2

    Input: s = "3[a2[c]]"
    Output: "accaccacc"

.. topic:: Example 3

    Input: s = "2[abc]3[cd]ef"
    Output: "abcabccdcdcdef"

.. topic:: Constraints

    1 <= s.length <= 30
    s consists of lowercase English letters, digits, and square brackets '[]'.
    s is guaranteed to be a valid input.
    All the integers in s are in the range [1, 300].

.. code-block:: java

    // Note: better to re-implemente this with stack instead of queue.

	class Solution {
        public String decodeString(String input) {
            LinkedList<String> queue = new LinkedList<>();
            LinkedList<Integer> countQueue = new LinkedList<>();
            List<String> strList = new ArrayList<>();
            List<String> storeDigits = new ArrayList<>();
            for (int i = 0; i < input.length(); i++) {
                char currChar = input.charAt(i);
                String curr = String.valueOf(input.charAt(i));
                if (Character.isLetter(currChar) || curr.equals("[")) {
                    queue.addFirst(curr);
                } else if (Character.isDigit(currChar)) { 
                    // Digits could be in the renage [1, 300]
                    storeDigits.add(curr);
                    if (i+1 < input.length() && !Character.isDigit(input.charAt(i+1))) {
                        int digitToAdd = Integer.valueOf(String.join("", storeDigits));
                        countQueue.addFirst(digitToAdd);
                        storeDigits.clear();
                    }
                } else {
                    // curr.equals("]") 
                    // example: 2[bc]
                    // queue = (c,b,[)
                    String next = queue.poll();
                    while (!next.equals("[")) {
                        strList.add(next);
                        next = queue.poll();
                    }
                    // strList = [c, b]
                    Collections.reverse(strList);
                    // strList = [b, c]
                    int count = Integer.valueOf(countQueue.pop()); // count = 2;
                    List<String> strLocalList = new ArrayList<>();
                    while (count > 0) {
                        strLocalList.addAll(strList);
                        count--;
                    } // strLocalList = [b, c, b, c];
                    for (int j = 0; j < strLocalList.size(); j++) {
                        queue.addFirst(strLocalList.get(j));
                    } // // queue = (c, b, c, b)
                    strList.clear();
                }
                // System.out.println(queue);
            }

            List<String> strRst = new ArrayList<>();
            while (!queue.isEmpty()) {
                String currStr = queue.pop();
                strRst.add(currStr);
            }
            Collections.reverse(strRst);
            return String.join("", strRst);
        }   
    }

