=====================
Dynamic Programming
=====================
.. contents::
    :depth: 2

----------
Overview
----------

.

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

