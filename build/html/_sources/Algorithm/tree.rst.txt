==================================
Tree
==================================

.. contents::
    :depth: 2

---------------------------------------
Overview
---------------------------------------

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

===================================================
Tree Traversals (Inorder, Preorder and Postorder)
===================================================

.. figure:: tree_traversal.png
    :scale: 100 %

Depth First Traversals: 

(a) Inorder (Left, Root, Right) : 4 2 5 1 3 

(b) Preorder (Root, Left, Right) : 1 2 4 5 3 

(c) Postorder (Left, Right, Root) : 4 5 2 3 1

Breadth-First or Level Order Traversal: 1 2 3 4 5 

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

---------------------------------------
429. N-ary Tree Level Order Traversal
---------------------------------------

Given an n-ary tree, return the level order traversal of its nodes' values.

Nary-Tree input serialization is represented in their level order traversal, each group of children is separated by the null value (See examples).

.. topic::  Example 1

    Input: root = [1,null,3,2,4,null,5,6]

    Output: [[1],[3,2,4],[5,6]]

.. topic::  Example 2

    Input: root = [1,null,2,3,4,5,null,null,6,7,null,8,null,9,10,null,null,11,null,12,null,13,null,null,14]

    Output: [[1],[2,3,4,5],[6,7,8,9,10],[11,12,13],[14]]
 
.. topic::  Constraints

    The height of the n-ary tree is less than or equal to 1000

    The total number of nodes is between [0, 104]

.. code-block:: java

    /*
    // Definition for a Node.
    class Node {
        public int val;
        public List<Node> children;

        public Node() {}

        public Node(int _val) {
            val = _val;
        }

        public Node(int _val, List<Node> _children) {
            val = _val;
            children = _children;
        }
    };
    */

    class Solution {
        public List<List<Integer>> levelOrder(Node root) {
            List<List<Integer>> rst = new ArrayList<>();
            traverse(root, rst, 0);
            
            return rst;
        }
        
        private void traverse(Node root, List<List<Integer>> rst, int depth) {
            if (root == null) {
                return;
            }
            
            for (Node child : root.children) {
                traverse(child, rst, depth+1);
            }
            
            while (rst.size() <= depth) {
                rst.add(new ArrayList<Integer>());
            }
            
            rst.get(depth).add(root.val);
                
        }
    }

------------------------------------------
515. Find Largest Value in Each Tree Row
------------------------------------------

Given the root of a binary tree, return an array of the largest value in each row of the tree (0-indexed).

.. topic::  Example 1

    Input: root = [1,3,2,5,3,null,9]

    Output: [1,3,9]

.. topic::  Example 2

    Input: root = [1,2,3]

    Output: [1,3]

.. topic::  Example 3

    Input: root = [1]

    Output: [1]

.. topic::  Example 4

    Input: root = [1,null,2]

    Output: [1,2]

.. topic::  Example 5

    Input: root = []

    Output: []
 
.. topic::  Constraints

    The number of nodes in the tree will be in the range [0, 104].

    -231 <= Node.val <= 231 - 1

.. code-block:: java

    public List<Integer> largestValues(TreeNode root) {
        List<Integer> maxes = new ArrayList<Integer>();
        
        traverse(root, maxes, 0);
        return maxes;
    }
    
    private void traverse(TreeNode root, List<Integer> maxes, int depth) {
        if (root == null) {
            return;
        }
        
        traverse(root.left, maxes, depth+1);
        
        while (maxes.size() <= depth) {
            maxes.add(Integer.MIN_VALUE);
        }
        
        maxes.set(depth, Math.max(maxes.get(depth), root.val));        
        
        traverse(root.right, maxes, depth+1);
        
    }

--------------------------------------------------
116. Populating Next Right Pointers in Each Node
--------------------------------------------------

You are given a perfect binary tree where all leaves are on the same level, and every parent has two children. The binary tree has the following definition:

.. code-block:: java

    struct Node {
      int val;
      Node *left;
      Node *right;
      Node *next;
    }

Populate each next pointer to point to its next right node. If there is no next right node, the next pointer should be set to NULL.

Initially, all next pointers are set to NULL.

.. topic::  Example 1

    Input: root = [1,2,3,4,5,6,7]

    Output: [1,#,2,3,#,4,5,6,7,#]

    Explanation: Given the above perfect binary tree (Figure A), your function should populate each next pointer to point to its next right node, just like in Figure B. The serialized output is in level order as connected by the next pointers, with '#' signifying the end of each level.

.. topic::  Example 2

    Input: root = []

    Output: []

.. topic::  Constraints

    The number of nodes in the tree is in the range [0, 212 - 1].

    -1000 <= Node.val <= 1000
 
.. topic::  Follow-up

    You may only use constant extra space.

    The recursive approach is fine. You may assume implicit stack space does not count as extra space for this problem.

**Approach**: Given a current root node:

- For its left child, the pointer should points to the right child of the root node.

- For its right child, the pointer should points to either null or the left child of root's next node. 

- For example, given 

1

2 3

4 5 6 7

Suppose current node is 2, its left child is 4, which should point to its right child (5). Its right child is 5, which should point to the left child of 5's next node(3)'s child (6). 

.. code-block:: java

    public Node connect(Node root) {
        traverse(root, null);
        
        return root;
    }
    
    private void traverse(Node root, Node right) {
        if (root == null) {
            return;
        }
        
        root.next = right;
        
        if (right != null) {
            right = right.left;
        }
        
        traverse(root.right, right);
        traverse(root.left, root.right);
    }

-----------------------------------------------------
117. Populating Next Right Pointers in Each Node II
-----------------------------------------------------

Given a binary tree

.. code-block:: java

    struct Node {
      int val;
      Node *left;
      Node *right;
      Node *next;
    }

Populate each next pointer to point to its next right node. If there is no next right node, the next pointer should be set to NULL.

Initially, all next pointers are set to NULL.

.. topic::  Example 1

    Input: root = [1,2,3,4,5,null,7]

    Output: [1,#,2,3,#,4,5,7,#]

    Explanation: Given the above binary tree (Figure A), your function should populate each next pointer to point to its next right node, just like in Figure B. The serialized output is in level order as connected by the next pointers, with '#' signifying the end of each level.

.. topic::  Example 2

    Input: root = []

    Output: []
     
.. topic::  Constraints

    The number of nodes in the tree is in the range [0, 6000].

    -100 <= Node.val <= 100

.. topic::  Follow-up

    You may only use constant extra space.

    The recursive approach is fine. You may assume implicit stack space does not count as extra space for this problem.

.. code-block:: java

    public Node connect(Node root) {
        List<Node> pointers = new ArrayList<>();
        traverse(root, pointers, 0);
        
        return root;
    }
    
    private void traverse(Node root, List<Node> pointers, int depth) {
        if (root == null) {
            return;
        }
        
        if (pointers.size() <= depth) {
            pointers.add(root);
            root.next = null;
        } else {
            root.next = pointers.get(depth);
            pointers.set(depth, root);
        }
        
        traverse(root.right, pointers, depth+1);
        traverse(root.left, pointers, depth+1);
    }

-------------------------
226. Invert Binary Tree
-------------------------

Given the root of a binary tree, invert the tree, and return its root.

.. topic::  Example 1

    Input: root = [4,2,7,1,3,6,9]

    Output: [4,7,2,9,6,3,1]

.. topic::  Example 2

    Input: root = [2,1,3]

    Output: [2,3,1]

.. topic::  Example 3

    Input: root = []

    Output: []
 
.. topic::  Constraints

    The number of nodes in the tree is in the range [0, 100].

    -100 <= Node.val <= 100

.. code-block:: java

    public TreeNode invertTree(TreeNode root) {
            traverse(root);
            return root;
        }
        
    private void traverse(TreeNode root) {
        if (root == null) {
            return;
        }
        
        traverse(root.left);
        traverse(root.right);
        
        TreeNode temp = root.left;
        root.left = root.right;
        root.right = temp;
    }

---------------------
101. Symmetric Tree
---------------------

Given the root of a binary tree, check whether it is a mirror of itself (i.e., symmetric around its center).

.. topic::  Example 1

    Input: root = [1,2,2,3,4,4,3]

    Output: true

.. topic::  Example 2

    Input: root = [1,2,2,null,3,null,3]

    Output: false

.. topic::  Constraints

    The number of nodes in the tree is in the range [1, 1000].

    -100 <= Node.val <= 100

.. code-block:: java

    public boolean isSymmetric(TreeNode root) {
        return compare(root.left, root.right);
    }
    
    private boolean compare(TreeNode left, TreeNode right) {
        if (left == null && right != null ||
           right == null && left != null ) {
            return false;
        } else if (left == null && right == null) {
            return true;
        }
        
        if (left.val != right.val) {
            return false;
        }
        
        return compare(left.left, right.right) && compare(left.right, right.left);
    }
 
------------------------------------
 111. Minimum Depth of Binary Tree
------------------------------------

Given a binary tree, find its minimum depth.

The minimum depth is the number of nodes along the shortest path from the root node down to the nearest leaf node.

Note: A leaf is a node with no children.

.. topic::  Example 1:

    Input: root = [3,9,20,null,null,15,7]

    Output: 2

.. topic::  Example 2:

    Input: root = [2,null,3,null,4,null,5,null,6]

    Output: 5
 
.. topic::  Constraints:

    The number of nodes in the tree is in the range [0, 105].

    -1000 <= Node.val <= 1000

.. code-block:: java

    class Solution {
        int minDepth = Integer.MAX_VALUE;
        
        public int minDepth(TreeNode root) {     
            if (root == null) {
                return 0;
            }
            traverse(root, 1);
            return this.minDepth;
        }
        
        private void traverse(TreeNode root, int depth) {
            if (root == null) {
                return;
            }
            
            if (depth == this.minDepth) {
                return;
            }
            
            traverse(root.left, depth+1);
            
            if (root.left == null && root.right == null) {
                if (depth < this.minDepth) {
                    this.minDepth = depth;
                }
            }
            
            traverse(root.right, depth+1);
            
        }
    }

---------------------------
110. Balanced Binary Tree
---------------------------

Given a binary tree, determine if it is height-balanced.

For this problem, a height-balanced binary tree is defined as:

a binary tree in which the left and right subtrees of every node differ in height by no more than 1.

.. topic:: Example 1

    Input: root = [3,9,20,null,null,15,7]

    Output: true

.. topic:: Example 2

    Input: root = [1,2,2,3,3,null,null,4,4]

    Output: false

.. topic:: Example 3

    Input: root = []

    Output: true

.. topic:: Constraints

    The number of nodes in the tree is in the range [0, 5000].

    -104 <= Node.val <= 104

.. code-block:: java

    public boolean isBalanced(TreeNode root) {
        return getDepth(root) != -1;
    }
    
    private int getDepth(TreeNode node) {
        if (node == null) {
            return 0;
        }
        int leftDepth = getDepth(node.left);
        if (leftDepth == -1) {
            return -1;
        }
        int rightDepth = getDepth(node.right);
        if (rightDepth == -1) {
            return -1;
        }
        int result;
        if (Math.abs(leftDepth - rightDepth) > 1) {
            result = -1;
        } else {
            result = 1 + Math.max(leftDepth, rightDepth);
        }

        return result;
    }

---------------------------------------
1448. Count Good Nodes in Binary Tree
---------------------------------------

Given a binary tree root, a node X in the tree is named good if in the path from root to X there are no nodes with a value greater than X.

Return the number of good nodes in the binary tree.

.. topic:: Example 1

    Input: root = [3,1,4,3,null,1,5]

    Output: 4

    Explanation: Nodes in blue are good.

    Root Node (3) is always a good node.

    Node 4 -> (3,4) is the maximum value in the path starting from the root.

    Node 5 -> (3,4,5) is the maximum value in the path

    Node 3 -> (3,1,3) is the maximum value in the path.

.. topic:: Example 2

    Input: root = [3,3,null,4,2]

    Output: 3

    Explanation: Node 2 -> (3, 3, 2) is not good, because "3" is higher than it.

.. topic:: Example 3

    Input: root = [1]

    Output: 1

    Explanation: Root is considered as good.

.. topic:: Constraints

    The number of nodes in the binary tree is in the range [1, 10^5].

    Each node's value is between [-10^4, 10^4].

.. code-block:: java

    public int goodNodes(TreeNode root) {
        return traverse(root, Integer.MIN_VALUE);
    }
    
    private int traverse(TreeNode root, int max) {
        if (root == null) {
            return 0;
        }
        
        max = Math.max(max, root.val);
        
        int count = traverse(root.left, max);
        count += traverse(root.right, max);
        
        count += (root.val >= max)?1:0;
        
        return count;
    }