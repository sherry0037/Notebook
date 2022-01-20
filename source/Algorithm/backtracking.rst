==================================
Backtracking
==================================

-----------------------------------------
784. Letter Case Permutation (09/03/19)
-----------------------------------------
Given a string S, we can transform every letter individually to be lowercase or uppercase to create another string.  Return a list of all possible strings we could create.

.. topic::  Examples

    Input: S = "a1b2"
    Output: ["a1b2", "a1B2", "A1b2", "A1B2"]

    Input: S = "3z4"
    Output: ["3z4", "3Z4"]

    Input: S = "12345"
    Output: ["12345"]

.. topic:: Notes

    - S will be a string with length between 1 and 12.
    - S will consist only of letters or digits.

**1st Attempt:**

Assuming that we've already dealt with all the chars except for the last character. Say, for ``S = "a1b2c"``, we already have ``["a1b2", "a1B2", "A1b2", "A1B2"]``, and we want to append ``c`` to the Strings. If the character is a letter, we need to add both upper case and lower case; if it's a digit, we just need to add a letter. In this case, we will obtain ``["a1b2c", "a1B2c", "A1b2c", "A1B2c", "a1b2C", "a1B2C", "A1b2C", "A1B2C"]``. A simple helper function will do this. Start with a List of an empty String, and traverse through the input String to add each character one by one.

See *Approach #1: Recursion* in Solution for different implementation using StringBuilder.

.. code-block:: java

    class Solution {
        public List<String> letterCasePermutation(String S) {
            List<String> inputList = new ArrayList<>();
            inputList.add("");
            char[] inputArray = S.toCharArray();
            for (char ch : inputArray){
                inputList = helper(ch, inputList);
            }
            return inputList;
        }
        
        private List<String> helper(char c, List<String> inputList){
            List<Character> toAdd = new ArrayList<>();
            if (Character.isLetter(c)){
                toAdd.add(Character.toLowerCase(c));
                toAdd.add(Character.toUpperCase(c));
            }else{
                toAdd.add(c);
            }
            List<String> rst = new ArrayList<>();

            for (String s : inputList){
                for (char newChar : toAdd){
                    rst.add(s+Character.toString(newChar));
                }
            }
            return rst;
        }
    }

-----------------------------------------
401. Binary Watch (09/03/19)
-----------------------------------------
A binary watch has 4 LEDs on the top which represent the hours (0-11), and the 6 LEDs on the bottom represent the minutes (0-59).

Each LED represents a zero or one, with the least significant bit on the right.

Given a non-negative integer n which represents the number of LEDs that are currently on, return all possible times the watch could represent.

.. topic::  Examples

    Input: n = 1
    Return: ["1:00", "2:00", "4:00", "8:00", "0:01", "0:02", "0:04", "0:08", "0:16", "0:32"]

.. topic:: Notes

    - The order of output does not matter.
    - The hour must not contain a leading zero, for example "01:00" is not valid, it should be "1:00".
    - The minute must be consist of two digits and may contain a leading zero, for example "10:2" is not valid, it should be "10:02".

**1st Attempt:**

The core of this problem is a combination problem. Suppose we know how many lights are on for hours/minutes, we need to find all possible combinations for lights that are on. For example, if we use [8, 4, 2, 1] to represent the hours, and 2 lights are on, then the combinations are [[8, 4], [8, 2], [8, 1], [4, 2], [4, 1], [2, 1]]. The corresponding time is [12, 10, 9, 6, 5, 3]. We can do the same thing for minutes. We can write a ``choose()`` method to compute this. Then remove any number that is not in the range, and convert them to proper Strings to get the final anser.

The ``choose()`` method takes two Lists of Integers, representing the lights, and the number of lights ``K`` to choose. For more ways to generate combination, read this `article <https://www.baeldung.com/java-combinations-algorithm>`_. The high level idea of the method I used is: suppose we are choosing ``K`` elements from a List of size ``N``, for the first element, we can decide if we want to keep it or not. If keep it, the question reduces to choosing ``K-1`` elements from the List without the first element (size ``N-1``); otherwise, the question reduces to choosing ``K`` elements from List without the first element (size ``N-1``). Then we can use any recursive method to implement this.

Things to do next time:
    - change a way to write the recursive method
    - extract the String generation out of the loop
    - use array instead of List?

.. code-block:: java

    class Solution {
        public List<String> readBinaryWatch(int num) {
            List<String> rst = new ArrayList<>();
            if (num == 0){
                rst.add("0:00");
                return rst;
            }
            List<Integer> HOURS = new ArrayList<>();
            HOURS = Arrays.asList(1,2,4,8);
            List<Integer> MINUTES = new ArrayList<>();
            MINUTES = Arrays.asList(1, 2, 4, 8, 16, 32);
            for (int h = 0; h < num+1; h++) {
                int m = num-h;
                List<Integer> hours_time = getSums(choose(HOURS, h), 11);
                List<Integer> minutes_time = getSums(choose(MINUTES, m), 59);
                if (h == 0){
                    hours_time.add(0);
                }
                if (m == 0){
                    minutes_time.add(0);
                }
                for (int hour : hours_time){
                    String h_str = String.valueOf(hour);
                    for (int minute : minutes_time){
                        String m_str = String.valueOf(minute);
                        if (minute < 10){
                            m_str = "0" + m_str;
                        }
                        rst.add(h_str + ":" + m_str);
                    }
                }

            }
            return rst;
        }
        
        private List<List<Integer>> choose(List<Integer> inputList, int K){
            List<List<Integer>> rst = new ArrayList<>();
            if (K == 0 || inputList.size() == 0){
                return rst;
            }
            if (K == 1){
                for (Integer i : inputList){
                    List<Integer> newList = new ArrayList<>();
                    newList.add(i);
                    rst.add(newList);
                }
                return rst;
            }
            Integer first = inputList.get(0);
            List<List<Integer>> included = choose(inputList.subList(1, inputList.size()), K-1);
            List<List<Integer>> notIncluded = choose(inputList.subList(1, inputList.size()), K);
            for (List<Integer> previousList : included){
                previousList.add(first);
                rst.add(previousList);
            }
            for (List<Integer> previousList : notIncluded){
                rst.add(previousList);
            }
            return rst;
        }
        
        private List<Integer> getSums(List<List<Integer>> inputList, int maxSum){
            List<Integer> rst = new ArrayList<>();
            for (List<Integer> intList : inputList){
                int sum = 0;
                for (int i : intList){
                    sum += i;
                }
                if (sum <= maxSum){
                    rst.add(sum);
                }
            }
            Collections.sort(rst);
            return rst;
        }
    }


-----------------------------------------
46. Permutations (09/04/19)
-----------------------------------------
Given a collection of **distinct** integers, return all possible permutations.

.. topic:: Example

    Input: [1,2,3]
    Output: [[1,2,3], [1,3,2], [2,1,3], [2,3,1], [3,1,2], [3,2,1]]


**1st Attempt:**

We can use a stack to keep adding elements to Lists. At the first step, the stack is ``[[1], [2], [3]]``. We pop out the head of the stack, in this case ``[1]``, iterate through the input nums. For each element, check wether it is contained in the head. If not, add this element to a copy of the head, and push it back to the stack. So the stack changes as follows:

======  ==================================================================
head    stack
======  ==================================================================
null    [[1], [2], [3]]
[1]     [[2], [3], [1, 2], [1, 3]]
[2]     [[3], [1, 2], [1, 3], [2, 1], [2, 3]]   
[3]     [[1, 2], [1, 3], [2, 1], [2, 3], [3, 1], [3, 2]]
[1, 2]  [[1, 3], [2, 1], [2, 3], [3, 1], [3, 2], [1, 2, 3]] 
[1, 3]  [[2, 1], [2, 3], [3, 1], [3, 2], [1, 2, 3], [1, 3, 2]] 
[2, 1]  [[2, 3], [3, 1], [3, 2], [1, 2, 3], [1, 3, 2], [2, 1, 3]] 
[2, 3]  [[3, 1], [3, 2], [1, 2, 3], [1, 3, 2], [2, 1, 3], [2, 3, 1]] 
[3, 1]  [[3, 2], [1, 2, 3], [1, 3, 2], [2, 1, 3], [2, 3, 1], [3, 1, 2]] 
[3, 2]  [[1, 2, 3], [1, 3, 2], [2, 1, 3], [2, 3, 1], [3, 1, 2], [3, 2, 1]] 
======  ==================================================================

When the size of the head is same as the input length, this List is finished and can be added to the final result.


.. code-block:: java

    class Solution {
        public List<List<Integer>> permute(int[] nums) {
            List<List<Integer>> rst = new ArrayList<>();
            LinkedList<List<Integer>> stack  = new LinkedList<>();
            for (int i:nums){
                List<Integer> l = new ArrayList<>();
                l.add(i);
                stack.add(l);
            }
            
            while (stack.size()>0){
                List<Integer> current = stack.pop();
                if (current.size() == nums.length){
                    rst.add(current);
                }else{
                    for (int i:nums){
                        if (!current.contains(i)){
                            List<Integer> newList = new ArrayList<>();
                            for (int j:current){
                                newList.add(j);
                            }
                            newList.add(i);
                            stack.push(newList);
                        }
                    }
                }
            }
            return rst;
                
        }
    }

Things to do:
    - check how to use stream: ``stack = Arrays.stream( nums ).boxed().collect( Collectors.toCollection(LinkedList::new) );``

========================
257. Binary Tree Paths
========================

Given the root of a binary tree, return all root-to-leaf paths in any order.

A leaf is a node with no children.

.. topic:: Example 1

    Input: root = [1,2,3,null,5]

    Output: ["1->2->5","1->3"]

.. topic:: Example 2

    Input: root = [1]

    Output: ["1"]

.. topic:: Constraints

    The number of nodes in the tree is in the range [1, 100].

    -100 <= Node.val <= 100

**Approach** use backtracking

.. code-block:: java

    public List<String> binaryTreePaths(TreeNode root) {
        List<String> rst = new ArrayList<>();
        traverse(root, new ArrayList<>(), rst);
        return rst;
    }
    
    private void traverse(TreeNode node, List<String> temp, List<String> rst) { 
        temp.add("" + node.val);
        if (node.left == null && node.right == null) {
            String s = String.join("->", temp);
            rst.add(s);
            return;
        }
        
        if (node.left != null) {
            traverse(node.left, temp, rst);
            temp.remove(temp.size()-1);
        }
        
        if (node.right != null) {
            traverse(node.right, temp, rst);
            temp.remove(temp.size()-1);
        }
            
    }