========
Greedy
========

---------------------
455. Assign Cookies
---------------------

Assume you are an awesome parent and want to give your children some cookies. But, you should give each child at most one cookie.

Each child i has a greed factor g[i], which is the minimum size of a cookie that the child will be content with; and each cookie j has a size s[j]. If s[j] >= g[i], we can assign the cookie j to the child i, and the child i will be content. Your goal is to maximize the number of your content children and output the maximum number.

.. topic:: Example 1

    Input: g = [1,2,3], s = [1,1]

    Output: 1

    Explanation: You have 3 children and 2 cookies. The greed factors of 3 children are 1, 2, 3. 

    And even though you have 2 cookies, since their size is both 1, you could only make the child whose greed factor is 1 content.

    You need to output 1.

.. topic:: Example 2

    Input: g = [1,2], s = [1,2,3]

    Output: 2

    Explanation: You have 2 children and 3 cookies. The greed factors of 2 children are 1, 2. 

    You have 3 cookies and their sizes are big enough to gratify all of the children, You need to output 2.

.. topic:: Constraints

    1 <= g.length <= 3 * 104

    0 <= s.length <= 3 * 104

    1 <= g[i], s[j] <= 231 - 1

**Approach**: First sort both arrays. Then from the largest cookie size, find the largest child for it and increase the count. Continue until there's no child left or no cookie left.

.. code-block:: java

    public int findContentChildren(int[] g, int[] s) {
        Arrays.sort(g);
        Arrays.sort(s);
        
        int i = g.length;
        int count = 0;
        
        for (int j=s.length-1; j>=0; j--) {
            while (i>0) {
                i--;
                //System.out.println("j: "+j + " s[j]: " + s[j] + " g[i]: "+ g[i]);
                if (s[j] >= g[i]) {  
                    count++;
                    break;
                }
            }
            
        }
        
        return count;
    }

-------------------------
376. Wiggle Subsequence
-------------------------

A wiggle sequence is a sequence where the differences between successive numbers strictly alternate between positive and negative. The first difference (if one exists) may be either positive or negative. A sequence with one element and a sequence with two non-equal elements are trivially wiggle sequences.

For example, [1, 7, 4, 9, 2, 5] is a wiggle sequence because the differences (6, -3, 5, -7, 3) alternate between positive and negative.
In contrast, [1, 4, 7, 2, 5] and [1, 7, 4, 5, 5] are not wiggle sequences. The first is not because its first two differences are positive, and the second is not because its last difference is zero.
A subsequence is obtained by deleting some elements (possibly zero) from the original sequence, leaving the remaining elements in their original order.

Given an integer array nums, return the length of the longest wiggle subsequence of nums.

.. topic:: Example 1

    Input: nums = [1,7,4,9,2,5]

    Output: 6

    Explanation: The entire sequence is a wiggle sequence with differences (6, -3, 5, -7, 3).

.. topic:: Example 2

    Input: nums = [1,17,5,10,13,15,10,5,16,8]

    Output: 7

    Explanation: There are several subsequences that achieve this length.

    One is [1, 17, 10, 13, 10, 16, 8] with differences (16, -7, 3, -3, 6, -8).

.. topic:: Example 3:

    Input: nums = [1,2,3,4,5,6,7,8,9]

    Output: 2

.. topic:: Constraints

    1 <= nums.length <= 1000

    0 <= nums[i] <= 1000
 

Follow up: Could you solve this in O(n) time?

**Approach**: We can ignore all the elements inside an increasing sub-sequence or decreasing sub-sequence.

    - For example, for [1,17,5,10,13,15,10,5,16,8], we can ignore 10, 13 in [5, 10, 13, 15] and ignore 10 in [15, 10, 5]. 

To do this, we start from left and flip between signs of the differences. If the sign flipped, we can add one, otherwise keep moving until the sign flipped.

.. code-block:: java

    public int wiggleMaxLength(int[] nums) {
        if (nums.length == 1) {
            return 1;
        }
        
        if (nums.length == 2){
            return (nums[0] != nums[1])?2:1;
        } 
        
        int rst = 1;
        int prediff = 0;
        int curdiff = 0;
        
        for (int i=1; i<nums.length; i++){
            curdiff = nums[i] - nums[i-1];
            if ((curdiff>0 && prediff <= 0) || (curdiff<0 && prediff >=0)){
                rst++;
                prediff = curdiff;
            }
        }
            
        return rst;
    }
