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



--------------------------------
99. Recover Binary Search Tree
--------------------------------

You are given the root of a binary search tree (BST), where exactly two nodes of the tree were swapped by mistake. Recover the tree without changing its structure.

Follow up: A solution using O(n) space is pretty straight forward. Could you devise a constant space solution?

First Approach - In Order Traversal
------------------------------------

1. Do an in order traversal(iot) of the tree.
2. Find the two elements in the iot that are not in the right order.
3. Search the tree again and swap the values.

.. topic:: InOrderTraversal

    After traversing, the sequence is in ascending order.

    .. code-block:: java

        private ArrayList<Integer> inOrderTraversal(TreeNode root) {
            if (root == null) {
                return new ArrayList<Integer>();
            }
            
            ArrayList<Integer> rst = (inOrderTraversal(root.left));
            rst.add(root.val);
            rst.addAll(inOrderTraversal(root.right));
            
            return rst;
        }

    .. code-block:: java

        private void inOrderTraversal(TreeNode root, List<Integer> rst) {
            if (root == null) {
                return;
            }
            
            inOrderTraversal(root.left, rst);
            rst.add(root.val);
            inOrderTraversal(root.right);
        }


.. code-block:: java

    class Solution {
        public void recoverTree(TreeNode root) {
            ArrayList<Integer> iot = inOrderTraversal(root);
            
            Integer first = null;
            Integer second = null;
            
            Integer prev = iot.get(0);
            
            for (Integer current : iot) {
                if (prev > current) {
                    if (first == null) {
                        first = prev;
                    }
                    if (first != null) {
                        second = current;
                    }
                }
                
                prev = current;
            }   
            
            //System.out.println("first: "+first + " second: "+second);
            
            swap(root, first, second);
        }
        
        
        
        private void swap(TreeNode root, int first, int second) {
            // first > second
            if (root == null) {
                return;
            }
            
            swap(root.left, first, second);
            
            if (root.val == first) {
                //System.out.println("Swap to " + second + ": " + root.val);
                root.val = second;
            } else if (root.val == second) {
                //System.out.println("Swap to " + first + ": " + root.val);
                root.val = first;
            }
            
            swap(root.right, first, second);
        }
    }

Second Approach - In Order Traversal In Place Swap
---------------------------------------------------

Same idea as the first approach, just do the swap while doing in order traversal.

.. code-block:: java

    class Solution {
        TreeNode first = null;
        TreeNode second = null;
        TreeNode prev = null;
        
        public void recoverTree(TreeNode root) {
            
            inOrderTraversal(root);
            
            //System.out.println("fisrt: "+first.val + " second: "+second.val);
            
            if (first != null && second != null) {
               
                int temp = second.val;
                second.val = first.val;
                first.val = temp;
            }
        }
        
        private void inOrderTraversal(TreeNode root) {
            if (root == null) {
                return;
            }
            
            inOrderTraversal(root.left);
            
            if (prev != null && prev.val > root.val) {
                if (first == null) {
                    first = prev;
                }
                
                if (first != null) {
                    second = root;
                }
            }
            
            prev = root;
            
            inOrderTraversal(root.right);
        }

    }

----------------------------------
199. Binary Tree Right Side View
----------------------------------

Given the root of a binary tree, imagine yourself standing on the right side of it, return the values of the nodes you can see ordered from top to bottom.

(This question is quite easy)

Approach: Keep a depth when traverse the tree. Keep an array list rst to store the final result. The index of the array list corresponds to the depth. For example, rst.get(5) is the right most TreeNode at depth 5. We do a right first traverse. Each time we reach a depth k for the first time (determined by rst.size()<k), we know that it is the right most TreeNode.

.. code-block:: java

    class Solution {
        List<Integer> rst = new ArrayList<Integer>();
        public List<Integer> rightSideView(TreeNode root) {
            traverseTree(root, rst, 1);
            return rst;
        }
        
        private void traverseTree(TreeNode root, List<Integer> rst, int d) {
            if (root == null) {
                return;
            }
            
            if (rst.size() < d) {
                rst.add(root.val);
            }
            
            traverseTree(root.right, rst, d+1);
            traverseTree(root.left, rst, d+1);
        }
    }


-----------------------
337. House Robber III
-----------------------

The thief has found himself a new place for his thievery again. There is only one entrance to this area, called root.

Besides the root, each house has one and only one parent house. After a tour, the smart thief realized that all houses in this place form a binary tree. It will automatically contact the police if two directly-linked houses were broken into on the same night.

Given the root of the binary tree, return the maximum amount of money the thief can rob without alerting the police.

Approach: For each node, we either choose it or not choose it. If we choose it, we cannot rob the left nor the right. If we don't choose it, we can rob or not rub the left, or rob or not rub the right (4 cases). So the helper function returns a pair of values for each node, one is the gain by choosing it, one is gain by not choosing it. Then at the end we compare the gain for the root.

Tip: using array instead of ArrayList will be much faster (54.95% -> 100%) and saves space.

.. code-block:: java

    class Solution {
        
        public int rob(TreeNode root) {
            List<Integer> rst = _rob(root);
            return Math.max(rst.get(0), rst.get(1));
        }
        
        private List<Integer> _rob(TreeNode root) {
            // 0 = choose, 1 = not choose
            List<Integer> rst = new ArrayList<Integer>();
            rst.add(0);
            rst.add(0);
            if (root == null) {
                return rst;
            }
            
            List<Integer> rstLeft = _rob(root.left);
            List<Integer> rstRight = _rob(root.right);
            int robLeft = rstLeft.get(0);
            int robRight = rstRight.get(0);
            int robLeftNo = rstLeft.get(1);
            int robRightNo = rstRight.get(1);
            
            // If choose root, do not choose left or right
            rst.set(0, root.val + robLeftNo + robRightNo);
            
            // If don't choose root, either choose/not left or choose/not right
            rst.set(1, Math.max(Math.max(Math.max(robLeft + robRight, robLeft + robRightNo), robLeftNo + robRight), robLeftNo + robRightNo));
            
            //System.out.println("root: " + root.val + " choose: "+ rst.get(0) + " not choose: " + rst.get(1));
            return rst;
            
        }

----------------------------------------
102. Binary Tree Level Order Traversal
----------------------------------------

Given the root of a binary tree, return the level order traversal of its nodes' values. (i.e., from left to right, level by level).

.. topic::  Example 1

    Input: root = [3,9,20,null,null,15,7]

    Output: [[3],[9,20],[15,7]]

.. topic::  Example 2

    Input: root = [1]

    Output: [[1]]

.. topic::  Example 3

    Input: root = []

    Output: []
 
.. topic::  Constraints

    The number of nodes in the tree is in the range [0, 2000].

    -1000 <= Node.val <= 1000

**Note:** This is BFS. See 107. Binary Tree Level Order Traversal II for DFS solution which will be faster than BFS.

.. code-block:: java

    public List<List<Integer>> levelOrder(TreeNode root) {
        List<List<Integer>> rst = new ArrayList<>();
        traverse(root, 0, rst);
        return rst;
    }
    
    private void traverse(TreeNode root, int level, List<List<Integer>> rst) {
        if (root == null) {
            return;
        }
        
        if (rst.size() <= level) {
            rst.add(new ArrayList<>());
        }
        rst.get(level).add(root.val);
        
        traverse(root.left, level+1, rst);
        traverse(root.right, level+1, rst);
    }

-------------------------------------------
107. Binary Tree Level Order Traversal II
-------------------------------------------

Given the root of a binary tree, return the bottom-up level order traversal of its nodes' values. (i.e., from left to right, level by level from leaf to root).

.. topic::  Example 1

    Input: root = [3,9,20,null,null,15,7]

    Output: [[15,7],[9,20],[3]]

.. topic::  Example 2

    Input: root = [1]

    Output: [[1]]

.. topic::  Example 3

    Input: root = []

    Output: []
 
.. topic::  Constraints

    The number of nodes in the tree is in the range [0, 2000].

    -1000 <= Node.val <= 1000

.. code-block:: java

    public List<List<Integer>> levelOrderBottom(TreeNode root) {
        List<List<Integer>> rst = new ArrayList<>();
        
        traverse(root, rst, 0);
        
        Collections.reverse(rst);
        
        return rst;
    }
    
    private void traverse(TreeNode root, List<List<Integer>> rst, int depth) {
        if (root == null) {
            return;
        }
        
        traverse(root.left, rst, depth+1);
        
        while (rst.size() <= depth) {
            rst.add(new ArrayList<Integer>());
        }
        
        rst.get(depth).add(root.val);
       
        traverse(root.right, rst, depth+1);
    }

---------------------------------------
637. Average of Levels in Binary Tree
---------------------------------------

Given the root of a binary tree, return the average value of the nodes on each level in the form of an array. Answers within 10-5 of the actual answer will be accepted.

.. topic::  Example 1

    Input: root = [3,9,20,null,15,7]

    Output: [3.00000,14.50000,11.00000]

    Explanation: The average value of nodes on level 0 is 3, on level 1 is 14.5, and on level 2 is 11.

    Hence return [3, 14.5, 11].

.. topic::  Example 2

    Input: root = [3,9,20,15,7]

    Output: [3.00000,14.50000,11.00000]
     
.. topic::  Constraints

    The number of nodes in the tree is in the range [1, 104].

    -231 <= Node.val <= 231 - 1

**Note**: use double to avoid overflow.

.. code-block:: java

    public List<Double> averageOfLevels(TreeNode root) {
        List<Double> avgs = new ArrayList<>();
        List<Integer> nNodes = new ArrayList<>();
        traverse(root, 0, avgs, nNodes);
        
        return avgs;
    }
    
    private void traverse(TreeNode root, int depth, List<Double> avgs, List<Integer> nNodes) {
        if (root == null) {
            return;
        }
        
        if (avgs.size() <= depth) {
            avgs.add((double)root.val); // we get to a new level
            nNodes.add(1);
        } else {
            int n = nNodes.get(depth);
            double a = avgs.get(depth);
            avgs.set(depth, (a*n + root.val) / (n+1));
            nNodes.set(depth, n+1);
        }
        
        traverse(root.left, depth + 1, avgs, nNodes);
        traverse(root.right, depth + 1, avgs, nNodes);
    }
