==================================
Tree
==================================

.. contents::
    :depth: 2

---------------------------------------
Overview
---------------------------------------

.. topic:: Template

    .. code-block:: java

        private int getResult(TreeNode root, xx, int rst) {
            if (root == null) {
                return rst;
            }
            
            // Set nextXX for the left iteration and right iteration according to the question
            
            // Change rst when constraints are met
            if (constraints_are_met) {
                rst += root.val;
            }
            
            rst = getResult(root.left, nextXX, rst);
            rst = getResult(root.right, nextXX, rst);
        
            return rst;
            
            
        }

---------------------------------------
784. Letter Case Permutation
---------------------------------------

Given the root of a binary tree, return the sum of values of its deepest leaves.

.. topic:: Examples

    Input:

    1

    2, 3

    4, 5, null, 6

    7, null, null, null, null, null, null, 8

    Output: 7 + 8 = 15


Approach: first traverse the tree to get the maximum depth. Then traverse again, if the depth is equal to the 
maximum depth, return that value.

Note:

- Depth increment should be done after the ``root == null`` check. Otherwise depth will 1 more.
- When ``currentDepth == maxDepth``, just return the current root value.


.. code-block:: java

    /**
     * Definition for a binary tree node.
     * public class TreeNode {
     *     int val;
     *     TreeNode left;
     *     TreeNode right;
     *     TreeNode() {}
     *     TreeNode(int val) { this.val = val; }
     *     TreeNode(int val, TreeNode left, TreeNode right) {
     *         this.val = val;
     *         this.left = left;
     *         this.right = right;
     *     }
     * }
     */
    class Solution {
        public int deepestLeavesSum(TreeNode root) {
            int maxDepth = getMaxDepth(root, 0);
            System.out.println("maxD: "+maxDepth);
            return getLeavesSum(root, 0, maxDepth);
            
        }
        
        private int getMaxDepth(TreeNode root, int depth) {
            if (root == null) {
                return depth;
            }
            
            depth = depth + 1;
            
            System.out.println("root: "+root.val+ " currentD: "+ depth);
            
            return Math.max(getMaxDepth(root.left, depth), getMaxDepth(root.right, depth));
        }
        
        private int getLeavesSum(TreeNode root, int currentDepth, int maxDepth) {
            if (root == null) {
                return 0;
            }
            
            currentDepth += 1;
            
            System.out.println("root: "+root.val+ " currentD: "+currentDepth);

            
            if (currentDepth == maxDepth) {
                return root.val;
            }
            
            return getLeavesSum(root.left, currentDepth, maxDepth) + 
                getLeavesSum(root.right, currentDepth, maxDepth);
        }
            
    }

.. topic::  Running result

    root: 1 currentD: 1
    root: 2 currentD: 2
    root: 4 currentD: 3
    root: 7 currentD: 4
    root: 5 currentD: 3
    root: 3 currentD: 2
    root: 6 currentD: 3
    root: 8 currentD: 4
    maxD: 4
    root: 1 currentD: 1
    root: 2 currentD: 2
    root: 4 currentD: 3
    root: 7 currentD: 4
    root: 5 currentD: 3
    root: 3 currentD: 2
    root: 6 currentD: 3
    root: 8 currentD: 4


------------------------------------------------------------------------------
1379. Find a Corresponding Node of a Binary Tree in a Clone of That Tree
------------------------------------------------------------------------------

Given two binary trees original and cloned and given a reference to a node target in the original tree.

The cloned tree is a copy of the original tree.

Return a reference to the same node in the cloned tree.

Note that you are not allowed to change any of the two trees or the target node and the answer must be a reference to a node in the cloned tree.

Follow up: Solve the problem if repeated values on the tree are allowed.

Constraints:

The number of nodes in the tree is in the range [1, 10^4].

The values of the nodes of the tree are unique.

target node is a node from the original tree and is not null.

.. code-block:: java

    /**
     * Definition for a binary tree node.
     * public class TreeNode {
     *     int val;
     *     TreeNode left;
     *     TreeNode right;
     *     TreeNode(int x) { val = x; }
     * }
     */

    class Solution {
        public final TreeNode getTargetCopy(final TreeNode original, final TreeNode cloned, final TreeNode target) {
            if (cloned == null) {
                return null;
            }
            
            if (cloned.val == target.val) {
                return cloned;
            }
            
            TreeNode left = getTargetCopy(original.left, cloned.left, target);
            if (left != null) {
                return left;
            }
            TreeNode right = getTargetCopy(original.right, cloned.right, target);
            if (right != null) {
                return right;
            }
            
            return null;
        }
    }


------------------------------------------------
1315. Sum of Nodes with Even-Valued Grandparent
------------------------------------------------

https://leetcode.com/problems/sum-of-nodes-with-even-valued-grandparent/

Given a binary tree, return the sum of values of nodes with even-valued grandparent.  (A grandparent of a node is the parent of its parent, if it exists.)

If there are no nodes with an even-valued grandparent, return 0.


.. code-block:: java

    /**
     * Definition for a binary tree node.
     * public class TreeNode {
     *     int val;
     *     TreeNode left;
     *     TreeNode right;
     *     TreeNode() {}
     *     TreeNode(int val) { this.val = val; }
     *     TreeNode(int val, TreeNode left, TreeNode right) {
     *         this.val = val;
     *         this.left = left;
     *         this.right = right;
     *     }
     * }
     */
    class Solution {
        public int sumEvenGrandparent(TreeNode root) {
            return getResult(root, false, false, 0);
        }
        
        private int getResult(TreeNode root, boolean parent, boolean grandparent, int rst) {
            if (root == null) {
                return rst;
            }
            
            // System.out.println("root: " + root.val + ", parent: " + parent + ", grandparent: " + grandparent + ", rst: "+ rst);
            
            boolean nextParent = false;
            if (root.val%2 == 0) {
                nextParent = true;
            }
            
            boolean nextGrandParent = false;
            if (parent) {
                nextGrandParent = true;
            }
            
            // Grandparent is even
            if (grandparent) {
                rst += root.val;
            }
            
            rst = getResult(root.left, nextParent, nextGrandParent, rst);
            rst = getResult(root.right, nextParent, nextGrandParent, rst);
        
            return rst;
            
            
        }
    }


.. topic::  Running result

    root: 6, parent: false, grandparent: false, rst: 0

    root: 7, parent: true, grandparent: false, rst: 0

    root: 2, parent: false, grandparent: true, rst: 0

    root: 9, parent: true, grandparent: false, rst: 2

    root: 7, parent: false, grandparent: true, rst: 2

    root: 1, parent: false, grandparent: false, rst: 9

    root: 4, parent: false, grandparent: false, rst: 9

    root: 8, parent: true, grandparent: false, rst: 9

    root: 1, parent: true, grandparent: true, rst: 9

    root: 3, parent: true, grandparent: true, rst: 10

    root: 5, parent: false, grandparent: true, rst: 13




