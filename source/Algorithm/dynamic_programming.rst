=====================
Dynamic Programming
=====================
.. contents::
    :depth: 2

---------------
55. Jump Game
---------------

You are given an integer array nums. You are initially positioned at the array's first index, and each element in the array represents your maximum jump length at that position.

Return true if you can reach the last index, or false otherwise.


.. topic:: Example 1

    Input: nums = [2,3,1,1,4]

    Output: true

    Explanation: Jump 1 step from index 0 to 1, then 3 steps to the last index.

.. topic:: Example 2

    Input: nums = [3,2,1,0,4]

    Output: false

    Explanation: You will always arrive at index 3 no matter what. Its maximum jump length is 0, which makes it impossible to reach the last index.

**Approach 1**: 

- Maintain an array of booleans where at index i, it means "can I jump from position i to the end?".
- Loop backwards; begin by filling in the array from the end.
- jump[n-1] = true; (it's always true at the end position).
- At position i, we can jump from i+1 to i+nums[i] (or til the end, whichever is first). So we check if that position is true, if so, it means we can jump from i to that position then to the end.


.. code-block:: java

    class Solution {
        // at each position i, can I jump to the end?
        // jump[n-1] = true
        // jump[i] = j from i+1 to i + nums[i], jump[j] == true
        
        public boolean canJump(int[] nums) {
            boolean[] jump = new boolean[nums.length];
            jump[nums.length - 1] = true;
            
            for (int i = nums.length - 2; i>=0; i--) {
                boolean canJ = false;
                // System.out.println("i: " + i);
                // System.out.println("i + nums[i]: " + (i + nums[i]));
                for (int j = i+1; j < Math.min(nums.length, i+nums[i]+1); j++) {
                    //System.out.print("j: " + j + " ");
                    if (jump[j]) {
                        canJ = true;
                    }
                    
                }
                // System.out.println("");
                jump[i] = canJ;
            }
            
            return jump[0];
        }    
        
    }

**Approach 2**

- Only need to maintain one index ``canJump`` indicating the location from where can jump to the end.
- Loop backwards, at current location i, if we can jump to ``canJump``, then update ``canJump`` to the current location.
- If at the end canJump == 0, return true.


.. code-block:: java

    class Solution {
        public boolean canJump(int[] nums) {
            int canJump = nums.length - 1;
            
            for (int i = nums.length -1; i>=0; i--) {
                if (i + nums[i] >= canJump) {
                    canJump = i;
                }
            }
            
            return canJump==0;
        }    
    }

-------------------
72. Edit Distance
-------------------

Given two strings word1 and word2, return the minimum number of operations required to convert word1 to word2.

You have the following three operations permitted on a word:

Insert a character

Delete a character

Replace a character

**Approach**: 

- Set dp[i][j] to be how many edits are needed to change word1[0][i-1] to word2[0][j-1].
 
    - "-1" is because row 0 and column 0 of dp are the cases when word1 or word2 is 0.

    - e.g, "horse" to "ros". First column corresponds to "horse" to ""; first row corresponds to "" to "ros".

- For dp[i][j], if word1[i-1] == word2[i-1], that means the new characters are the same, so no additional edits needed. So dp[i][j] = dp[i-1][j-1].

    - e.g. "xxxxxe" to "yyyyye" has the same number of edits as "xxxxx" to "xxxxx"

- Then there are three possibilities (for the following examples, i, j points to the last character of the two words):

    - Insert: dp[i][j] = dp[i][j-1] + 1
 
        - e.g. for "xxxxx" to "yyyyye", edits equals to edits of "xxxxx" to "yyyyy" + 1 insertion. 

    - Delete: dp[i][j] = dp[i-1][j] + 1

        - e.g. for "xxxxxe" to "yyyyy", edits equals to edits of "xxxxx" to "yyyyy" + 1 deletion. 

    - Replace: dp[i][j] = dp[i-1][j-1] + 1

        - e.g. for "xxxxxe" to "yyyyyf", edits equals to edits of "xxxxx" to "yyyyy" + 1 replace. 

    Then dp[i][j] is set to the minimum of the three cases.

- Finally returns dp[n+1][m+1].
 

.. topic:: Example 1

    Input: word1 = "horse", word2 = "ros"

    Output: 3

    Explanation: 

    horse -> rorse (replace 'h' with 'r')

    rorse -> rose (remove 'r')

    rose -> ros (remove 'e')

.. topic:: Example 2

    Input: word1 = "intention", word2 = "execution"

    Output: 5

    Explanation: 

    intention -> inention (remove 't')

    inention -> enention (replace 'i' with 'e')

    enention -> exention (replace 'n' with 'x')

    exention -> exection (replace 'n' with 'c')

    exection -> execution (insert 'u')


.. code-block:: java

    public int minDistance(String word1, String word2) {
        int n = word1.length();
        int m = word2.length();
        int[][] rst = new int[n+1][m+1];  
        
        //column 0 and row 0 means word1 or word2 are empty
        for (int i = 0; i<=n; i++){
            rst[i][0] = i;
        }
        
        for (int j = 0; j<=m; j++){
            rst[0][j] = j;
        }
        
        for (int i=1; i<=n; i++) {
            for (int j=1; j<=m; j++) {
                if (word1.charAt(i-1) == word2.charAt(j-1)) {
                    rst[i][j] = rst[i-1][j-1];                    
                    //System.out.println("i: "+i + " j: "+j + " rst: "+rst[i][j]);
                    continue;
                }
                
                rst[i][j] = Math.min(Math.min(rst[i-1][j-1] + 1, rst[i][j-1] + 1), rst[i-1][j] + 1);
                //System.out.println("i: "+i + " j: "+j + " rst: "+rst[i][j]);
            }
        }
        
        return rst[n][m];
    }

----------------------------
115. Distinct Subsequences
----------------------------

Given two strings s and t, return the number of distinct subsequences of s which equals t.

A string's subsequence is a new string formed from the original string by deleting some (can be none) of the characters without disturbing the remaining characters' relative positions. (i.e., "ACE" is a subsequence of "ABCDE" while "AEC" is not).

It is guaranteed the answer fits on a 32-bit signed integer.

 

.. topic:: Example 1

    Input: s = "rabbbit", t = "rabbit"

    Output: 3

    Explanation:

    As shown below, there are 3 ways you can generate "rabbit" from S.

    **rabb** b **it**

    **ra** b **bbit**

    **rab** b **bit**


.. topic:: Example 2

    Input: s = "babgbag", t = "bag"

    Output: 5

    Explanation:

    As shown below, there are 5 ways you can generate "bag" from S.

    **ba** b **g** bag

    **ba** bgba **g**

    **b** abgb **ag**

    ba **b** gb **ag**

    babg **bag**
 

.. topic:: Constraints

    1 <= s.length, t.length <= 1000

    s and t consist of English letters.

**Approach**

- rst[i][j] means the number of distinct subsequences of t[0][j] in s[0][i].

- The first column rst[i][0] equals to the number of t[0] in s.

    - For example, s = "rabbbit", t = "rabbit", rst[i][0][i] is the number of r in "r", "ra", "rab", ... "rabbbit".

- Then we fill in column by column, from left to right.

- rst[j][j] is special, it is if t[0][j] and s[0][j] are equal. 

- We don't need to consider rst[i][j] where j>i

- Then for rst[i][j], compare if s[i] and t[j] are the same. 
    
    - If they are not the same, rst[i][j] = rst[i-1][j] (copy the previous element)

        - e.g. if rst[i][j] is number of "rab" in "rabbbi**t**" and rst[i-1][j] is number of "rab" in "rabbbi", then they are the same because "t" != "b".

    - If they are the same, rst[i][j] = rst[i-1][j-1] + rst[i-1][j]

        - e.g. Consider rst[i][j] is "rabb" in "rabbb", then rst[i-1][j] is "rabb" in "rabb" (1) and rst[i-1][j-1] is "rab" in "rabb" (2).

            - From "rab" in "rabb", we have "**rab** b" and "**ra** b **b**". Now for rst[i][j] we can add an additional b at the end: "**rab**  b **b**" and "**ra** b **bb**"

            - From "rabb" in "rabb", we have "**rabb**". Now for rst[i][j] we again add an additional b at the end: "**rabbb**" 

            - Add them together, we know rst[i][j]=3:  "**rab**  b **b**" and "**ra** b **bb**" and "**rabbb**".

- Finally we output rst[s.length()-1][t.length()-1].


.. code-block:: java

    public int numDistinct(String s, String t) {
        if (t.length() > s.length()) {
            return 0;
        }
        
        int[][] rst = new int[s.length()][t.length()];
        
        if (s.charAt(0) == t.charAt(0)) {
            rst[0][0] = 1;
        } else {
            rst[0][0] = 0;
        }
        
        // System.out.println("i: "+ 0 + " j: "+0 + " rst: " + rst[0][0]);
        
        // for j==0
        for (int i=1; i<s.length(); i++) {
            if (s.charAt(i) == t.charAt(0)) {
                rst[i][0] = rst[i-1][0]+1;
            } else {
                rst[i][0] = rst[i-1][0];
            }
            // System.out.println("i: "+i + " j: "+0 + " rst: " + rst[i][0]);
        }
        
        for (int j = 1; j<t.length(); j++) {
            if (rst[j-1][j-1] == 1 && s.charAt(j) == t.charAt(j)) {
                rst[j][j] = 1;
            } else {
                rst[j][j] = 0;
            }
            
            // System.out.println("i: "+j + " j: "+j + " rst: " + rst[j][j]);
            
            for (int i=j+1; i<s.length(); i++) {
                if (s.charAt(i) == t.charAt(j)) {
                    // if (rst[i][j-1] == rst[i-1][j-1] + 1) {
                    //     rst[i][j] = rst[i][j-1];
                    // } else {
                    //     rst[i][j] = rst[i][j-1] + rst[i-1][j];
                    // }
                    rst[i][j] = rst[i-1][j-1] + rst[i-1][j];
                } else {
                    rst[i][j] = rst[i-1][j];
                }
                // System.out.println("i: "+i + " j: "+j + " rst: " + rst[i][j]);
            }
        }
        return rst[s.length()-1][t.length()-1];
    }

-----------------------------------
124. Binary Tree Maximum Path Sum
-----------------------------------

A path in a binary tree is a sequence of nodes where each pair of adjacent nodes in the sequence has an edge connecting them. A node can only appear in the sequence at most once. Note that the path does not need to pass through the root.

The path sum of a path is the sum of the node's values in the path.

Given the root of a binary tree, return the maximum path sum of any path.

**Approach**

- Given a node A, we need to calculate the path sum assuming A is the root node.

- There are four possible cases (since node values can be negative):

    - A + pathSum of the left child tree

    - A + pathSum of the right child tree

    - Only A

    - A + pathSum of both children trees

- Observe that for A's parent, only the first three cases can be considered (these are the sums that can be used by the parent). Because if a path includes A and both of it's children, this path cannot be added to the path that goes through A's parent (this is the sum that cannot be used by the parent).

- Therefore for each node, we calculate two sums: one is the path sum of A as the root, which cannot be used by the parent; the other one is the max of the first three cases, which can be used by the A's parent.

- We can keep a global variable that keep record of the running maximum. 

- Then when doing tree traversal, return the sum that can be used by the parent for each node. Meanwhile compare the results of the four cases to the global maximum.

.. code-block:: java

    class Solution {
        int rst = Integer.MIN_VALUE;
        
        public int maxPathSum(TreeNode root) {
            traverse(root);
            
            return rst;
        }
        
        private int traverse(TreeNode root) {
            if (root == null) {
                return 0;
            }
            
            int leftSum = traverse(root.left);
            int rightSum = traverse(root.right);
            
            // parent can use
            int sumForParent = Math.max(Math.max(leftSum + root.val, rightSum+root.val), root.val);
            
            // parent cannot use
            int sumNotForParent = leftSum + rightSum + root.val;
            
            rst =  Math.max(Math.max(sumForParent, rst), sumNotForParent);
            
            return sumForParent;        
        }
    }

-------------------
174. Dungeon Game
-------------------

The demons had captured the princess and imprisoned her in the bottom-right corner of a dungeon. The dungeon consists of m x n rooms laid out in a 2D grid. Our valiant knight was initially positioned in the top-left room and must fight his way through dungeon to rescue the princess.

The knight has an initial health point represented by a positive integer. If at any point his health point drops to 0 or below, he dies immediately.

Some of the rooms are guarded by demons (represented by negative integers), so the knight loses health upon entering these rooms; other rooms are either empty (represented as 0) or contain magic orbs that increase the knight's health (represented by positive integers).

To reach the princess as quickly as possible, the knight decides to move only rightward or downward in each step.

Return the knight's minimum initial health so that he can rescue the princess.

Note that any room can contain threats or power-ups, even the first room the knight enters and the bottom-right room where the princess is imprisoned.

**Approach**

- Keep a 2D array rst where rst[i][j] means the min health it required to enter dungeon[i][j].

- Suppose we are going from room A to room B. The minimum health required to enter room B is t and suppose dungeon[A] is c. Then the health requirement of room A is h + c = t. If c is larger than t, e.g. if we can gain 30 health at room A and B requires only 10 health, the health requirement of A is then 1. So h = max(1, t-c).

- For any room, we can either go right or go down, choose whichever is less or whichever is go-able.

- Then we just traverse from the bottom-right up till top-left then output rst[0][0];

.. code-block:: java

    public int calculateMinimumHP(int[][] dungeon) {
        int m = dungeon.length;
        int n = dungeon[0].length;
        int[][] rst = new int[m][n];
        
        //System.out.println("i: "+(m-1)+" j: "+(n-1)+" rst: "+rst[m-1][n-1]);
        
        for (int i = m-1; i>=0; i--) {
            for (int j = n-1; j>=0; j--) {
                
                // bottom-right
                if (j == n-1 && i == m-1) {
                    rst[i][j] = getH(1, dungeon[i][j]);
                } else if (j == n-1) {
                    // can't go right
                    rst[i][j] = getH(rst[i+1][j], dungeon[i][j]);
                } else if (i == m-1) {
                    // can't go down
                    rst[i][j] = getH(rst[i][j+1], dungeon[i][j]);
                } else {
                    //rst[i][j] = Math.min(getH(rst[i+1][j], dungeon[i+1][j]), getH(rst[i][j+1], dungeon[i][j+1]));
                    rst[i][j] = Math.min(getH(rst[i+1][j], dungeon[i][j]), getH(rst[i][j+1], dungeon[i][j]));
                }
                
                //System.out.println("i: "+i+" j: "+j+" rst: "+rst[i][j]);
            }
        }
        
        return rst[0][0];
    }
    
    private int getH(int t, int c) {
        return Math.max(t-c, 1);
    }

---------------------
70. Climbing Stairs
---------------------

You are climbing a staircase. It takes n steps to reach the top.

Each time you can either climb 1 or 2 steps. In how many distinct ways can you climb to the top?

.. topic:: Example 1

    Input: n = 2

    Output: 2

    Explanation: There are two ways to climb to the top.

    1. 1 step + 1 step

    2. 2 steps

.. topic:: Example 2

    Input: n = 3

    Output: 3

    Explanation: There are three ways to climb to the top.

    1. 1 step + 1 step + 1 step

    2. 1 step + 2 steps

    3. 2 steps + 1 step
 

.. topic:: Constraints

    1 <= n <= 45

.. code-block:: java

    public int climbStairs(int n) {
        int[] rst = new int[n+1]; // number of ways to get to step k

        for (int i=0; i<= n; i++) {
            if (i <= 2) {
                rst[i] = i; // It's the same as setting rst[0] = 1. Because for rst[2], we either jump from step 0 or from step 1.
            } else {
                rst[i] = rst[i-1] + rst[i-2];
            }
        }
        
        return rst[n];
    }

-------------------------------
746. Min Cost Climbing Stairs
-------------------------------

You are given an integer array cost where cost[i] is the cost of ith step on a staircase. Once you pay the cost, you can either climb one or two steps.

You can either start from the step with index 0, or the step with index 1.

Return the minimum cost to reach the top of the floor.

.. topic:: Example 1

    Input: cost = [10,15,20]

    Output: 15

    Explanation: Cheapest is: start on cost[1], pay that cost, and go to the top.

.. topic:: Example 2

    Input: cost = [1,100,1,1,1,100,1,1,100,1]

    Output: 6

    Explanation: Cheapest is: start on cost[0], and only step on 1s, skipping cost[3].

.. topic:: Constraints

    2 <= cost.length <= 1000

    0 <= cost[i] <= 999


.. code-block:: java

    public int minCostClimbingStairs(int[] cost) {
        int[] rst = new int[cost.length+1]; // cost need to pay to go to step k
        
        for (int i=0; i<=cost.length; i++) {
            if (i<=1) {
                rst[i] = 0;
            } else {
                rst[i] = Math.min(rst[i-1] + cost[i-1], rst[i-2] + cost[i-2]);
            }    
        }
        
        return rst[cost.length];
    }

------------------
62. Unique Paths
------------------

A robot is located at the top-left corner of a m x n grid (marked 'Start' in the diagram below).

The robot can only move either down or right at any point in time. The robot is trying to reach the bottom-right corner of the grid (marked 'Finish' in the diagram below).

How many possible unique paths are there?

.. topic:: Example 1:

    Input: m = 3, n = 7

    Output: 28

.. topic:: Example 2:

    Input: m = 3, n = 2

    Output: 3

    Explanation:

    From the top-left corner, there are a total of 3 ways to reach the bottom-right corner:

    1. Right -> Down -> Down

    2. Down -> Down -> Right

    3. Down -> Right -> Down

.. topic:: Example 3:

    Input: m = 7, n = 3

    Output: 28

.. topic:: Example 4:

    Input: m = 3, n = 3

    Output: 6
 
.. topic:: Constraints:

    1 <= m, n <= 100

    It's guaranteed that the answer will be less than or equal to 2 * 109.

.. code-block:: java

    public int uniquePaths(int m, int n) {
        int[][] rst = new int[m][n]; // rst[i][j] is how many unique ways can go from i, j to m, n
        
        //rst[i][j] = rst[i+1][j] (move down) + rst[i][j+1] (move right)
        
        // traverse from bottom to top, right to left
        for (int j=n-1; j>=0; j--){
            for (int i=m-1; i>=0; i--) {
                if (j==n-1 && i==m-1) {
                    // arrived
                    rst[i][j] = 1;
                } else if (j==n-1) {
                    // rightmost column, can only move down
                    rst[i][j] = rst[i+1][j];
                } else if (i==m-1) {
                // bottom column, can only move right
                    rst[i][j] = rst[i][j+1];
                } else {
                    rst[i][j] = rst[i+1][j] + rst[i][j+1];
                }
                
            }
        }
        
        return rst[0][0];
        
    }

---------------------
63. Unique Paths II
---------------------

A robot is located at the top-left corner of a m x n grid (marked 'Start' in the diagram below).

The robot can only move either down or right at any point in time. The robot is trying to reach the bottom-right corner of the grid (marked 'Finish' in the diagram below).

Now consider if some obstacles are added to the grids. How many unique paths would there be?

An obstacle and space is marked as 1 and 0 respectively in the grid.

.. topic:: Example 1:

    Input: obstacleGrid = [[0,0,0],[0,1,0],[0,0,0]]

    Output: 2

    Explanation: There is one obstacle in the middle of the 3x3 grid above.

    There are two ways to reach the bottom-right corner:

    1. Right -> Right -> Down -> Down

    2. Down -> Down -> Right -> Right

.. topic:: Example 2:

    Input: obstacleGrid = [[0,1],[0,0]]

    Output: 1
 
.. topic:: Constraints:

    m == obstacleGrid.length

    n == obstacleGrid[i].length

    1 <= m, n <= 100

    obstacleGrid[i][j] is 0 or 1.

.. code-block:: java

    public int uniquePathsWithObstacles(int[][] obstacleGrid) {
        int n = obstacleGrid[0].length;
        int m = obstacleGrid.length;
        int[][] rst = new int[m][n]; // rst[i][j] = how many ways from i,j to m-1, n-1
        
        for (int j=n-1; j>=0; j--) {
            for (int i=m-1; i>=0; i--) {
                if (obstacleGrid[i][j] == 1) {
                    rst[i][j] = 0;
                } else if (i==m-1 && j==n-1) {
                    rst[i][j] = 1;
                } else if (i==m-1){
                    rst[i][j] = rst[i][j+1];
                } else if (j==n-1) {
                    rst[i][j] = rst[i+1][j];
                } else {
                    // move down and move right
                    rst[i][j] = rst[i+1][j] + rst[i][j+1];
                }

            }
        }
        
        return rst[0][0];
        
    }

--------------------
343. Integer Break
--------------------

Given an integer n, break it into the sum of k positive integers, where k >= 2, and maximize the product of those integers.

Return the maximum product you can get.

.. topic:: Example 1:

    Input: n = 2

    Output: 1

    Explanation: 2 = 1 + 1, 1 × 1 = 1.

.. topic:: Example 2:

    Input: n = 10

    Output: 36

    Explanation: 10 = 3 + 3 + 4, 3 × 3 × 4 = 36.

.. topic:: Constraints:

    2 <= n <= 58

**Approach:** rst[i] means the maximum product given n == i. We get all the possible 2 splits of i, then calculate the maximum product from them.

  - For example, 7=1+6=2+5=3+4. For 1+6, we already knew rst[1] == 1 and rst[6] == 9. Then this gives 1*9=9. Similar for 2+5, we knew rst[2] == 1, rst[5] == 6, this gives 2*6=12.

.. code-block:: java

    public int integerBreak(int n) {
        int[] rst = new int[n+1];
        rst[1] = 1;
        
        for (int i=2; i<=n; i++){
            for (int k=1; k<=i; k++) {
                int p = i-k;
                //System.out.println("i: "+i + " k: "+ k + " p: "+ p);
                rst[i] = Math.max(rst[i], Math.max(k, rst[k]) * Math.max(p, rst[p]));
                //System.out.println(rst[i]);
                
                if (p==k || p==k+1) {
                    break;
                }
            }
        }
        
        return rst[n];
    }

--------------------------------
96. Unique Binary Search Trees
--------------------------------

Given an integer n, return the number of structurally unique BST's (binary search trees) which has exactly n nodes of unique values from 1 to n.

.. topic:: Example 1:

    Input: n = 3

    Output: 5

.. topic:: Example 2:

    Input: n = 1

    Output: 1

.. topic:: Constraints:

    1 <= n <= 19

.. code-block:: java

    public int numTrees(int n) {
        int[] rst = new int[n+1];
        rst[0] = 1; 
        rst[1] = 1;
         
        for (int i = 2; i<n+1; i++) {
            for (int k = 0; k<i; k++) {
                //System.out.println(" i: " + i + " k: "+k + " " + (i-1-k) );
                rst[i] = rst[i] + rst[k] * rst[i-1-k];
            }
        }
        return rst[n];
    }

------------------------------------------------
416. Partition Equal Subset Sum (0-1 Knapsack)
------------------------------------------------

Given a non-empty array nums containing only positive integers, find if the array can be partitioned into two subsets such that the sum of elements in both subsets is equal.

.. topic:: Example 1

    Input: nums = [1,5,11,5]

    Output: true

    Explanation: The array can be partitioned as [1, 5, 5] and [11].

.. topic:: Example 2

    Input: nums = [1,2,3,5]

    Output: false

    Explanation: The array cannot be partitioned into equal sum subsets.
     
.. topic:: Constraints

    1 <= nums.length <= 200

    1 <= nums[i] <= 100

**Approach**: This problem can be translate to a 0-1 Knapsack problem. First we calculate half of the sum (call it sum). Then we consider sum as the total weight, the given nums as an array of weights, and we try to fill the knapsack with these weights. rst[i][j] means choosing from item 0 to i, get the maximum weights we can fit into the knapsack with capacity j. Then if any value in rst[i][sum] is equal to sum, that means we can fill exactly half of the total sum with some of the items, then we can return true.

.. code-block:: java

    public boolean canPartition(int[] nums) {
        int sum = Arrays.stream(nums).sum();
        
        if (sum%2 == 1) {
            return false;
        }
        
        int[][] rst = new int[nums.length][sum+1];
        
        for (int j=0; j<sum+1; j++) {
            if (j >= nums[0]) {
                rst[0][j] = nums[0];
            }
        }
        
        for (int i=1; i<nums.length; i++) {
            for (int j=1; j<sum+1; j++) {
                rst[i][j] = Math.max(rst[i-1][j], (j>nums[i])?(rst[i-1][j-nums[i]] + nums[i]):0);
            }
        }
        
        for (int i=0; i<nums.length; i++) {
            if (rst[i][sum/2] == sum/2) {
                return true;
            }
        }
        return false;
    }

-------------------------------------
Last Stone Weight II (0-1 Knapsack)
-------------------------------------

You are given an array of integers stones where stones[i] is the weight of the ith stone.

We are playing a game with the stones. On each turn, we choose any two stones and smash them together. Suppose the stones have weights x and y with x <= y. The result of this smash is:

If x == y, both stones are destroyed, and
If x != y, the stone of weight x is destroyed, and the stone of weight y has new weight y - x.
At the end of the game, there is at most one stone left.

Return the smallest possible weight of the left stone. If there are no stones left, return 0.

.. topic:: Example 1

    Input: stones = [2,7,4,1,8,1]

    Output: 1

    Explanation:

    We can combine 2 and 4 to get 2, so the array converts to [2,7,1,8,1] then,

    we can combine 7 and 8 to get 1, so the array converts to [2,1,1,1] then,

    we can combine 2 and 1 to get 1, so the array converts to [1,1,1] then,

    we can combine 1 and 1 to get 0, so the array converts to [1], then that's the optimal value.

.. topic:: Example 2

    Input: stones = [31,26,33,21,40]

    Output: 5

.. topic:: Example 3

    Input: stones = [1,2]

    Output: 1

.. topic:: Constraints

    1 <= stones.length <= 30

    1 <= stones[i] <= 100

**Approach**: We can think of this problem as splitting the input array into 2 subarrays so that the difference between the sums of the two subarrays is minimized. Then the result is the difference between the sums. 

This can be done using 0-1 Knapstack with nums as both values and weights. We calculate all the columns until half of the sum, then when we reach half (ceiling of sum/2), we start evaluate if rst[i][j] == j. If it's equal, that means some elements can sum to j. Then we calculate the difference between j and sum-j.

    - e.g. The half of [31,26,33,21,40] is 76. We will get that rst[3][78] == 78. That is because 31+26+31 == 78. Then sum-78 = 73, which is because 33+40 == 73. Then 78-73 = 5 and we have the answer.

Some catches:

- We start evaluate from ceiling of sum/s because suppose we have an odd sum 23, and some of the elements sum to 12, the other elements sum to 11, then we can do j- (sum-j) to get the final answer because j > sum-j. Alternatively we can just use half = sum/2 and Math.abs(j - (sum-j)).

- The inner loop has to be i because we want to get all values calculated for a column j (=half+) and determine if some elements sum to exactly that j, then continue with column j+1, j+2, etc.

.. code-block:: java

    public int lastStoneWeightII(int[] stones) {
        int sum = Arrays.stream(stones).sum();
        int half = (sum%2 == 1)?(sum/2+1):sum/2;
        
        //System.out.println("half: " + half);
        int[][] rst = new int[stones.length][sum+1];
        
        for (int j=stones[0]; j<=sum; j++) {
            rst[0][j] = stones[0];
        }
        
        for (int j=1; j<=sum; j++) {
            for (int i=1; i<stones.length; i++) {
                if (j<stones[i]) {
                    rst[i][j] = rst[i-1][j];
                } else {
                    rst[i][j] = Math.max(rst[i-1][j], rst[i-1][j-stones[i]] + stones[i]);
                }
                
                if (j>=half) {
                    //System.out.println("i: " + i + " j: " + j + " rst: "+ rst[i][j]);
                    if (rst[i][j] == j) {
                        return j- (sum-j);
                    }
                }
            }
        }
        
        return sum;
    }


----------------------------
494. Target Sum (Knapsack)
----------------------------

You are given an integer array nums and an integer target.

You want to build an expression out of nums by adding one of the symbols '+' and '-' before each integer in nums and then concatenate all the integers.

For example, if nums = [2, 1], you can add a '+' before 2 and a '-' before 1 and concatenate them to build the expression "+2-1".
Return the number of different expressions that you can build, which evaluates to target.

.. topic:: Example 1

    Input: nums = [1,1,1,1,1], target = 3

    Output: 5

    Explanation: There are 5 ways to assign symbols to make the sum of nums be target 3.

    -1 + 1 + 1 + 1 + 1 = 3

    +1 - 1 + 1 + 1 + 1 = 3

    +1 + 1 - 1 + 1 + 1 = 3

    +1 + 1 + 1 - 1 + 1 = 3

    +1 + 1 + 1 + 1 - 1 = 3

.. topic:: Example 2

    Input: nums = [1], target = 1

    Output: 1
     
.. topic:: Constraints

    1 <= nums.length <= 20

    0 <= nums[i] <= 1000

    0 <= sum(nums[i]) <= 1000

    -1000 <= target <= 1000

**Approach:** This problem can be considered as an "1-1 Knapsack" problem: instead of choosing/not choosing an item, it is plus/minus an item. So the problem is given an array of items, we have a knapsack of capacity "target", we can choose to plus or minus the items in order, how many ways can we fill the knapsack exactly.

Now consider the table rst, where rows are from item 0 to m, and columns are from -sum to sum (sum of nums). Then rst[i][j] means given item 0 to i, how many ways can we fill a knapsack of capacity j.

We can get rst[i][j] = rst[i-1][j-nums[i]] (plus item i) + rst[i-1][j+nums[i]] (minus item i).

    - e.g. nums = [1,1,1,1,1]. rst[1][0] = rst[0][-1] + rst[0][+1] (Given two 1's, how many ways can we get to 0 = given one 1, how many ways can we get to -1 + given one 1, how many ways can we get to +1). 

We need to iterate rows by rows because rst[i] only depends on rst[i-1]. 

The first code snippet is a slightly better implementation. We have two rst tables, rstp and rstn, to store "j= 0 to sum" and "j= 0 to -sum" separately. Then we first calculate for +j, then we calculate for -j. Trick is to get the sign correct and fetch values from the correct table.

The second code snippet improves on that. We notice that rstp and rstn are completely identical (which makes sense because the signs are all symmetric). So we can reduce to just one table and adjust the sign accordingly.

.. code-block:: java

    public int findTargetSumWays(int[] nums, int target) {
        int sum = Arrays.stream(nums).sum();
        
        if (target > sum || target < -sum) {
            return 0;
        }
        
        int[][] rstp = new int[nums.length][sum+1]; // j = 0 to sum
        int[][] rstn = new int[nums.length][sum+1]; // j = 0 to -sum
        
        // CATCH!! 0 has two ways to get to 0.
        if (nums[0] != 0) {
            rstp[0][nums[0]] = 1;
            rstn[0][nums[0]] = 1;
        } else {
            rstp[0][nums[0]] = 2;
            rstn[0][nums[0]] = 2;
        }
        
            
        for (int i=1; i<nums.length; i++) {
            for (int j=0; j<=sum; j++) {
                // positive j
                int countp = 0;
                int countn = 0;
                
                if (j-nums[i] <= sum && j-nums[i] >= -sum) {
                    if (j-nums[i] >= 0) {
                        countp = rstp[i-1][j-nums[i]];
                    } else {
                        countp = rstn[i-1][-(j-nums[i])];
                    }
                }
                
                if (j+nums[i] <= sum && j+nums[i] >= -sum) {
                    if (j+nums[i] >= 0) {
                        countn = rstp[i-1][j+nums[i]];
                    } else {
                        countn = rstn[i-1][-(j+nums[i])];
                    }
                }
               
                rstp[i][j] = countp + countn;
                
                // negative j   
                countp = 0;
                countn = 0;
                if (-j-nums[i] <= sum && -j-nums[i] >= -sum) {
                    if (-j-nums[i] >= 0) {
                        countp = rstp[i-1][-j-nums[i]];
                    } else {
                        countp = rstn[i-1][j+nums[i]]; // reverse order in negative rst
                    }
                }
                
                if (-j+nums[i] <= sum && -j+nums[i] >= -sum) {
                    if (-j+nums[i] >= 0) {
                    countn = rstp[i-1][-j+nums[i]];
                    } else {
                        countn = rstn[i-1][-(-j+nums[i])];
                    }
                }
                
                rstn[i][j] = countp + countn;
            }
        }
        
        return (target>=0)?rstp[nums.length-1][target]:rstn[nums.length-1][-target];
        
    }


.. code-block:: java

    public int findTargetSumWays(int[] nums, int target) {
        int sum = Arrays.stream(nums).sum();
        
        if (target > sum || target < -sum) {
            return 0;
        }
        
        int[][] rst = new int[nums.length][sum+1]; 
        
        if (nums[0] != 0) {
            rst[0][nums[0]] = 1;
        } else {
            rst[0][nums[0]] = 2;
        }
        
            
        for (int i=1; i<nums.length; i++) {
            for (int j=0; j<=sum; j++) {
                int countp = 0;
                int countn = 0;
                
                if (Math.abs(j-nums[i]) <= sum) {
                    countp = rst[i-1][Math.abs(j-nums[i])];
                }
                
                if (Math.abs(j+nums[i]) <= sum) {
                    countn = rst[i-1][Math.abs(j+nums[i])];
                }
               
                rst[i][j] = countp + countn;
            }
        }
        
        return rst[nums.length-1][Math.abs(target)];
        
    }

-------------------------------------
474. Ones and Zeroes (0-1 Knapsack)
-------------------------------------

You are given an array of binary strings strs and two integers m and n.

Return the size of the largest subset of strs such that there are at most m 0's and n 1's in the subset.

A set x is a subset of a set y if all elements of x are also elements of y.

.. topic:: Example 1

    Input: strs = ["10","0001","111001","1","0"], m = 5, n = 3

    Output: 4

    Explanation: The largest subset with at most 5 0's and 3 1's is {"10", "0001", "1", "0"}, so the answer is 4.

    Other valid but smaller subsets include {"0001", "1"} and {"10", "1", "0"}.

    {"111001"} is an invalid subset because it contains 4 1's, greater than the maximum of 3.

.. topic:: Example 2

    Input: strs = ["10","0","1"], m = 1, n = 1

    Output: 2

    Explanation: The largest subset is {"0", "1"}, so the answer is 2.
 
.. topic:: Constraints

    1 <= strs.length <= 600

    1 <= strs[i].length <= 100

    strs[i] consists only of digits '0' and '1'.

    1 <= m, n <= 100

**Approach:** Need to use the 3-dimensional arrays rst. rst[i][j][k] means the maximum subsets of String 0 to String i that has no more than j 1's and k 0's. Then rst[i][j][k] = Math.max(rst[i-1][j][k] (do not choose i), rst[i-1][j-count0[i]][k-count1[i]] + 1 (choose i)).

.. code-block:: java

    public int findMaxForm(String[] strs, int m, int n) {
        int[][][] rst = new int[strs.length][m+1][n+1];
    
        int[] count0 = new int[strs.length];
        int[] count1 = new int[strs.length];
        
        for (int i=0; i<strs.length; i++) {
            int c0 = 0;
            int c1 = 0;
            for (int j=0; j<strs[i].length(); j++){
                if (strs[i].charAt(j)=='0') {
                    c0++;
                } else {
                    c1++;
                }
            }
            count0[i] = c0;
            count1[i] = c1;
        }
        
        for (int j=0; j<m+1; j++) {
            for (int k=0; k<n+1; k++) {
                if (j>=count0[0] && k>=count1[0]) {
                    rst[0][j][k] = 1;
                }
            }
        }
        
        for (int i=1; i<strs.length; i++) {
            for (int j=0; j<m+1; j++) {
                for (int k=0; k<n+1; k++) {
                    if (j<count0[i] || k < count1[i]) {
                        rst[i][j][k] = rst[i-1][j][k];
                    } else{
                        rst[i][j][k] = Math.max(rst[i-1][j][k], rst[i-1][j-count0[i]][k-count1[i]] + 1);
                    }
                    
                    //System.out.println("i: "+i + " j: " + j + " k: "+ k + " rst: " + rst[i][j][k]);
                }
            }
        }
        
        return rst[strs.length-1][m][n];
    }

-------------------------------
518. Coin Change 2 (Knapsack)
-------------------------------

**Combination**

You are given an integer array coins representing coins of different denominations and an integer amount representing a total amount of money.

Return the number of combinations that make up that amount. If that amount of money cannot be made up by any combination of the coins, return 0.

You may assume that you have an infinite number of each kind of coin.

The answer is guaranteed to fit into a signed 32-bit integer.

.. topic:: Example 1

    Input: amount = 5, coins = [1,2,5]

    Output: 4

    Explanation: there are four ways to make up the amount:

    5=5

    5=2+2+1

    5=2+1+1+1

    5=1+1+1+1+1

.. topic:: Example 2

    Input: amount = 3, coins = [2]

    Output: 0

    Explanation: the amount of 3 cannot be made up just with coins of 2.

.. topic:: Example 3:

    Input: amount = 10, coins = [10]

    Output: 1

.. topic:: Constraints

    1 <= coins.length <= 300

    1 <= coins[i] <= 5000

    All the values of coins are unique.

    0 <= amount <= 5000

.. code-block:: java

    public int change(int amount, int[] coins) {
        int[] rst = new int[amount+1];
        
        rst[0] = 1;
        
        int count;
        
        for (int i=0; i<coins.length; i++) {
            for (int j = coins[i]; j <= amount; j++) {
                rst[j] += rst[j - coins[i]];
            }
        }
        
        return rst[amount];
    }

------------------------------------
377. Combination Sum IV (Knapsack)
------------------------------------

**Permutation**

Given an array of distinct integers nums and a target integer target, return the number of possible combinations that add up to target.

The answer is guaranteed to fit in a 32-bit integer.

.. topic:: Example 1

    Input: nums = [1,2,3], target = 4

    Output: 7

    Explanation:

    The possible combination ways are:

    (1, 1, 1, 1)

    (1, 1, 2)

    (1, 2, 1)

    (1, 3)

    (2, 1, 1)

    (2, 2)

    (3, 1)

    Note that different sequences are counted as different combinations.

.. topic:: Example 2

    Input: nums = [9], target = 3

    Output: 0
 
.. topic:: Constraints

    1 <= nums.length <= 200

    1 <= nums[i] <= 1000

    All the elements of nums are unique.

    1 <= target <= 1000

Follow up: What if negative numbers are allowed in the given array? How does it change the problem? What limitation we need to add to the question to allow negative numbers?

.. code-block:: java

    public int combinationSum4(int[] nums, int target) {
        int[] rst = new int[target+1];
        
        rst[0] = 1;
        
        for (int j = 0; j <= target; j++) {
            for (int i = 0; i < nums.length; i++) {
                if (j - nums[i] >= 0) {
                    rst[j] += rst[j - nums[i]];
                }
                System.out.println("i: "+i + " j: "+j + " rst: "+rst[j]);
            }
        }
        
        return rst[target];
    }

---------------------------------
279. Perfect Squares (Knapsack)    
---------------------------------

Given an integer n, return the least number of perfect square numbers that sum to n.

A perfect square is an integer that is the square of an integer; in other words, it is the product of some integer with itself. For example, 1, 4, 9, and 16 are perfect squares while 3 and 11 are not.

.. topic:: Example 1

    Input: n = 12

    Output: 3

    Explanation: 12 = 4 + 4 + 4.

.. topic:: Example 2

    Input: n = 13

    Output: 2

    Explanation: 13 = 4 + 9.

.. topic:: Constraints

    1 <= n <= 104

.. code-block:: java

    public int numSquares(int n) {
        int[] rst = new int[n+1];
        int min = n;
        
        for (int j=0; j<=n; j++) {
            rst[j] = j;
        }
        
        for (int i=2; i*i<=n; i++) {
            for (int j=i*i; j<=n; j++) {
                rst[j] = Math.min(rst[j], rst[j-i*i] + 1);
                //System.out.println("i: "+i + " j: " + j + " rst: "+ rst[j]);
            }
        }
        
        return rst[n];
    }

-------------------
198. House Robber
-------------------

You are a professional robber planning to rob houses along a street. Each house has a certain amount of money stashed, the only constraint stopping you from robbing each of them is that adjacent houses have security systems connected and it will automatically contact the police if two adjacent houses were broken into on the same night.

Given an integer array nums representing the amount of money of each house, return the maximum amount of money you can rob tonight without alerting the police.

.. topic:: Example 1

    Input: nums = [1,2,3,1]

    Output: 4

    Explanation: Rob house 1 (money = 1) and then rob house 3 (money = 3).

    Total amount you can rob = 1 + 3 = 4.

.. topic:: Example 2

    Input: nums = [2,7,9,3,1]

    Output: 12

    Explanation: Rob house 1 (money = 2), rob house 3 (money = 9) and rob house 5 (money = 1).

    Total amount you can rob = 2 + 9 + 1 = 12.

.. topic:: Constraints

    1 <= nums.length <= 100

    0 <= nums[i] <= 400

.. code-block:: java

     public int rob(int[] nums) {
        int prepre = 0;
        int pre = 0;
        int current = 0;
        
        for (int i=0; i<nums.length; i++) {
            current = Math.max(nums[i] + prepre, pre);
            prepre = pre;
            pre = current;
        }
        
        return current;
    }

----------------------
213. House Robber II
----------------------

You are a professional robber planning to rob houses along a street. Each house has a certain amount of money stashed. All houses at this place are arranged in a circle. That means the first house is the neighbor of the last one. Meanwhile, adjacent houses have a security system connected, and it will automatically contact the police if two adjacent houses were broken into on the same night.

Given an integer array nums representing the amount of money of each house, return the maximum amount of money you can rob tonight without alerting the police.

 

.. topic:: Example 1

    Input: nums = [2,3,2]

    Output: 3

    Explanation: You cannot rob house 1 (money = 2) and then rob house 3 (money = 2), because they are adjacent houses.

.. topic:: Example 2

    Input: nums = [1,2,3,1]

    Output: 4

    Explanation: Rob house 1 (money = 1) and then rob house 3 (money = 3).

    Total amount you can rob = 1 + 3 = 4.

.. topic:: Example 3

    Input: nums = [1,2,3]

    Output: 3

.. topic:: Constraints:

    1 <= nums.length <= 100

    0 <= nums[i] <= 1000

.. code-block:: java

    public int rob(int[] nums) {
        int prepre = 0;
        int pre = 0;
        
        if (nums.length == 1) {
            return nums[0];
        }
        
        int first = 0; 
        for (int i=1; i<nums.length; i++) {
            first = Math.max(nums[i] + prepre, pre);
            prepre = pre;
            pre = first;
            //System.out.println("f: " + first);
        }
        
        prepre = 0;
        pre = 0;
        int last = 0; 
        for (int i=0; i<nums.length-1; i++) {
            last = Math.max(nums[i] + prepre, pre);
            prepre = pre;
            pre = last;
            //System.out.println("l: " + last);
        }
        
        //System.out.println(first + " " + last);
        
        return Math.max(first, last);
    }
