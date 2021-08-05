==============
Disjoint Set
==============

.. contents::
    :depth: 2

----------
Overview
----------

Consider a situation with a number of persons and following tasks to be performed on them.

Add a new friendship relation, i.e., a person x becomes friend of another person y.
Find whether individual x is a friend of individual y (direct or indirect friend)


.. topic:: Example

    We are given 10 individuals say,

        a, b, c, d, e, f, g, h, i, j

    Following are relationships to be added.
    
        a <-> b  
        
        b <-> d
        
        c <-> f
        
        c <-> i
        
        j <-> e
        
        g <-> j

    And given queries like whether a is a friend of d or not.

    We basically need to create following 4 groups and maintain a quickly accessible connection among group items:

        G1 = {a, b, d}

        G2 = {c, f, i}

        G3 = {e, g, j}

        G4 = {h}


Operations
------------

**Find** : Can be implemented by recursively traversing the parent array until we hit a node who is parent of itself.


.. code-block:: java

    // Finds the representative of the set  
    // that i is an element of
    int find(int i) 
    {
        // If i is the parent of itself
        if (parent[i] == i) 
        {
            // Then i is the representative of 
            // this set
            return i;
        }
        else 
        {
            // Else if i is not the parent of 
            // itself, then i is not the 
            // representative of his set. So we 
            // recursively call Find on its parent
            return find(parent[i]);
        }
    }

**Union**: It takes, as input, two elements. And finds the representatives of their sets using the find operation, and finally puts either one of the trees (representing the set) under the root node of the other tree, effectively merging the trees and the sets.


.. code-block:: java

    // Unites the set that includes i 
    // and the set that includes j
    void union(int i, int j) 
    {
        // Find the representatives
        // (or the root nodes) for the set
        // that includes i
        
        int irep = this.Find(i),

        // And do the same for the set 
        // that includes j    
        int jrep = this.Find(j);

        // Make the parent of i’s representative
        // be j’s  representative effectively 
        // moving all of i’s set into j’s set)
        this.Parent[irep] = jrep;
    }

-----------------------------------
128. Longest Consecutive Sequence
-----------------------------------

[Note: this is faster using HashSet]

Given an unsorted array of integers nums, return the length of the longest consecutive elements sequence.

You must write an algorithm that runs in O(n) time.

**Approach**: Add all the nums to a Hash Set. Then for each num, determine if it is the start of a sequence (n-1 is not in the set), then keep counting while n+1 is in the set.

 
.. topic:: Example 1

    Input: nums = [100,4,200,1,3,2]

    Output: 4

    Explanation: The longest consecutive elements sequence is [1, 2, 3, 4]. Therefore its length is 4.


.. topic:: Example 2

    Input: nums = [0,3,7,2,5,8,4,6,0,1]

    Output: 9
 

.. topic:: Constraints

    0 <= nums.length <= 105

    -109 <= nums[i] <= 109


.. code-block:: java

    public class DisjointSet {
            HashMap<Integer, Integer> parent;
            HashMap<Integer, Integer> size;
            
            public DisjointSet(int n){
                this.parent = new HashMap<>();     
                this.size = new HashMap<>();    
            }
            
            public void add(int i) {
                parent.put(i, i);
                size.put(i, 1);
            }

            // Finds the representative of the set that i is an element in
            // If i is not in the set, return null
            Integer find(int i) {
                if (parent.containsKey(i)) {
                    if (parent.get(i) == i) {
                        return i;
                    } else {
                        return find(parent.get(i));
                    }
                }
                
                return null;
            }
            
            // Combine the set i and set j. Make i's rep the new rep.
            void union(int i, int j) {
                int iRep = find(i);
                int jRep = find(j);
                if (iRep == jRep) {
                    return;
                }
                parent.put(jRep, iRep); 
                size.put(iRep, size.get(jRep)+size.get(iRep));
                size.put(jRep, 0);
            }
        }
        
        public int longestConsecutive(int[] nums) {
            DisjointSet set = new DisjointSet(nums.length);
            int rst = 0;
            
            for (int i : nums) {
                set.add(i);
                rst = 1;
            }
            
            for (int i : nums) {
                Integer iRep = set.find(i+1);
                if (iRep != null) {
                    set.union(iRep, i);
                    rst = Math.max(set.size.get(iRep), rst);
                }
            }
            
    //         for (int i : set.parent.keySet()) {
    //           System.out.println("key: " + i + " value: " + set.parent.get(i));
    //         }
            
    //         System.out.println("============");
            
    //         for (int i : set.size.keySet()) {
    //           System.out.println("key: " + i + " value: " + set.size.get(i));
    //         }
            
            return rst;
        }
