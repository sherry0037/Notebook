=======
Graph
=======

-----------------------------------------------------
1761. Minimum Degree of a Connected Trio in a Graph
-----------------------------------------------------

You are given an undirected graph. You are given an integer n which is the number of nodes in the graph and an array edges, where each edges[i] = [ui, vi] indicates that there is an undirected edge between ui and vi.

A connected trio is a set of three nodes where there is an edge between every pair of them.

The degree of a connected trio is the number of edges where one endpoint is in the trio, and the other is not.

Return the minimum degree of a connected trio in the graph, or -1 if the graph has no connected trios.

.. topic:: Example 1

	Input: n = 6, edges = [[1,2],[1,3],[3,2],[4,1],[5,2],[3,6]]

	Output: 3

	Explanation: There is exactly one trio, which is [1,2,3]. The edges that form its degree are bolded in the figure above.

.. topic:: Example 2

	Input: n = 7, edges = [[1,3],[4,1],[4,3],[2,5],[5,6],[6,7],[7,5],[2,6]]

	Output: 0

	Explanation: There are exactly three trios:

	1) [1,4,3] with degree 0.

	2) [2,5,6] with degree 2.

	3) [5,6,7] with degree 2.

.. topic:: Constraints

	2 <= n <= 400

	edges[i].length == 2

	1 <= edges.length <= n * (n-1) / 2

	1 <= ui, vi <= n

	ui != vi

	There are no repeated edges.

**Hint 1** Consider a trio with nodes u, v, and w. The degree of the trio is just degree(u) + degree(v) + degree(w) - 6. The -6 comes from subtracting the edges u-v, u-w, and v-w, which are counted twice each in the vertex degree calculation.

**Hint 2** To get the trios (u,v,w), you can iterate on u, then iterate on each w,v such that w and v are neighbors of u and are neighbors of each other.

**Approach**: given hint 1, the solution is pretty straightforward. We first construct an adjacent matrix, then do a n^3 traverse. To not exceed time limit, precompute the degrees when constructing the matrix.

.. code-block:: java

	public int minTrioDegree(int n, int[][] edges) {
        int[][] matrix = new int[n+1][n+1];
        int[] degree = new int[n+1];
        int min = Integer.MAX_VALUE;
    
        
        for (int[] edge : edges) {
            matrix[edge[0]][edge[1]] = 1;
            matrix[edge[1]][edge[0]] = 1;
            degree[edge[0]] += 1; 
            degree[edge[1]] += 1; 
        }
        
        for (int i=1; i<=n; i++) {
            if (degree[i] < 2) {
                continue;
            }
            
            for (int j=1; j<=n; j++) {
                if (matrix[i][j] == 1) {
                    for (int k=1; k<=n; k++) {
                        if (matrix[i][k] == 1 && matrix[j][k] == 1) {
                            min = Math.min(min, degree[i] + degree[j] + degree[k] - 6);
                        }
                    }
                }
            }
        }
        
        return (min==Integer.MAX_VALUE)?-1:min;
    }
