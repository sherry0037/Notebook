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


----------------------
53. Maximum Subarray
----------------------

Given an integer array nums, find the contiguous subarray (containing at least one number) which has the largest sum and return its sum.

A subarray is a contiguous part of an array.

.. topic:: Example 1:

    Input: nums = [-2,1,-3,4,-1,2,1,-5,4]

    Output: 6

    Explanation: [4,-1,2,1] has the largest sum = 6.

.. topic:: Example 2:

    Input: nums = [1]

    Output: 1

.. topic:: Example 3:

    Input: nums = [5,4,-1,7,8]

    Output: 23
 
.. topic:: Constraints:

    1 <= nums.length <= 3 * 104

    -105 <= nums[i] <= 105
 

Follow up: If you have figured out the O(n) solution, try coding another solution using the divide and conquer approach, which is more subtle.

**Approach**: Keep a running sum from left to right. If the running sum becomes less than 0, count again from zero; this is because if the sum is 0, it's impossible to add it to the sum without reducing the sum.

    - For example, for [7, -8, 5], the running sum is 7, -1, 5 (clear to zero after index 1 because the sum is less than 0). If we don't clear to zero, the running sum at index 2 would be 4, which is less than when we clear it to 0.

.. code-block:: java

    public int maxSubArray(int[] nums) {
        int max = Integer.MIN_VALUE;
        int curr = 0;
        for (int i=0; i<nums.length; i++){
            curr += nums[i];
            max = Math.max(max, curr);
            if (curr < 0) {
                curr = 0;
            }
        }
        return max;
    }

--------------------------------------------------------------
1647. Minimum Deletions to Make Character Frequencies Unique
--------------------------------------------------------------

A string s is called good if there are no two different characters in s that have the same frequency.

Given a string s, return the minimum number of characters you need to delete to make s good.

The frequency of a character in a string is the number of times it appears in the string. For example, in the string "aab", the frequency of 'a' is 2, while the frequency of 'b' is 1.

.. topic:: Example 1

    Input: s = "aab"

    Output: 0

    Explanation: s is already good.

.. topic:: Example 2

    Input: s = "aaabbbcc"

    Output: 2

    Explanation: You can delete two 'b's resulting in the good string "aaabcc".

    Another way it to delete one 'b' and one 'c' resulting in the good string "aaabbc".

.. topic:: Example 3

    Input: s = "ceabaacb"

    Output: 2

    Explanation: You can delete both 'c's resulting in the good string "eabaab".

    Note that we only care about characters that are still in the string at the end (i.e. frequency of 0 is ignored).

.. topic:: Constraints

    1 <= s.length <= 105

    s contains only lowercase English letters.

.. code-block:: java
    
    public int minDeletions(String s) {
        HashMap<Character, Integer> count = new HashMap<>();
        for (int i = 0; i < s.length(); i++) {
            Character c = s.charAt(i);
            if (count.containsKey(c)) {
                count.put(c, count.get(c) + 1);
            } else {
                count.put(c, 1);
            }
        }
        
        List<Integer> freq = new ArrayList<Integer>(count.values());
        Collections.sort(freq);
        HashSet<Integer> set = new HashSet<>(); 
        int rst = 0;
        for (int f:freq) {
            if (!set.contains(f)) {
                set.add(f);
                continue;
            }
            while (set.contains(f)) {
                //System.out.println(f);
                f--;
                rst++;
            }
            if (f!=0) {
                set.add(f);
            }
        
        }
        
        return rst;
    }